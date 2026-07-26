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

`cpolar`是一款免费内网穿透工具，可以把内网中的 HTTP、HTTPS、TCP 等服务映射到公网。这里记录一下 Docker 部署、配置文件以及 Windows 下的安装方法。

## Cpolar配置文件

Cpolar使用`YAML`格式的配置文件，Docker部署时可以保存在宿主机的`/opt/cpolar/cpolar.yml`。

```bash
mkdir -p /opt/cpolar
vi /opt/cpolar/cpolar.yml
```

```yaml
client_dashboard_addr: 127.0.0.1:5025
tunnels:
  remoteDesktop:
    proto: tcp
    addr: "3389"
  ssh:
    proto: tcp
    addr: "22"
```

## Docker安装

```bash
docker run -d \
    --name cpolar \
    --restart always \
    --network host \
    -v /opt/cpolar:/usr/local/etc/cpolar \
    probezy/cpolar:latest
```

## Windows安装

Windows可以直接从[Cpolar官网](https://www.cpolar.com/download)下载对应版本，解压后双击安装程序，一路默认安装即可。
