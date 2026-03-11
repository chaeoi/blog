---
title: "删除Win10连接过的网络名"
date: 2020-12-18
lastmod: 2020-12-20
author: ["沧海"]
tags: ["Windows"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
路由器刷固件后，SSID不变。笔记本再连接就会从openwrt变成openwrt2。强迫症表示完全受不了！可以通过清理注册表解决。
- **win+R** 输入`regedit`进入注册表编辑器。
- 删除下列目录中所有子项
- 重启电脑。
```c
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Profiles
```
```c
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged
```
终于通过清理注册表后，使其恢复到默认状态。