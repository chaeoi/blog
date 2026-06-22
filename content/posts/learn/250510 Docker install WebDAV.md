---
title: "使用Docker安装WebDAV"
date: 2025-05-10
lastmod: 2025-05-10
author: ["沧海​"]
tags: ["WebDav"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
共享音视频等流媒体文件时往往**WebDAV**是个不错的选择，WebDAV基于**HTTP**协议可以轻松借用Cloudflare等CDN服务加速，并且WebDAV在Infuse等视频软件上的挂载比Samba的体验要好很多。这里选择的是Github上的明星项目[hacdias/webdav](https://github.com/hacdias/webdav)。
```c
docker run -d \
  --name webdav \
  --restart always \
  --network host \
  -v /opt/webdav/config.yml:/config.yml \
  -v /mnt:/mnt \
  hacdias/webdav:latest -c /config.yml
  ```
推荐配置如下
```yml
address: 0.0.0.0
port: 5066
prefix: /
debug: false
noSniff: true
behindProxy: false
directory: /mnt/disk/video
permissions: CRUD
rules: []
rulesBehavior: overwrite
log:
  format: console
  colors: true
  outputs:
  - stderr
cors:
  enabled: false
users:
  - username: canghai
    password: 123456
```
其中`permissions`可选权限有C (Create), R (Read), U (Update), D (Delete)。也可在`users`中单独写入权限及规则，具体配置解析可参考**README**。