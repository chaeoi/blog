---
title: "在 VHDX 中安装 Windows 并添加启动项"
date: 2026-11-07
lastmod: 2026-11-07
author: ["沧海"]
tags: ["虚拟机"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

VHDX 原生启动可以把一套新的 Windows 安装在 `.vhdx` 文件里，开机时通过 Windows Boot Manager 选择进入。系统运行时直接使用电脑的硬件，原来的 Windows 也可以继续保留，适合准备一套独立的测试环境。

## 创建 VHDX

本文以已有 Windows、采用 UEFI 启动的电脑为例，复用现有 EFI 系统分区。先在资源管理器中创建 `D:\NativeBoot` 文件夹，用于保存新系统的 VHDX 文件。

按 `Win + X` 打开“磁盘管理”，选择菜单中的“操作 → 创建 VHD”。在创建窗口中填写：

- 位置：`D:\NativeBoot\Win11.vhdx`。
- 虚拟硬盘大小：`512`，单位选择 **GB**。
- 虚拟硬盘格式：**VHDX**。
- 虚拟硬盘类型：**动态扩展**。

点击确定，等待创建完成。新建的虚拟磁盘会附加到当前系统；如果以后需要重新挂载，可以在“操作 → 附加 VHD”中选择这个文件。[微软磁盘管理文档](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/manage-virtual-hard-disks)介绍了对应的界面操作。

在下方磁盘列表中找到刚创建的虚拟磁盘。如果显示“未初始化”，右键左侧的磁盘名称，选择“初始化磁盘”，分区形式选择 **GPT**。然后右键这块虚拟磁盘的“未分配”区域，选择“新建简单卷”，使用可用空间，分配盘符 `F:`，按 **NTFS** 快速格式化。

初始化和格式化时，确认选中的是这个新建的虚拟磁盘。完成后，资源管理器中应能看到 `F:`，后面就把 Windows 部署到这里。VHDX 内不需要另外创建 EFI 分区，本次启动复用电脑已有的 EFI 系统分区。

这里的 `F:` 是 VHDX 内部的分区，`D:` 是保存 VHDX 文件的物理磁盘分区。分配盘符前确认 `F:` 未被其他设备占用；如果改用其他盘符，后面的部署命令也要同步修改。

VHDX 应保存在本地基本磁盘上。本文使用 NTFS 分区，存放位置不启用压缩或文件加密，所在卷也不使用 BitLocker 加密。动态扩展的 VHDX 会随写入数据增长，但原生启动时，存放它的分区仍要有足够空间供它扩展到最大容量，并留出分页文件空间。具体限制见[微软 VHDX 原生启动文档](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/deploy-windows-on-a-vhd--native-boot?view=windows-11)。

## 部署 Windows

挂载 Windows ISO。本文假设镜像文件位于 `E:\sources\install.wim`，实际路径按挂载后的盘符修改。

后续部署与引导命令都在**管理员命令提示符（CMD）**中执行。Windows Terminal 中选择“命令提示符”配置文件。修改引导前，先备份当前 BCD：

```cmd
bcdedit /export D:\NativeBoot\BCD-before-install
```

备份文件保存在 `D:\NativeBoot`，之后如果需要回退引导配置，可以使用这份备份。

先查看安装镜像中包含哪些 Windows 版本：

```cmd
dism /Get-WimInfo /WimFile:E:\sources\install.wim
```

`/WimFile` 指定镜像路径。输出会列出每个映像的索引、名称和描述，记下需要安装的版本对应的索引。本文使用 `3`，它不固定对应专业版或其他版本，更换 ISO 后应重新查询。

确认索引后，将系统映像写入 VHDX 内部的 `F:` 分区：

```cmd
dism /Apply-Image /ImageFile:E:\sources\install.wim /Index:3 /ApplyDir:F:\
```

`/ImageFile` 指定源镜像，`/Index:3` 选择其中的第 3 个映像，`/ApplyDir:F:\` 指定部署位置。如果安装介质中只有 `install.esd` 或分卷的 `install.swm`，需要按实际文件调整镜像路径或参数。

等待 DISM 明确报告操作成功，再检查系统目录：

```cmd
dir F:\Windows
```

这里应能看到新系统的 Windows 目录。部署中途报错时，应先解决问题，再继续添加引导。

## 添加启动项

使用刚写入的系统文件，向电脑现有的启动环境添加这套 Windows：

```cmd
bcdboot F:\Windows /d /p
```

`F:\Windows` 是启动文件的来源路径。`/d` 保留原来的默认 Windows，`/p` 保留 Windows Boot Manager 在 UEFI 启动顺序中的现有位置；如果该固件项不存在，`/p` 会创建它并放到首位。参数说明见[微软 BCDBoot 文档](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/bcdboot-command-line-options-techref-di)。

添加成功后，查看操作系统加载器：

```cmd
bcdedit /enum osloader /v
```

`osloader` 筛选操作系统加载器，`/v` 显示完整 GUID。找到指向 `Win11.vhdx` 的记录，核对 `device` 和 `osdevice` 中的路径，例如 `vhd=[D:]\NativeBoot\Win11.vhdx`，并记下该项的 GUID。原系统与新系统可能同名，不能只根据“Windows 11”判断。

为了在启动菜单中区分，可以给新项改名。把下面的 `{新系统GUID}` 替换为刚查到的完整 GUID，保留两侧花括号：

```cmd
bcdedit /set {新系统GUID} description "Windows 11 VHDX"
bcdedit /timeout 10
```

第一条修改菜单中的显示名称，第二条把菜单等待时间设为 10 秒。保存正在处理的文件，然后重启，在菜单中选择“Windows 11 VHDX”，完成首次初始化。如果初始化过程中再次出现启动菜单，继续选择这套 VHDX 系统。

## 设置默认系统

如果希望以后默认进入 VHDX 系统，先在这套新 Windows 中打开管理员 CMD，查看当前项：

```cmd
bcdedit /enum {current} /v
```

`{current}` 代表当前正在运行的 Windows。在原系统中执行，它就指向原系统；要将 VHDX 系统设为默认，应先进入 VHDX 系统。[微软的标识符说明](https://learn.microsoft.com/en-us/windows-hardware/drivers/devtest/boot-options-identifiers)区分了当前项与默认项。

确认当前项正确后，将它设为默认，再查看默认项：

```cmd
bcdedit /default {current}
bcdedit /enum {default} /v
```

确认默认项正确、且新系统可以正常重启后，再取消启动菜单等待：

```cmd
bcdedit /timeout 0
```

`0` 表示不等待用户选择，直接进入默认系统，不会删除其他启动项。需要重新显示菜单时，在能够正常进入的 Windows 中执行 `bcdedit /timeout 10` 即可。

从 VHDX 启动后，里面的 Windows 分区可能显示为 `C:`，不再是部署时的 `F:`。后续以当前环境实际显示的盘符为准。引导已经记录了 VHDX 的路径，建立引导后应保持文件的位置和名称。

## 清理启动项

以后不用这套 VHDX 系统，或者反复部署留下了多余记录，再清理对应的 BCD 启动项。要移除 VHDX 系统时，先回到准备保留的原 Windows。

在原系统的管理员 CMD 中备份配置，将当前系统设为默认，并列出加载器：

```cmd
bcdedit /export D:\NativeBoot\BCD-before-cleanup
bcdedit /default {current}
bcdedit /enum osloader /v
```

记录较多时，可以用名称或完整 GUID 辅助定位。下面的 GUID 仅为示例，实际使用时替换为本机记录：

```cmd
bcdedit /enum all /v | findstr /i /l /c:"Win11" /c:"e06b2941-d1f5-11f1-94c7-8e8626714c8b"
```

`/i` 忽略大小写，`/l` 按普通文本匹配，`/c:` 将后面的内容作为一个完整搜索字符串。多个 `/c:` 之间是“或”的关系；原先把 GUID 按空格拆开的写法，也会分别匹配每个片段。具体规则见[微软 findstr 文档](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/findstr)。

`findstr` 只输出匹配行，名称和 GUID 可能来自不同记录。找到 GUID 后，用下面的命令查看完整对象，并与当前项、默认项核对。`{xxx}` 替换为待删除项的完整 GUID：

```cmd
bcdedit /enum {xxx} /v
bcdedit /enum {current} /v
bcdedit /enum {default} /v
```

根据 `device` 和 `osdevice` 确认它指向要移除的系统，并且不是当前项、默认项或仍需使用的恢复项，再执行删除：

```cmd
bcdedit /delete {xxx} /cleanup
bcdedit /enum osloader /v
bcdedit /enum {bootmgr}
```

`/delete` 删除指定启动项，`/cleanup` 清理它在启动显示顺序等位置的相关引用，后两条命令用于检查清理结果。这一步不会删除 VHDX 文件；确认原系统正常启动、相关启动项也已移除后，才考虑删除不再需要的 `.vhdx` 文件。

如果发现误删，并且还能进入 Windows，可以导入清理前的备份：

```cmd
bcdedit /import D:\NativeBoot\BCD-before-cleanup
```

导入会替换当前系统 BCD，备份之后的其他引导改动也会一起回退。备份路径按当前系统中的实际盘符修改。

## 命令汇总

先通过磁盘管理创建好 `D:\NativeBoot\Win11.vhdx`，容量为 **512 GB**，并将内部的 NTFS 卷分配为 `F:`。下面按执行阶段集中列出命令；确认每一步成功后再继续。

### 部署与添加引导（原系统）

备份 BCD，查询 WIM 中的版本：

```cmd
bcdedit /export D:\NativeBoot\BCD-before-install
dism /Get-WimInfo /WimFile:E:\sources\install.wim
```

确认需要的版本对应索引为 `3` 后部署，其他索引按实际结果修改：

```cmd
dism /Apply-Image /ImageFile:E:\sources\install.wim /Index:3 /ApplyDir:F:\
```

DISM 报告成功后执行：

```cmd
dir F:\Windows
bcdboot F:\Windows /d /p
bcdedit /enum osloader /v
```

查到新系统的 GUID 后，替换 `{新系统GUID}`，再执行：

```cmd
bcdedit /set {新系统GUID} description "Windows 11 VHDX"
bcdedit /timeout 10
```

随后重启，选择新系统完成初始化。

### 默认启动设置（VHDX 系统）

```cmd
bcdedit /enum {current} /v
bcdedit /default {current}
bcdedit /enum {default} /v
```

确认新系统可以正常重启后，取消菜单等待：

```cmd
bcdedit /timeout 0
```

### 启动项清理（原系统）

备份、设置默认项并查看记录：

```cmd
bcdedit /export D:\NativeBoot\BCD-before-cleanup
bcdedit /default {current}
bcdedit /enum osloader /v
bcdedit /enum all /v | findstr /i /l /c:"Win11" /c:"e06b2941-d1f5-11f1-94c7-8e8626714c8b"
```

替换示例 GUID 和 `{xxx}`，核对待删除项：

```cmd
bcdedit /enum {xxx} /v
bcdedit /enum {current} /v
bcdedit /enum {default} /v
```

确认无误后删除并检查：

```cmd
bcdedit /delete {xxx} /cleanup
bcdedit /enum osloader /v
bcdedit /enum {bootmgr}
```

仅在需要恢复清理前配置时执行：

```cmd
bcdedit /import D:\NativeBoot\BCD-before-cleanup
```
