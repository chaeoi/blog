---
title: "用Docker跑Windows虚拟机"
date: 2026-03-12
lastmod: 2026-03-12
author: ["沧海"]
tags: ["虚拟机"]
description: "记录我用dockurr/windows拉起Windows虚拟机时常用的命令，以及111个环境变量的默认值和可选值。"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

> 我先记第一次安装时最常用的命令：

```bash
docker run -d \
  --name windows \
  --restart always \
  --device=/dev/kvm \
  --device=/dev/net/tun \
  --cap-add NET_ADMIN \
  -p 8006:8006 \
  -p 3389:3389/tcp \
  -p 3389:3389/udp \
  -v /opt/windows:/storage \
  -v /opt/windows/windows.iso:/boot.iso \
  -e MANUAL=Y \
  -e RAM_SIZE=4G \
  -e CPU_CORES=24 \
  -e DISK_TYPE=nvme \
  -e DISK_SIZE=256G \
  dockurr/windows
```

这个命令适合第一次装系统的时候用，因为它保留了网页控制台，远程桌面没配好之前，至少还能从浏览器进去看安装过程。我自己最常改的环境变量基本就是下面这些：

```
-v /opt/windows/windows.iso:/boot.iso
```

挂载`/opt/windows/windows.iso`为系统镜像，如果没有挂载的话记得指定一下系统版本

```bash
-e MANUAL=Y
```

表示手动安装。第一次部署时我一般都会先保留手动

```bash
-e RAM_SIZE=4G
```

虚拟机内存大小，默认是`4G`

```bash
-e CPU_CORES=24
```

虚拟机核心数，默认是`2`

```bash
-e DISK_TYPE=nvme
```

虚拟磁盘类型，默认是`scsi`，但进到系统中硬盘会识别为可插拔，提示**安全删除硬件和弹出媒体**/**Safely Remove Hardware and Eject Media**，我一般会改为`nvme`，常见的选项有：

```bash
ide
sata
nvme
usb
scsi
blk
auto
none
```

如果更看重性能，一般会优先在`nvme`、`scsi`、`blk`里选。  如果更看重兼容性，就选`sata`

```bash
-e DISK_SIZE=256G
```

虚拟磁盘大小，默认是`64G`

> 虚拟机稳定以后，我一般会改用第二个命令：

```bash
docker run -d \
  --name windows \
  --restart always \
  --device=/dev/kvm \
  --device=/dev/net/tun \
  --cap-add NET_ADMIN \
  -p 3389:3389/tcp \
  -p 3389:3389/udp \
  -v /opt/windows:/storage \
  -v /opt/windows/windows.iso:/boot.iso \
  -e RAM_SIZE=4G \
  -e CPU_CORES=24 \
  -e DISK_TYPE=nvme \
  -e DISK_SIZE=256G \
  -e DISPLAY=none \
  -e WEB=N \
  dockurr/windows
```

这个就是无头模式，只保留`RDP`，不用再开网页控制台。系统装好、远程桌面正常以后，我更推荐这个。

---

#### Windows 安装相关变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `APP` | `Windows` | 产品名 | 任意字符串 |
| `SUPPORT` | `dockur/windows` | 支持链接 | 任意URL |
| `PLATFORM` | `x64` | 平台标识 | `x64` |
| `VERSION` | `11` | 安装版本 | `11` `10` `ISO URL`等 |
| `LANGUAGE` | `en` | 安装语言 | 语言代码 |
| `REGION` | 空 | 区域设置 | `en-US` `fr-FR`等 |
| `KEYBOARD` | 空 | 键盘布局 | `en-US` `de-DE`等 |
| `USERNAME` | `Docker` | 用户名 | Windows用户名 |
| `PASSWORD` | `admin` | 安装用户密码 | 字符串 |
| `KEY` | 空 | 产品密钥 | 密钥 |
| `EDITION` | 空 | 服务器安装版本 | `DATACENTER` `STANDARD`等 |
| `MANUAL` | 关 | 是否手动安装 | `Y`/`N` |
| `VERIFY` | 关 | 下载后校验哈希 | `Y`/`N` |
| `WIDTH` | `1280` | 安装阶段分辨率宽 | 正整数 |
| `HEIGHT` | `720` | 安装阶段分辨率高 | 正整数 |
| `REMOVE` | 开 | 安装完成删除ISO | `Y`/`N` |
| `DETECTED` | 空 | 内部检测版本标记 | 内部值，不建议改 |
| `MIDO` | 开 | 启用Mido下载链路 | `Y`/`N` |
| `ESD` | 开 | 启用ESD下载链路 | `Y`/`N` |
| `UNPACK` | 关 | 解包内层ISO | `Y`/`N` |
| `SAMBA` | `Y` | 启用共享目录 | `Y`/`N` |
| `SAMBA_DEBUG` | `N` | Samba调试日志 | `Y`/`N` |
| `SAMBA_LEVEL` | `1` | Samba日志级别 | 数字字符串 |
| `SAMBA_INTERFACE` | 空 | 绑定接口或IP | 接口名或IP |
| `QEMU_TIMEOUT` | `110` | 关机等待超时秒数 | 正整数 |

#### 启动、CPU 和虚拟机核心变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `BOOT` | 空 | 启动镜像选择 | 关键字 URL 本地映射 |
| `BOOT_MODE` | `windows` | 启动固件模式 | `uefi` `secure` `legacy`等 |
| `BOOT_INDEX` | `9` | 启动介质 | 数字 |
| `BIOS` | 空 | 自定义BIOS | 容器内路径 |
| `LOGO` | 开 | 注入UEFI LOGO | `Y`/`N` |
| `KVM` | `Y` | 硬件加速 | `Y`/`N` |
| `MACHINE` | `q35` | QEMU machine类型 | `q35` `pc`等 |
| `CPU_CORES` | `2` | vCPU 数 | `2` `max` `half`等 |
| `RAM_SIZE` | `4G` | 内存大小 | `2G` `max` `half`等 |
| `RAM_CHECK` | `Y` | 自动降配内存 | `Y`/`N` |
| `CPU_MODEL` | 自动 | CPU型号 | `QEMU model` `host`等 |
| `CPU_FLAGS` | 自动 | 附加CPU特性 | QEMU参数 |
| `VMX` | `N` | VMX开关 | `Y`/`N` |
| `HPET` | `off` | machine hpet | `on`/`off` |
| `VMPORT` | `off` | machine vmport | `on`/`off` |
| `UUID` | 空 | VM UUID | UUID |
| `TPM` | `N` | swtpm | `Y`/`N` |
| `SMM` | `N` | SMM | `Y`/`N` |
| `SERIAL` | `mon:stdio` | 串口后端 | QEMU参数 |
| `SMP` | `$CPU_CO...` | 完整SMP拓扑 | QEMU参数 |
| `MONITOR` | `teln...` | monitor配置 | monitor参数 |
| `MON_PORT` | `7100` | monitor端口 | 端口号 |
| `ARGUMENTS` | 空 | 追加QEMU参数 | 任意参数串 |
| `ARGS` | 空 | 内部兼容参数 | 字符串 |
| `DEBUG` | `N` | 调试日志 | `Y`/`N` |
| `TRACE` | 关 | shell trace | `Y`/`N` |

#### 磁盘和存储变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `STORAGE` | `/storage` | 持久化目录 | 容器内路径 |
| `ALLOCATE` | `N` | 是否预分配磁盘 | `Y`/`N` |
| `DISK_SIZE` | `64G` | 主盘大小 | `64G` `max` `half`等 |
| `DISK2_SIZE` | 空 | 第二块盘大小 | `64G` `max` `half`等 |
| `DISK3_SIZE` | 空 | 第三块盘大小 | `64G` `max` `half`等 |
| `DISK4_SIZE` | 空 | 第四块盘大小 | `64G` `max` `half`等 |
| `DISK5_SIZE` | 空 | 第五块盘大小 | `64G` `max` `half`等 |
| `DISK6_SIZE` | 空 | 第六块盘大小 | `64G` `max` `half`等 |
| `DISK_NAME` | `data` | 磁盘文件名前缀 | 字符串 |
| `DEVICE` | 空 | 直通块设备 1 | 块设备路径 |
| `DEVICE2` | 空 | 直通块设备 2 | 块设备路径 |
| `DEVICE3` | 空 | 直通块设备 3 | 块设备路径 |
| `DEVICE4` | 空 | 直通块设备 4 | 块设备路径 |
| `DEVICE5` | 空 | 直通块设备 5 | 块设备路径 |
| `DEVICE6` | 空 | 直通块设备 6 | 块设备路径 |
| `DISK_FMT` | `raw` | 磁盘格式 | `raw` `qcow2` |
| `DISK_TYPE` | `scsi` | 磁盘设备类型 | `sata` `nvme` `scsi` `auto` `none`等 |
| `MEDIA_TYPE` | 自动 | 光驱或安装介质设备类型 | `sata` `nvme` `scsi` `auto` `none`等 |
| `DISK_IO` | `native` | AIO模式 | `native` `threads` `io_uring` |
| `DISK_CACHE` | `none` | 磁盘缓存策略 | `none` `writeback`等 |
| `DISK_DISCARD` | `on` | TRIM透传 | `on`/`off` |
| `DISK_ROTATION` | `1` | 旋转率 | 整数 |
| `DISK_FLAGS` | 空 | qcow2附加参数 | qemu-img参数 |

#### 网络变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `NETWORK` | `Y` | 网络类型总开关 | `N` `Y` `tap` `tun`等 |
| `DHCP` | `N` | 让客体从路由器拿 IP | `Y`/`N` |
| `ADAPTER` | `virtio-net-pci` | 网卡型号 | `e1000`、`rtl8139`等 |
| `MAC` | 自动生成 | MAC地址 | MAC地址 |
| `MTU` | 自动探测 | MTU | MTU |
| `HOST_PORTS` | 空 | 保留或排除端口 | 端口 |
| `USER_PORTS` | 空 | 转发端口 | 端口 |
| `VM_NET_IP` | 自动 | 客体IP | IPv4 |
| `VM_NET_DEV` | 自动 | 容器网卡名 | 网卡名 |
| `VM_NET_TAP` | `qemu` | tap名 | 字符串 |
| `VM_NET_MAC` | `$MAC` | 覆盖客体MAC | MAC |
| `VM_NET_HOST` | `$APP` | 客体hostname | 字符串 |
| `VM_NET_BRIDGE` | `docker` | bridge 名 | 字符串 |
| `VM_NET_MASK` | `255.255.255.0` | 子网掩码 | 子网掩码 |
| `PASST` | `/run/passt` | passt路径 | 文件路径 |
| `PASST_MTU` | 空 | passt MTU | 整数 |
| `PASST_OPTS` | 空 | passt参数 | 参数串 |
| `PASST_DEBUG` | 关 | passt日志 | `Y`/`N` |
| `PASST_PID` | `/var/ru...` | passt pid文件 | 文件路径 |
| `DNSMASQ` | `/usr/sbi...` | dnsmasq程序路径 | 文件路径 |
| `DNSMASQ_OPTS` | 空 | dnsmasq参数 | 参数串 |
| `DNSMASQ_DEBUG` | 关 | dnsmasq日志 | `Y`/`N` |
| `DNSMASQ_CONF_DIR` | `/etc/dn...` | dnsmasq目录 | 路径 |
| `DNSMASQ_PID` | `/var/ru...` | dnsmasq pid文件 | 路径 |
| `DNSMASQ_DISABLE` | 关 | 禁用dnsmasq | `Y`/`N` |

#### 显示、Web 和 USB 变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `DISPLAY` | `web` | 显示模式 | `web` `vnc` `disabled` `none`等 |
| `VGA` | `virtio` | 显卡类型 | 如 `virtio` `std` `cirrus` |
| `GPU` | `N` | Intel GPU加速 | `Y`/`N` |
| `RENDERNODE` | `/dev/dr...` | GPU render node | 设备路径 |
| `VNC_PORT` | `5900` | VNC | `>=5900` |
| `WEB_PORT` | `8006` | Web UI | 端口号 |
| `WSD_PORT` | `8004` | websocketd | 端口号 |
| `WSS_PORT` | `5700` | noVNC websocket | 端口号 |
| `WEB` | 开 | 启用Web UI | `Y`/`N` |
| `USER` | `admin` | 用户名 | 字符串 |
| `PASS` | 空 | 密码 | 字符串 |
| `USB` | `qemu-x...` | 控制器或设备参数 | `字符串` `no` |
