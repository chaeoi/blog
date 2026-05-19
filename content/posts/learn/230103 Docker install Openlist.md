---
title: "Openlist之Docker部署"
date: 2023-01-03
lastmod: 2025-06-23
author: ["沧海"]
tags: ["Docker", "Openlist"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
Openlist是一个支持多种存储的文件列表程序，项目地址见[Github](https://github.com/OpenListTeam/OpenList)
```c
docker run -d \
    --name openlist \
    --restart always \
    --network host \
    --user $(id -u):$(id -g) \
    -e UMASK=022 \
    -v /opt/openlist:/opt/openlist/data \
    openlistteam/openlist:latest
```