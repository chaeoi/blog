---
title: "使用Docker安装OpenClaw"
date: 2026-03-26
lastmod: 2026-03-26
author: ["沧海"]
tags: ["Docker", "OpenClaw"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

这里记录一下用 Docker 运行`OpenClaw`。第一次运行需要先执行`onboard`完成初始化，之后再以后台方式启动`gateway`。

先执行初始化：

```bash
docker run -it --rm \
  --user root \
  -e HOME=/home/node \
  -e NODE_ENV=production \
  -e TERM=xterm-256color \
  -e XDG_CONFIG_HOME=/home/node/.openclaw \
  -e PATH=/home/linuxbrew/.linuxbrew/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
  -v /opt/openclaw:/home/node/.openclaw \
  -v /opt/openclaw/workspace:/home/node/.openclaw/workspace \
  ghcr.io/openclaw/openclaw:latest \
  node dist/index.js onboard
```

初始化命令使用`--rm`，执行完成后容器会自动删除，但配置会保存在宿主机的`/opt/openclaw`目录。

后台启动服务：

```bash
docker run -d \
  --name openclaw \
  --restart always \
  --network host \
  --user root \
  -e HOME=/home/node \
  -e NODE_ENV=production \
  -e TERM=xterm-256color \
  -e XDG_CONFIG_HOME=/home/node/.openclaw \
  -e PATH=/home/linuxbrew/.linuxbrew/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
  -v /opt/openclaw:/home/node/.openclaw \
  -v /opt/openclaw/workspace:/home/node/.openclaw/workspace \
  ghcr.io/openclaw/openclaw:latest \
  node dist/index.js gateway --bind lan --port 5099
```

其中：

- `/opt/openclaw`用于保存 OpenClaw 配置
- `/opt/openclaw/workspace`用于保存工作目录
- `--network host`表示直接使用宿主机网络
- `--bind lan --port 5099`表示在局域网监听`5099`端口

启动后可以查看容器状态：

```bash
docker ps | grep openclaw
```

查看日志：

```bash
docker logs -f openclaw
```

如果需要重新初始化，可以先停止并删除后台容器，再执行`onboard`命令：

```bash
docker stop openclaw
docker rm openclaw
```
