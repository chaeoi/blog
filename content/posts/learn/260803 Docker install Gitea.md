---
title: "Gitea之Docker部署"
date: 2026-08-03
lastmod: 2026-08-03
author: ["沧海"]
tags: ["Gitea"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

`Gitea`是一个轻量、开源的 Git 服务，可以用来托管自己的代码仓库。下面使用官方镜像部署一个单容器实例，数据统一保存到宿主机的`/opt/gitea/data`。

## 启动 Gitea

```bash
docker run -d \
  --name gitea \
  --restart always \
  -e USER_UID=1000 \
  -e USER_GID=1000 \
  -p 5035:3000 \
  -v /opt/gitea/data:/data \
  -v /etc/timezone:/etc/timezone:ro \
  -v /etc/localtime:/etc/localtime:ro \
  docker.gitea.com/gitea:latest
```

## 首次配置

第一次访问会进入安装页面。个人使用或小型团队可以选择`SQLite3`，数据库路径保持默认即可。生产环境或仓库数量较多时，建议使用独立的`PostgreSQL`或`MySQL`，并单独规划数据库备份。

安装页面中需要重点确认以下项目：

- **服务器域名**：填写实际访问 Gitea 的域名或服务器 IP。
- **Gitea 基础 URL**：填写完整地址，例如`http://git.example.com/`或`http://192.168.1.10:5035/`，末尾保留`/`。
- **管理员账号**：设置第一个管理员用户和强密码。

基础 URL 使用了非标准端口时，端口必须写进去，否则仓库地址、回调地址和页面跳转可能会生成错误的链接。完成安装后登录管理员账号即可创建组织和仓库。

## 使用 HTTP 克隆

当前命令只映射了 Web 端口，没有映射容器的 SSH 端口，因此直接使用页面生成的 HTTP 地址即可：

```bash
git clone http://服务器IP:5035/用户名/仓库名.git
```

推送时使用 Gitea 用户名和密码，或者使用个人访问令牌。建议在用户设置中创建访问令牌后，用令牌代替密码进行 Git 操作。


更多配置可以参考[Gitea 官方 Docker 文档](https://docs.gitea.com/installation/install-with-docker)。
