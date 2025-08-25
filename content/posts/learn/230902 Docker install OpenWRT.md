---
title: "Docker部署OpenWRT旁路由快速入门"
date: 2023-09-02
lastmod: 2023-09-02
author: ["沧海"]
tags: ["OpenWRT"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
旁路由是在不改变网络架构的情况下最方便的升级网络环境的方式，OpenWRT社区生态丰富，基本可以满足你的绝大部分上网需求。
#### 自制OpenWRT固件
- **OpenWRT**作为一个成熟的路由系统，基本适配市面上绝大部分设备，不同人编译的固件搭载了不同的插件。这里推荐使用**Github Action**进行固件的自编译，选择需要的插件，编译一个适合自己的固件，具体过程这里就不过多介绍了。
- 你也可以试试开源项目[OpenWRT.ai](https://openwrt.ai)，进行固件的自编译，这里给出OpenWRT.ai默认加载的插件，可以选择性精简。
```c
-luci-app-gpsysupgrade -luci-app-quickstart -luci-app-firewall -luci-app-advanced -luci-app-autoreboot -luci-app-cpufreq -luci-app-upnp -luci-app-fan -luci-app-wizard
```

#### 上传镜像至DockerHub
- **wget**命令将编译好的镜像下载至服务器中，并使用**mv**命令改名
- 解压文件
```c
gzip -d openwrt.img.gz
```
- 挂载镜像
```c
modprobe nbd
```
```c
qemu-nbd -c /dev/nbd0 -f raw openwrt.img
```
- 打包镜像
```c
mkdir /opt/openwrt
```
```c
mount /dev/nbd0p2 /opt/openwrt
```
```c
cd /opt/openwrt
```
```c
tar -czvf /opt/openwrt/openwrt.rootfs.tar.gz *
```
- 导入镜像，`tag`部分注意修改
```c
docker import openwrt.rootfs.tar.gz user/app:latest
```
- 通过`docker login`登录DockerHub
- 推送镜像
```c
docker push user/app:latest
```

#### 部署OpenWRT
- 开启网卡混杂模式
```c
ip link set eth0 promisc on
```
- (Optional)也可以通过在`/etc/rc.local`中写入命令，永久开启网卡混杂模式
```c
ip link set eth0 promisc on
```
- 创建Docker网络，具体信息请根据实际情况修改，容器内需**IPV6**支持，需要增加相关信息
```c
docker network create -d macvlan --subnet=192.168.10.0/24 --gateway=192.168.10.1 -o parent=eth0 openwrt
```
```c
docker network create -d macvlan --subnet=192.168.10.0/24 --gateway=192.168.10.1 --subnet=fe80::/16 --gateway=fe80::1 -o parent=eth0 openwrt
```
- 拉取镜像
```c
docker run -d \
	--restart always \
	--name openwrt \
	--network openwrt \
	--privileged=true \
	user/app:latest \
	/sbin/init
```
- (Optional)如需**IPV6**支持需要在`sysctl.conf`中添加如下字段：
```c
docker exec -it openwrt bash
```
```c
vi /etc/sysctl.conf
```
```c
net.ipv6.conf.all.disable_ipv6=0
net.ipv6.conf.default.disable_ipv6=0
net.ipv6.conf.default.accept_ra=2
net.ipv6.conf.all.accept_ra=2
```
- (Optional)如编译时选择的OpenWRT地址不在局域网网段内需自行修改`ipaddr`
```c
docker exec -it openwrt bash
```
```c
vi /etc/config/network
```
- 重启后通过填写的局域网地址即可打开OpenWRT后台界面