---
title: "Armbian入坑指南"
date: 2023-08-11
lastmod: 2023-08-11
author: ["沧海"]
tags: ["Armbian"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
cover:
  image: "https://cdn.canghai.org/images/25012202.png"
---
最近想整个**AdGuardHome**提升一下家里的网络体验，一开始准备弄个~~AllinOne~~顺便当作**轻NAS**，接着刷到了矿渣**玩客云**，一年一杯奶茶钱的耗电量以及大把的开源项目约等于零成本达成目标。楼下收旧电脑那正好有一台，机器直接白嫖到手。

#### 刷机

网上有许多关于玩客云的开源项目，这里选择的是Github上开源的[Armbian](https://github.com/hzyitc/armbian-onecloud)。
- 正面板热风枪加热镊子轻撬即可看到底下螺丝，拧下螺丝抽出电路板，做工还行。
- 打开**Burn Tool**软件烧录，选择镜像点击开始，将玩客云**靠近HDMI的接口**使用数据线连接电脑，用镊子短接，再通电。等待刷机完成即可SSH连接。

<div style="text-align: center;">
<img src="https://cdn.canghai.org/images/23081101.jpg" alt="23081101" width="650" style="display: unset;"/>
</div>

#### 固定IP地址  
 
- 打开`/etc/netplan/`目录修改`armbian-default.yaml`，具体参数根据实际情况修改。
```c
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    eth0:
      addresses: 
        - 192.168.10.10/24
      gateway4: 192.168.10.1
      nameservers:
        addresses:
          - 119.29.29.29
          - 223.5.5.5
```
- 固定IPV4地址后，如需IPV6网路支持需在`/etc/sysctl.conf`中添加如下字段：
```c
net.ipv6.conf.all.disable_ipv6=0
net.ipv6.conf.default.disable_ipv6=0
net.ipv6.conf.default.accept_ra=2
net.ipv6.conf.all.accept_ra=2
```
- 重启后通过新地址连接服务器。
