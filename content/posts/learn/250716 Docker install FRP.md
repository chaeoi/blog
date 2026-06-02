---
title: "使用Docker安装FRP"
date: 2025-07-16
lastmod: 2025-07-16
author: ["沧海"]
tags: ["Docker", "FRP"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

`FRP`是一款内网穿透工具，分为服务端`frps`和客户端`frpc`。这里记录一下用 Docker 部署`frps`和`frpc`，以及 Windows 下把`frpc`注册为服务的配置。

服务端运行`frps`：

```bash
docker run -d \
  --name frps \
  --restart always \
  --network host \
  -v /opt/frp/frps.toml:/frp/frps.toml \
  fatedier/frps:v0.64.0 \
  -c /frp/frps.toml
```

服务端配置`/opt/frp/frps.toml`：

```toml
bindPort = 5031
auth.token = "password"
```

客户端运行`frpc`：

```bash
docker run -d \
  --name frpc \
  --restart always \
  --network host \
  -v /opt/frp/frpc.toml:/frp/frpc.toml \
  fatedier/frpc:v0.64.0 \
  -c /frp/frpc.toml
```

客户端配置`/opt/frp/frpc.toml`：

```toml
serverAddr = "1.2.3.4"
serverPort = 5031
auth.token = "password"

[[proxies]]
name = "ssh"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = 6000
```

这个配置会把客户端机器的`22`端口映射到服务端的`6000`端口。连接时访问服务端：

```bash
ssh user@1.2.3.4 -p 6000
```

需要注意，`auth.token`要改成自己的密码，客户端和服务端必须保持一致。`serverAddr`也需要改成服务端公网 IP 或域名。

如果是在 Windows 上运行`frpc.exe`，可以配合 WinSW 注册成系统服务，这部分单独记录。
