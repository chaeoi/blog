---
title: "重置Winsock目录"
date: 2020-12-29
lastmod: 2020-12-29
author: ["沧海"]
tags: ["Windows"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
有时候电脑会因为一些莫名其妙的问题无法上网，如以下情况：
- 安装广告软件、使用**VPN**、修改**防火墙**后无法联网
- 无法**访问任何网页**或只能访问某些网页
- 出现与网络相关问题的**弹出错误窗口**
- 由于**注册表错误**，没有网络连接
- 发生**DNS**查找问题
- 无法续订网络适配器的**IP**地址或其他一些**DHCP**错误
- 没有连接消息的网络连接问题

这时以管理员身份打开**CMD**，运行以下命令即可解决问题
```c
netsh winsock reset
```