---
title: "极点云Cpolar安装及配置指南"
date: 2026-07-26
lastmod: 2026-07-26
author: ["沧海"]
tags: ["cpolar"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

`cpolar`是一款内网穿透工具，可以把内网中的 HTTP、HTTPS、TCP 等服务映射到公网。这里记录一下 Docker 部署、配置文件以及 Windows 下的安装方法。

## Cpolar配置文件

Cpolar使用`YAML`格式的配置文件，Docker部署时可以保存在宿主机的`/opt/cpolar/cpolar.yml`。

```bash
mkdir -p /opt/cpolar
vi /opt/cpolar/cpolar.yml
```

推荐配置如下：

```yaml
client_dashboard_addr: 0.0.0.0:5025

tunnels:
  website:
    addr: 8080
    proto: http
  ssh:
    addr: 22
    proto: tcp
```

其中`client_dashboard_addr`是Web UI监听地址，这里改为`5025`端口。如果只允许本机访问，可以改成`127.0.0.1:5025`。`website`将本机`8080`端口映射到公网，`ssh`将本机`22`端口映射到公网。

`probezy/cpolar`镜像没有提供修改端口的环境变量，镜像中的`PATH`、`REFRESHED_AT`和`CPOLAR_VERSION`都不是Cpolar配置项，所以端口和隧道需要在`cpolar.yml`中修改。

## Docker安装

```bash
docker run -d \
    --name cpolar \
    --restart always \
    --network host \
    -v /opt/cpolar:/usr/local/etc/cpolar \
    probezy/cpolar:latest
```

其中`/opt/cpolar`用于持久化Cpolar配置，容器内的配置文件路径为`/usr/local/etc/cpolar/cpolar.yml`。启动后访问`http://服务器IP:5025`，使用Cpolar账号登录Web UI即可。

修改配置后重启容器：

```bash
docker restart cpolar
```

查看运行日志：

```bash
docker logs -f cpolar
```

需要注意，容器使用了`--network host`，因此不需要再配置`-p`端口映射。

## Windows安装

Windows可以直接从[Cpolar官网](https://www.cpolar.com/download)下载对应版本，解压后双击安装程序，一路默认安装即可。

安装完成后可以在PowerShell中查看版本：

```powershell
cpolar version
```

浏览器访问`http://localhost:9200`，使用Cpolar账号登录Web UI即可。Windows配置文件位于`C:\Users\用户名\.cpolar\cpolar.yml`，修改`client_dashboard_addr`后需要重启Cpolar服务。
