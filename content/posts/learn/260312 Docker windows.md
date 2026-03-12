---
title: "用 Docker跑Windows虚拟机"
date: 2026-03-12
lastmod: 2026-03-12
author: ["沧海"]
tags: ["Docker", "虚拟机"]
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
  -e RAM_SIZE=6G \
  -e CPU_CORES=24 \
  -e DISK_TYPE=nvme \
  -e DISK_SIZE=256G \
  dockurr/windows
```

这个命令适合第一次装系统的时候用，因为它保留了网页控制台，远程桌面没配好之前，至少还能从浏览器进去看安装过程。我自己最常改的环境变量基本就是下面这些：

```
-v /opt/windows/windows.iso:/boot.iso
```

挂载`/opt/windows/windows.iso`为系统镜像，如果没有挂载的话记得指定一下系统版本。

```bash
-e MANUAL=Y
```

表示手动安装。第一次部署时我一般都会先保留手动。

```bash
-e RAM_SIZE=6G
```

虚拟机内存大小，默认是`4G`。

```bash
-e CPU_CORES=24
```

虚拟机核心数，默认是`2`。

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

如果我更看重性能，一般会优先在 `nvme`、`scsi`、`blk` 里选。  如果更看重兼容性，就选 `sata`。

```bash
-e DISK_SIZE=256G
```

虚拟磁盘大小，默认是 `64G`。

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
  -e MANUAL=Y \
  -e RAM_SIZE=6G \
  -e CPU_CORES=24 \
  -e DISK_TYPE=nvme \
  -e DISK_SIZE=256G \
  -e DISPLAY=none \
  -e WEB=N \
  dockurr/windows
```

这个就是无头模式，只保留 `RDP`，不用再开网页控制台。系统装好、远程桌面正常以后，我更推荐这个。

---

下面是完整的变量速查表。

#### Windows 安装相关变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `APP` | `Windows` | 页面或日志里的产品名 | 任意字符串 |
| `SUPPORT` | `https://github.com/dockur/windows` | 支持链接 | 任意 URL |
| `PLATFORM` | `x64` | 平台标识 | `x64` |
| `VERSION` | `11` | 选择安装版本或自定义 ISO | `11` `11e` `11i` `11l` `10` `10e` `10i` `10l` `8e` `7u` `7e` `7x86` `7ux86` `7ex86` `vu` `ve` `vistax86` `vux86` `vex86` `xp` `xp64` `2k` `2025` `2022` `2019` `hv` `2016` `2012` `2008` `2003` `tiny11` `tiny10` `core11` `nano11`，或直接填写 ISO URL |
| `LANGUAGE` | `en` | 安装语言 | 语言名或语言代码 |
| `REGION` | 空 | 区域设置 | 如 `en-US`、`fr-FR` |
| `KEYBOARD` | 空 | 键盘布局 | 如 `en-US`、`de-DE` |
| `USERNAME` | `Docker` | 安装时创建的用户名 | 字母数字及 `@!._-` |
| `PASSWORD` | `admin` | 安装用户密码 | 字符串 |
| `KEY` | 空 | 产品密钥 | 25 位密钥 |
| `EDITION` | 空 | 服务器版安装 edition | 常见 `CORE`、`DATACENTER`、`STANDARD` |
| `MANUAL` | 关 | 是否手动安装 | `Y` / `N` |
| `VERIFY` | 关 | 下载后校验哈希 | `Y` / `N` |
| `WIDTH` | `1280` | 安装阶段分辨率宽 | 正整数 |
| `HEIGHT` | `720` | 安装阶段分辨率高 | 正整数 |
| `REMOVE` | 开 | 安装完成后删除 ISO | `Y` / `N` |
| `DETECTED` | 空 | 内部检测版本标记 | 内部值，不建议手动改 |
| `MIDO` | 开 | 启用 Mido 下载链路 | `Y` / `N` |
| `ESD` | 开 | 启用 ESD 下载链路 | `Y` / `N` |
| `UNPACK` | 关 | 解包后继续解包内层 ISO | `Y` / `N` |
| `SAMBA` | `Y` | 启用共享目录 | `Y` / `N` |
| `SAMBA_DEBUG` | `N` | Samba 调试日志 | `Y` / `N` |
| `SAMBA_LEVEL` | `1` | Samba 日志级别 | 数字字符串 |
| `SAMBA_INTERFACE` | 空 | 绑定指定接口或 IP | 接口名或 IP |
| `QEMU_TIMEOUT` | `110` | 关机等待超时秒数 | 正整数 |

#### 启动、CPU 和虚拟机核心变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `BOOT` | 空 | 通用启动镜像选择 | 关键字、URL、本地映射 |
| `BOOT_MODE` | `windows` | 启动固件模式 | `uefi` `secure` `windows` `windows_plain` `windows_secure` `windows_legacy` `legacy` `custom` |
| `BOOT_INDEX` | `9` | 启动介质 boot index | 数字 |
| `BIOS` | 空 | 自定义 BIOS 文件 | 容器内路径 |
| `LOGO` | 开 | 注入 UEFI logo | `Y` / `N` |
| `KVM` | `Y` | 硬件加速 | `Y` / `N` |
| `MACHINE` | `q35` | QEMU machine 类型 | 如 `q35`、`pc` |
| `CPU_CORES` | `2` | vCPU 数 | 数字、`max`、`half` |
| `RAM_SIZE` | `4G` | 内存大小 | 如 `2G`、`8192M`、`max`、`half` |
| `RAM_CHECK` | `Y` | 检查可用内存并自动降配 | `Y` / `N` |
| `CPU_MODEL` | 自动 | CPU 型号 | 任意 QEMU model，如 `host` |
| `CPU_FLAGS` | 自动 | 附加 CPU 特性 | QEMU `-cpu` flags |
| `VMX` | `N` | Windows 场景 VMX 开关 | `Y` / `N` |
| `HPET` | `off` | machine hpet | `on` / `off` |
| `VMPORT` | `off` | machine vmport | `on` / `off` |
| `UUID` | 空 | VM UUID | UUID |
| `TPM` | `N` | swtpm | `Y` / `N` |
| `SMM` | `N` | SMM | `Y` / `N` |
| `SERIAL` | `mon:stdio` | 串口后端 | 任意 QEMU serial 参数 |
| `SMP` | `$CPU_CORES,sockets=1,dies=1,cores=$CPU_CORES,threads=1` | 完整 SMP 拓扑 | 任意 QEMU `-smp` |
| `MONITOR` | `telnet:localhost:$MON_PORT,server,nowait,nodelay` | monitor 配置 | 任意 monitor 参数 |
| `MON_PORT` | `7100` | monitor 端口 | 端口号 |
| `ARGUMENTS` | 空 | 追加 QEMU 参数 | 任意参数串 |
| `ARGS` | 空 | 内部兼容参数 | 字符串 |
| `DEBUG` | `N` | 调试日志 | `Y` / `N` |
| `TRACE` | 关 | shell trace | `Y` / `N` |

#### 磁盘和存储变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `STORAGE` | `/storage` | 持久化目录 | 容器内路径 |
| `ALLOCATE` | `N` | 是否预分配磁盘 | `Y` / `N` |
| `DISK_SIZE` | `64G` | 主盘大小 | 如 `64G` `200G` `max` `half` |
| `DISK2_SIZE` | 空 | 第二块盘大小 | 如 `64G` `200G` `max` `half` |
| `DISK3_SIZE` | 空 | 第三块盘大小 | 如 `64G` `200G` `max` `half` |
| `DISK4_SIZE` | 空 | 第四块盘大小 | 如 `64G` `200G` `max` `half` |
| `DISK5_SIZE` | 空 | 第五块盘大小 | 如 `64G` `200G` `max` `half` |
| `DISK6_SIZE` | 空 | 第六块盘大小 | 如 `64G` `200G` `max` `half` |
| `DISK_NAME` | `data` | 磁盘文件名前缀 | 字符串 |
| `DEVICE` | 空 | 直通块设备 1 | 块设备路径 |
| `DEVICE2` | 空 | 直通块设备 2 | 块设备路径 |
| `DEVICE3` | 空 | 直通块设备 3 | 块设备路径 |
| `DEVICE4` | 空 | 直通块设备 4 | 块设备路径 |
| `DEVICE5` | 空 | 直通块设备 5 | 块设备路径 |
| `DEVICE6` | 空 | 直通块设备 6 | 块设备路径 |
| `DISK_FMT` | `raw` | 磁盘格式 | `raw` / `qcow2` |
| `DISK_TYPE` | `scsi` | 磁盘设备类型 | `ide` `sata` `nvme` `usb` `scsi` `blk` `auto` `none` |
| `MEDIA_TYPE` | 自动 | 光驱或安装介质设备类型 | 同 `DISK_TYPE` |
| `DISK_IO` | `native` | AIO 模式 | `native` `threads` `io_uring` |
| `DISK_CACHE` | `none` | 磁盘缓存策略 | 常见 `none` `writeback` |
| `DISK_DISCARD` | `on` | TRIM 透传 | `on` / `off` |
| `DISK_ROTATION` | `1` | 旋转率 | 整数 |
| `DISK_FLAGS` | 空 | qcow2 附加参数 | `qemu-img` 参数串 |

#### 网络变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `NETWORK` | `Y` | 网络类型总开关 | `N`、`Y`、`tap`、`tun`、`tuntap`、`user`、`passt`、`slirp` |
| `DHCP` | `N` | 让客体从路由器拿 IP | `Y` / `N` |
| `ADAPTER` | `virtio-net-pci` | 网卡型号 | 任意 QEMU NIC，如 `e1000`、`rtl8139` |
| `MAC` | 自动生成 | MAC 地址 | 12 位或 17 位 |
| `MTU` | 自动探测 | MTU | 整数 |
| `HOST_PORTS` | 空 | 主机保留或排除端口 | 逗号分隔，如 `22,80,443,3389/udp` |
| `USER_PORTS` | 空 | user mode 需要转发的端口 | 逗号分隔 |
| `VM_NET_IP` | 自动 | 客体 IP | IPv4 |
| `VM_NET_DEV` | 自动 | 容器网卡名 | 如 `eth0` `net0` |
| `VM_NET_TAP` | `qemu` | tap 或 macvtap 名 | 字符串 |
| `VM_NET_MAC` | `$MAC` | 覆盖客体 MAC | MAC |
| `VM_NET_HOST` | `$APP` | 客体 hostname | 字符串 |
| `VM_NET_BRIDGE` | `docker` | bridge 名 | 字符串 |
| `VM_NET_MASK` | `255.255.255.0` | 子网掩码 | IPv4 mask |
| `PASST` | `/run/passt` | passt 可执行文件路径 | 文件路径 |
| `PASST_MTU` | 空 | passt MTU | 整数 |
| `PASST_OPTS` | 空 | passt 参数 | 参数串 |
| `PASST_DEBUG` | 关 | passt 调试日志 | `Y` / `N` |
| `PASST_PID` | `/var/run/passt.pid` | passt pid 文件 | 文件路径 |
| `DNSMASQ` | `/usr/sbin/dnsmasq` | dnsmasq 程序路径 | 文件路径 |
| `DNSMASQ_OPTS` | 空 | dnsmasq 参数 | 参数串 |
| `DNSMASQ_DEBUG` | 关 | dnsmasq 调试日志 | `Y` / `N` |
| `DNSMASQ_CONF_DIR` | `/etc/dnsmasq.d` | dnsmasq 配置目录 | 路径 |
| `DNSMASQ_PID` | `/var/run/dnsmasq.pid` | dnsmasq pid 文件 | 路径 |
| `DNSMASQ_DISABLE` | 关 | 禁用 dnsmasq | `Y` / `N` |

#### 显示、Web 和 USB 变量

| 变量 | 默认值 | 作用 | 可选值 |
|---|---|---|---|
| `DISPLAY` | `web` | 显示输出模式 | `web` `vnc` `disabled` `none` 或其它 QEMU display 字符串 |
| `VGA` | `virtio` | 显卡类型 | 如 `virtio` `std` `cirrus` |
| `GPU` | `N` | Intel GPU 加速路径 | `Y` / `N` |
| `RENDERNODE` | `/dev/dri/renderD128` | GPU render node | 设备路径 |
| `VNC_PORT` | `5900` | VNC 端口 | `>=5900` |
| `WEB_PORT` | `8006` | Web UI 端口 | 端口号 |
| `WSD_PORT` | `8004` | websocketd 端口 | 端口号 |
| `WSS_PORT` | `5700` | noVNC websocket 端口 | 端口号 |
| `WEB` | 开 | 是否启用 Web UI | `Y` / `N` |
| `USER` | `admin` | Web Basic Auth 用户名 | 字符串，仅 `PASS` 设置时生效 |
| `PASS` | 空 | Web Basic Auth 密码 | 字符串 |
| `USB` | `qemu-xhci,id=xhci,p2=7,p3=7` | USB 控制器或设备参数 | 任意 QEMU `-device` 字符串，`no` 可禁用 |
