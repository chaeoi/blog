---
title: "部署CodeServer"
date: 2025-07-16
lastmod: 2026-07-27
author: ["沧海"]
tags: ["CodeServer"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

`CodeServer`可以把 VS Code 运行在服务器上，通过浏览器直接访问。这里记录一下使用 Docker 和安装脚本部署`CodeServer`的方法。

> Docker安装CodeServer

```bash
docker run -d \
    --name codeserver \
    --restart always \
    --network host \
    --user root \
    -e PORT=5068 \
    -e PASSWORD=password \
    -e all_proxy=socks5h://127.0.0.1:10808 \
    -v /opt/codeserver/root:/root \
    -v /opt/codeserver/project:/opt/codeserver/project \
    codercom/code-server:latest
```

需要注意几点：

- `PORT=5068`是网页访问端口，启动后访问`http://服务器IP:5068`
- `PASSWORD`需要改成自己的密码
- `all_proxy=socks5h://127.0.0.1:10808`用于配置网络代理，不需要时可以删除
- `/opt/codeserver/root`用于持久化`/root`目录，重建容器后配置不会丢失
- `/opt/codeserver/project`用于存放项目文件

> 在宿主机安装CodeServer

不使用 Docker 时，可以通过官方安装脚本直接部署：

```bash
curl -fsSL https://code-server.dev/install.sh | sh
```

安装完成后启动服务并设置开机自启：

```bash
sudo systemctl enable --now code-server@$USER
```

配置文件位于：

```txt
~/.config/code-server/config.yaml
```

可以在配置文件中修改监听地址、端口和密码等设置。
