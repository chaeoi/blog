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
- 创建Cloudreve配置文件，以及数据库文件
```c
mkdir -p /opt/cloudreve/data && touch /opt/cloudreve/data/conf.ini && touch /opt/cloudreve/data/cloudreve.db
```
- 启动容器，配置离线下载，需要监听其他端口，采用Host模式，默认端口5212，可在conf.ini修改，更多配置详情见[官方文档](https://docs.cloudreve.org/getting-started/config)
```c
docker run -d \
    --name cloudreve \
    --restart always \
    --mount type=bind,source=/opt/cloudreve/data/conf.ini,target=/cloudreve/conf.ini \
    --mount type=bind,source=/opt/cloudreve/data/cloudreve.db,target=/cloudreve/cloudreve.db \
    --network host \
    -v /opt/cloudreve/uploads:/cloudreve/uploads \
    -v /opt/cloudreve/data/avatar:/cloudreve/avatar \
    -v /opt/aria2/downloads:/downloads \
    cloudreve/cloudreve:latest
```