---
title: "解决下载文件夹变成 “Downloads”"
date: 2024-04-16
lastmod: 2024-04-16
author: ["沧海"]
tags: ["Win10"]
description: "通过重建 Downloads 目录的 desktop.ini，把资源管理器里突然变成英文的“Downloads”恢复成中文“下载”。"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

我碰到“下载”文件夹突然显示成 `Downloads` 时，最后发现不是目录真的被改名了，而是本地化信息丢了。修起来并不复杂，本质上就是给这个目录重新写一份正确的 `desktop.ini`。

#### 先理解这类问题的本质

这件事看起来像是“文件夹名字被系统改了”，实际上多数情况下只是资源管理器没有再按本地化规则去显示中文名称。

也就是说：

- 实际目录路径通常没变
- 文件夹本身往往也没有真的重命名
- 丢掉的是目录对应的本地化元数据

所以我处理这个问题时，不会先去重命名文件夹，而是直接修 `desktop.ini`。

#### 利用Attrib修改属性

编辑前先去除文件属性

```cmd
attrib -h -r -s -a desktop.ini
```
编辑完成后重新添加文件属性
```cmd
attrib +h +r +s +a desktop.ini
```
---

#### 附上一些常见的Desktop.ini内容

- Contacts 联系人
```ini
[.ShellClassInfo]
LocalizedResourceName=@%CommonProgramFiles%\system\wab32res.dll,-10100
InfoTip=@%CommonProgramFiles%\system\wab32res.dll,-10200
IconResource=%SystemRoot%\system32\imageres.dll,-181
```

- Desktop 桌面
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21769
IconResource=%SystemRoot%\system32\imageres.dll,-18
```

- Documents 文档
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21770
IconResource=%SystemRoot%\system32\imageres.dll,-112
IconFile=%SystemRoot%\system32\shell32.dll
IconIndex=-235
```

- Download 下载
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21798
IconResource=%SystemRoot%\system32\imageres.dll,-184
```

- Favorites 收藏夹
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21796
IconResource=%SystemRoot%\system32\imageres.dll,-115
IconFile=%SystemRoot%\system32\shell32.dll
IconIndex=-173
```

- Links 链接
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21810
IconResource=%SystemRoot%\system32\imageres.dll,-185
DefaultDropEffect=4

[LocalizedFileNames]
RecentPlaces.lnk=@shell32.dll,-37217
Desktop.lnk=@shell32.dll,-21769
Downloads.lnk=@shell32.dll,-21798
```

- Music 音乐
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21790
InfoTip=@%SystemRoot%\system32\shell32.dll,-12689
IconResource=%SystemRoot%\system32\imageres.dll,-108
IconFile=%SystemRoot%\system32\shell32.dll
IconIndex=-237
```

- OneDrive
```ini
[.ShellClassInfo]
IconResource=C:\Users\ZHCS\AppData\Local\Microsoft\OneDrive\OneDrive.exe,1
```
- Pictures 图片
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21779
InfoTip=@%SystemRoot%\system32\shell32.dll,-12688
IconResource=%SystemRoot%\system32\imageres.dll,-113
IconFile=%SystemRoot%\system32\shell32.dll
IconIndex=-236
```

- Save Games 保存的游戏
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21814
IconResource=%SystemRoot%\system32\imageres.dll,-186
```

- Searches 搜索
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-9031
IconResource=%SystemRoot%\system32\imageres.dll,-18

[LocalizedFileNames]
Indexed Locations.search-ms=@searchfolder.dll,-32820
Everywhere.search-ms=@searchfolder.dll,-32822
```

- Videos 视频
```ini
[.ShellClassInfo]
LocalizedResourceName=@%SystemRoot%\system32\shell32.dll,-21791
InfoTip=@%SystemRoot%\system32\shell32.dll,-12690
IconResource=%SystemRoot%\system32\imageres.dll,-189
IconFile=%SystemRoot%\system32\shell32.dll
IconIndex=-238
```
