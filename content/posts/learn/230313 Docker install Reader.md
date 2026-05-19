---
title: "Reader之Docker部署"
date: 2023-03-13
lastmod: 2023-03-13
author: ["沧海"]
tags: ["Docker", "Reader"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
网页端开源阅读📖。    
官方提供的镜像有两种，**常规镜像**基于`Alpine`，占用空间较小，但是占用内存多；**openj9镜像**基于`Ubuntu`，占用空间较大，但是占用内存小。可根据自身情况选择。

```c
docker run -d \
    --name=reader \
    --restart always \
    --network host \
    -e READER_SERVER_PORT=5010 \
    -e SPRING_PROFILES_ACTIVE=prod \
    -e READER_APP_SECURE=true \
    -e READER_APP_SECUREKEY=123456 \
    -e READER_APP_INVITECODE=123456 \
    -v /opt/reader/logs:/logs \
    -v /opt/reader/storage:/storage \
    hectorqin/reader:latest
```
```c
docker run -d \
    --name=reader \
    --restart always \
    --network host \
    -e READER_SERVER_PORT=5010 \
    -e SPRING_PROFILES_ACTIVE=prod \
    -e READER_APP_SECURE=true \
    -e READER_APP_SECUREKEY=123456 \
    -e READER_APP_INVITECODE=123456 \
    -v /opt/reader/logs:/logs \
    -v /opt/reader/storage:/storage \
    hectorqin/reader:openj9-latest
```
