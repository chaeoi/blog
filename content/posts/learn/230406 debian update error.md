---
title: "解决Debian 11更新错误"
date: 2023-04-06
lastmod: 2023-04-06
author: ["沧海​"]
tags: ["Linux"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

新装的系统执行`apt-get update`一直报错，详情如下：
```cmd
Release file for http://security.debian.org/dists/bullseye-security/InRelease is expired. Updates for this repository will not be applied.
```
尝试过~~校正时区~~和~~同步时间~~都没有效果，排查之后将源修改即可解决问题

- 修改前
```cmd
deb http://deb.debian.org/debian bullseye main contrib non-free
#deb-src http://deb.debian.org/debian bullseye main contrib non-free

deb http://deb.debian.org/debian bullseye-updates main contrib non-free
#deb-src http://deb.debian.org/debian bullseye-updates main contrib non-free

deb http://deb.debian.org/debian bullseye-backports main contrib non-free
#deb-src http://deb.debian.org/debian bullseye-backports main contrib non-free

deb http://security.debian.org/ bullseye-security main contrib non-free
#deb-src http://security.debian.org/ bullseye-security main contrib non-free
```

- 修改后
```cmd
deb http://deb.debian.org/debian/ bullseye main contrib non-free
#deb-src http://deb.debian.org/debian/ bullseye main contrib non-free

deb http://deb.debian.org/debian/ bullseye-updates main contrib non-free
#deb-src http://deb.debian.org/debian/ bullseye-updates main contrib non-free

deb http://deb.debian.org/debian/ bullseye-backports main contrib non-free
#deb-src http://deb.debian.org/debian/ bullseye-backports main contrib non-free

deb http://deb.debian.org/debian-security/ bullseye-security main contrib non-free
#deb-src http://deb.debian.org/debian-security/ bullseye-security main contrib non-free
```