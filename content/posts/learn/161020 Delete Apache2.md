---
title: "Debian下卸载Apache2"
date: 2016-10-20
lastmod: 2022-12-03
author: ["沧海"]
tags: ["Apache"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
- 首先执行卸载命令
```c
apt-get --purge remove apache2 apache2-bin apache2-doc apache2-data apache2-utils -y
```
- 检查是否完全卸载
```c
dpkg -l | grep apache2
```
- 删除多余文件
```c
find /etc -name "*apache*" |xargs  rm -rf
```
```c
rm -rf /etc/libapache2-mod-jk
```
- 重启你的机器
搞定！