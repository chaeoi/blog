---
title: "Docker内容器设置开机自启"
date: 2022-06-29
lastmod: 2022-06-29
author: ["沧海"]
tags: ["Docker"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
#### 允许Docker开机自启
- 设置开机启动
```c
systemctl enable docker.service
```
#### 设置容器开机自启

##### 以Valutwarden为例
- 新建容器时配置自启参数 
```c
docker run --restart=always Valutwarden
```
- 已存在的容器配置自启参数 
```c
docker update --restart=always Valutwarden
```