---
title: "DDNS之Docker部署"
date: 2023-06-30
lastmod: 2023-06-30
author: ["沧海"]
tags: ["DDNS"]
description: "使用Docker部署DNS动态解析"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
DDNS-GO可以自动获得你的公网**IPv4**或**IPv6**地址，并解析到对应的域名服务。项目地址见[Github](https://github.com/jeessy2/ddns-go)。
- 支持Mac、Windows、Linux系统，支持ARM、x86架构。
- 支持的域名服务商 Alidns、Dnspod、Cloudflare、HUAWEI、Baidu、Porkbun、GoDaddy、Google。
- 支持接口、网卡、命令获取IP。
- 支持以服务的方式运行。
- 默认间隔5分钟同步一次。
- 支持同时配置多个DNS服务商。
- 支持多个域名同时解析。
- 支持多级域名。
- 网页中配置，简单又方便，默认勾选禁止从公网访问。
- 网页中方便快速查看最近50条日志。
- 支持Webhook通知。
- 支持TTL。
- 支持部分DNS服务商传递自定义参数，实现地域解析等功能。
```c
docker run -d \
    --name ddns-go \
    --restart always \
    --network host \
    -v /opt/ddns-go:/root \
    jeessy/ddns-go -l :5011 -c /root/config.yaml
```