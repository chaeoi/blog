---
title: "使用Docker安装下载利器qBittorrent"
date: 2025-02-12
lastmod: 2025-02-12
author: ["沧海​"]
tags: ["qBittorrent"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
cover:
  image: "https://cdn.canghai.org/images/25021901.webp"
---
- **类似 μTorrent 的精致用户界面**
- **无广告**
- **高度集成且可扩展的搜索引擎**
- **支持 RSS 订阅及高级下载过滤（包括正则表达式）**
- **支持多种 BitTorrent 扩展协议**
- **通过网页端实现远程控制（基于 AJAX 开发）**
- **顺序下载（按顺序下载文件）**
- **对种子、追踪器和节点的精细控制**
- **带宽调度器**
- **种子创建工具**
- **IP 过滤（兼容 eMule 和 PeerGuardian 格式）**
- **支持 IPv6**
- **支持 UPnP / NAT-PMP 端口转发**
- **全平台可用**：Windows、Linux、macOS、FreeBSD、OS/2
- **支持约 70 种语言**

建议Docker部署，这里采用`LinuxServer`打包的镜像，`armhf`已停止支持，可通过使用旧版本解决
```docker
docker run -d \
    --name qbittorrent \
    --restart always \
    --network host \
    -e PUID=1000 \
    -e PGID=1000 \
    -e WEBUI_PORT=5060 \
    -e TZ=Asia/Shanghai \
    -v /opt/qbittorrent/appdata:/config \
    -v /opt/qbittorrent/downloads:/downloads \
    linuxserver/qbittorrent:latest
```
```docker
docker run -d \
    --name qbittorrent \
    --restart always \
    --network host \
    -e PUID=1000 \
    -e PGID=1000 \
    -e WEBUI_PORT=5060 \
    -e TZ=Asia/Shanghai \
    -v /opt/qbittorrent/appdata:/config \
    -v /opt/qbittorrent/downloads:/downloads \
    linuxserver/qbittorrent:arm32v7-4.5.3
```