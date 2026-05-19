---
title: "Twikoo之Docker部署"
date: 2022-11-17
lastmod: 2022-11-17
author: ["沧海"]
tags: ["Docker", "Twikoo"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
- 启动容器
```c
docker run -d \
    --name twikoo \
    --restart always \
    --network host \
    -e TWIKOO_PORT=5003 \
    -e TWIKOO_THROTTLE=1000 \
    -v /opt/twikoo/data:/app/data \
    imaegoo/twikoo
```
```c
docker run -d \
    --name twikoo \
    --restart always \
    --network host \
    -e TWIKOO_PORT=5003 \
    -e TWIKOO_THROTTLE=1000 \
    -v /opt/twikoo/data:/app/data \
    imaegoo/twikoo:arm32v7
```
- 删除`/opt/twikoo/data/db.json.1`中的**ADMIN_PASS**及其值，即可重置管理面板密码