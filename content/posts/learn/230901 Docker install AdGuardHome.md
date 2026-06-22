---
title: "AdGuardHome之Docker部署"
date: 2023-09-01
lastmod: 2023-09-01
author: ["沧海"]
tags: ["AdGuardHome"]
description: "使用Docker部署AdGuardHome，守护网络安全"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
<font size="5">尽情享受安全且无⁠广⁠告⁠的互联网体验。即刻获取保护！</font>

- 拦截所有种类的广告
- 移除烦人的网络元素
- 节省流量并加速页面载入
- 工作于浏览器与应用程序
- 保持网站功能和外观
```c
docker run -d \
    --name adguardhome \
    --restart always \
    --log-opt max-size=1m \
    --network host \
    -v /opt/adguardhome/work:/opt/adguardhome/work \
    -v /opt/adguardhome/conf:/opt/adguardhome/conf \
    adguard/adguardhome
```