---
title: "Aria2之Docker部署"
date: 2022-12-16
lastmod: 2022-12-16
author: ["沧海"]
tags: ["Aria2"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
Aria2 是目前最强大的全能型**下载工具**，它支持 BT、磁力、HTTP、FTP 等下载协议，常用做**离线下载**的服务端。这里采用了[P3TERX](https://github.com/P3TERX)大佬的[Aria2 Pro](https://github.com/P3TERX/Aria2-Pro-Docker)项目。
- 启动容器，为了方便使用**IPV6**，网络采用**Host模式**
```c
docker run -d \
    --name aria2 \
    --restart always \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e IPV6_MODE=true \
    -e RPC_SECRET=E7t3vdHKLPyUa8a2 \
    -e RPC_PORT=5006 \
    -e LISTEN_PORT=5007 \
    -v /opt/aria2/config:/config \
    -v /opt/aria2/downloads:/downloads \
    p3terx/aria2-pro
```
- 更多进阶使用方法，见项目[中文官网](https://p3terx.com/archives/docker-aria2-pro.html)