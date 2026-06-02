---
title: "重置Docker网络配置"
date: 2026-04-21
lastmod: 2026-04-21
author: ["沧海"]
tags: ["Docker"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

Docker 网络配置异常时，可能会出现容器无法访问外网、容器之间无法通信、网桥规则混乱等问题。如果确认是 Docker 网络元数据损坏，可以删除 Docker 的网络配置目录，让 Docker 重新生成默认网络。

> 操作前建议先确认没有正在运行的重要容器。这个方法会重置 Docker 网络配置，自定义 network 需要重新创建。

停止 Docker：

```bash
systemctl stop docker
```

删除 Docker 网络配置目录：

```bash
rm -rf /var/lib/docker/network
```

重新启动 Docker：

```bash
systemctl start docker
```

如果之前创建过自定义网络，需要重新执行对应的`docker network create`命令。容器本身和镜像数据不在`/var/lib/docker/network`目录里，一般不会因为这个操作被删除，但依赖自定义网络的容器可能需要重新连接网络或重建启动。
