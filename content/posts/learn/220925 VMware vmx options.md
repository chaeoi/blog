---
title: "VMware常见的配置参数"
date: 2022-09-25
lastmod: 2022-09-25
author: ["沧海"]
tags: ["VMware"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

现在我用`VMware`创建`Win10`、`Linux`虚拟机测试软件的场景越来越多，很多优化和兼容设置其实不需要在图形界面里点，直接改`.vmx`就够了。我的习惯一直是先找到目标虚拟机的安装目录，然后用直接改对应的`.vmx`文件。

#### 1. 禁止内存映射

虚拟机运行时，安装目录里经常会生成一个和虚拟机内存差不多大小的文件，关机后又消失。这个文件反复生成、反复删除，我一般会关掉它：

```ini
mainMem.useNamedFile = "FALSE"
```

这条对我来说最直接的意义，就是别再让虚拟机目录每次都莫名其妙多出来一大块文件，同时会减少一层映射，提升性能。

#### 2. 解决一堆 ATA Channel 设备的问题

运行`Windows`虚拟机时，右下角“安全删除硬件”里有时会出现一串`ATA Channel X`，看着很烦。碰到这种情况，我通常就加：

```ini
devices.hotplug = "false"
```

加完以后，这一类热插拔设备提示通常就不会再冒出来。

#### 3. 苹果虚拟机

如果我要装`macOS`，除了先装`unlocker`，还会补下面这一行：

```ini
smc.version = "0"
```

这不是通用参数，只在这个场景下用。

#### 4. Linux 系统下开启 3D 加速兼容

如果是`Linux`虚拟机要开`3D`，但默认驱动兼容性不够，我会加：

```ini
mks.gl.allowUnsupportedDrivers = "TRUE"
```

这条的意思很直接，就是允许`VMware`接受默认不在支持列表里的图形驱动组合。

#### 5. 不想再看到 Scoreboard 文件

有时候虚拟机目录里会出现很多`Scoreboard`文件，这类文件本质上是`VMX`进程生成的统计信息文件。如果我不想看到，就会加：

```ini
vmx.scoreboard.enabled = "FALSE"
```

#### 6. 反虚拟机检测

有些程序只要检测到自己运行在虚拟机里，就会限制功能，甚至直接不运行。这个时候我通常会把下面这一整组参数写进去：

```ini
isolation.tools.getPtrLocation.disable = "TRUE"
isolation.tools.setPtrLocation.disable = "TRUE"
isolation.tools.setVersion.disable = "TRUE"
isolation.tools.getVersion.disable = "TRUE"
monitor_control.disable_directexec = "TRUE"
monitor_control.disable_chksimd = "TRUE"
monitor_control.disable_ntreloc = "TRUE"
monitor_control.disable_selfmod = "TRUE"
monitor_control.disable_reloc = "TRUE"
monitor_control.disable_btinout = "TRUE"
monitor_control.disable_btmemspace = "TRUE"
monitor_control.disable_btpriv = "TRUE"
monitor_control.disable_btseg = "TRUE"
monitor_control.restrict_backdoor = "TRUE"
monitor_control.virtual_rdtsc = "false"
```

只写上面这些还不算完。如果我要继续往下做，还会把显卡驱动暴露出来的名字一起改掉。

先安装`VMware Tools`，把显卡驱动备份出来，然后打开对应的`.inf`文件，找到下面这些字段：

```ini
DiskID = "VMware Tools"
CompanyName = "VMware, Inc."
SVGA = "VMware SVGA II"
```

然后把它们改成类似下面这样：

```ini
DiskID = "NVIDIA Windows Driver Library Installation"
CompanyName = "NVIDIA"
SVGA = "GeForce GTX 660"
```

改完再重新安装。这样做的思路很简单，就是把`VMware SVGA`这种一眼能看出虚拟机身份的图形特征伪装成普通显卡信息。

#### 7. 关闭调试信息

如果虚拟机已经跑得很稳定了，我一般还会再加：

```ini
vmx.buildType = "release"
```

这一条的目的就是把构建类型改成`release`。我自己把它当成一个小优化项，不是什么决定性开关，但机器稳定以后加上没坏处。

#### 8. 关于日志文件

`vmware.log`这个虚拟机日志文件本身很有用，而且也没法直接关闭。另外还有个常见文件叫`mksSandbox.log`。它通常和`3D`图形有关，只要虚拟机启用了下面这个条件：

```ini
mks.enable3d = "TRUE"
```

也就是在图形设置里勾选了“加速 3D 图形”，那么`mksSandbox.log`就会出现；如果不启用这一项，它一般也不会继续生成。

---

**禁止内存映射**、**关闭虚拟硬件热插拔**、**强制允许 3D 加速运行**、**关闭CPU执行跟踪**、**修改内核构建类型**
```ini
mainMem.useNamedFile = "FALSE"
devices.hotplug = "false"
mks.gl.allowUnsupportedDrivers = "TRUE"
vmx.scoreboard.enabled = "FALSE"
vmx.buildType = "release"
```