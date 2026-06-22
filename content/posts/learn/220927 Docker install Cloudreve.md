---
title: "Cloudreve之Docker部署"
date: 2022-09-27
lastmod: 2022-09-27
author: ["沧海"]
tags: ["Cloudreve"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
启动容器，配置离线下载，需要监听其他端口，采用Host模式，默认端口5212，可在conf.ini修改，更多配置详情见[官方文档](https://docs.cloudreve.org/getting-started/config)
```c
docker run -d \
    --name cloudreve \
    --restart always \
    --network host \
    -v /opt/cloudreve/data:/cloudreve/data \
    -v /opt/aria2/downloads:/downloads \
    cloudreve/cloudreve:latest
```