---
title: "通过IPV6拉取Docker镜像"
date: 2023-02-16
lastmod: 2023-02-16
author: ["沧海"]
tags: ["Docker"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
现在我们很高兴为**Docker Hub**引入**Beta IPv6**支持！这意味着，如果您使用的是仅限**IPv6**的网络，您现在可以选择直接使用注册表，而无需NAT64网关。
- 在要拉取的镜像前添加以下代码即可
```c
registry.ipv6.docker.com/
```
- 示例
```c
docker run -d \
    --name vaultwarden \
    --restart always \
    --network host \
    -e ROCKET_PORT=5002 \
    -v /opt/vaultwarden/data:/data \
    registry.ipv6.docker.com/vaultwarden/server:latest
```