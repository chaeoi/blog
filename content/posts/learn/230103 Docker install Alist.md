---
title: "Alist之Docker部署"
date: 2023-01-03
lastmod: 2025-06-23
author: ["沧海"]
tags: ["Alist"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
Alist是一个支持多种存储的文件列表程序，项目地址见[Github](https://github.com/AlistGo/alist)
```c
docker run -d \
    --name alist \
    --restart always \
    --network host \
    -e PUID=0 \
    -e PGID=0 \
    -e UMASK=022 \
    -v /opt/alist:/opt/alist/data \
    xhofe/alist:latest
```