---
title: "Docker开启IPv6"
date: 2026-04-16
lastmod: 2026-04-16
author: ["沧海"]
tags: ["Docker"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

Docker 默认的 bridge 网络不一定开启 IPv6，如果容器需要使用 IPv6，可以参考[Docker官方文档](https://docs.docker.com/engine/daemon/ipv6/)修改 Docker daemon 配置。

编辑`/etc/docker/daemon.json`：

```json
{
  "ipv6": true,
  "fixed-cidr-v6": "2001:db8:1::/64"
}
```

保存后重启 Docker：

```bash
systemctl restart docker
```

需要注意，`2001:db8::/32`是文档示例地址段，实际使用时应替换成自己的 IPv6 地址段。如果只是内网容器通信，也可以使用 ULA 地址段。

重启后可以查看默认网络：

```bash
docker network inspect bridge
```

如果是在 Docker Compose 中创建自定义 bridge 网络，需要给 network 单独开启 IPv6：

```yaml
networks:
  ipv6:
    driver: bridge
    enable_ipv6: true
    ipam:
      config:
        - subnet: "2001:db8::/64"
```

如果容器需要从公网 IPv6 访问，还需要确认宿主机本身已经有可用的 IPv6，防火墙和上游路由也允许对应流量。
