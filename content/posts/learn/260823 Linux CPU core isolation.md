---
title: "Linux把一个CPU核心留给指定程序"
date: 2026-08-23
lastmod: 2026-08-23
author: ["沧海"]
tags: ["Linux"]
description: "让普通程序避开一个CPU核心，把它留给需要稳定运行的程序。"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

有些程序对运行延迟比较敏感，可以把一个 CPU 核心专门留给它。

本文以 CPU 3 为例，最后达到的效果是：

```text
普通程序：使用 0-2、4-11
指定程序：使用 CPU 3
```

这台机器有 12 个 CPU，所以才会出现 `0-2、4-11`。如果你的机器不是 12 个 CPU，需要把它改成“除了目标 CPU 以外的其他编号”。

## 临时使用

只想让程序这一次运行在 CPU 3，启动时在命令前加 `taskset -c 3`：

```bash
taskset -c 3 your_program
```

把 `your_program` 换成实际的启动命令。例如原来这样启动：

```bash
/path/to/your_program
```

加锁后就写成：

```bash
taskset -c 3 /path/to/your_program
```

程序已经启动时，先查 PID，再设置 CPU：

```bash
pgrep -af your_program
taskset -cp 3 <PID>
```

检查是否成功：

```bash
taskset -pc <PID>
```

看到下面的结果，说明程序只能使用 CPU 3：

```text
current affinity list: 3
```

这种方法只对当前运行的程序有效。程序退出或机器重启后，需要重新设置。

## 开机后一直保留CPU 3

如果希望每次开机都把 CPU 3 空出来，需要改两个配置，然后重启一次。

### 1. 修改启动参数

先备份启动配置：

```bash
sudo cp -a /boot/extlinux/extlinux.conf \
  /boot/extlinux/extlinux.conf.backup.$(date +%Y%m%d-%H%M%S)
```

打开配置文件：

```bash
sudoedit /boot/extlinux/extlinux.conf
```

找到以 `APPEND` 开头的那一行，在最后加上：

```text
isolcpus=3 nohz_full=3 rcu_nocbs=3 irqaffinity=0-2,4-11
```

不要删除原来的内容，只在行尾追加这一段。

这几个参数的作用可以简单理解为：告诉 Linux，普通任务和默认中断不要使用 CPU 3。

### 2. 修改普通服务的 CPU 范围

创建一个 systemd 配置文件：

```bash
sudo install -d /etc/systemd/system.conf.d
sudoedit /etc/systemd/system.conf.d/90-cpu-affinity.conf
```

写入下面两行：

```ini
[Manager]
CPUAffinity=0-2 4-11
```

保存后让 systemd 重新读取配置：

```bash
sudo systemctl daemon-reexec
```

最后重启电脑：

```bash
sudo reboot
```

## 重启后检查

检查 CPU 3 是否已经隔离：

```bash
cat /sys/devices/system/cpu/isolated
```

如果输出 `3`，说明内核参数已经生效。

检查普通进程可以使用哪些 CPU：

```bash
taskset -pc 1
```

正常应看到：

```text
current affinity list: 0-2,4-11
```

启动指定程序时，仍然要加 `taskset`：

```bash
taskset -c 3 your_program
```

最后可以查看 CPU 3 上还有哪些任务：

```bash
ps -eLo pid,ppid,psr,user,comm --no-headers | awk '$3 == 3 {print}'
```

看到少量 `kworker`、`ksoftirqd` 之类的内核线程是正常的，普通用户进程不应该出现在这里。

## 恢复原来的设置

不再需要 CPU 隔离时：

1. 从 `/boot/extlinux/extlinux.conf` 删除刚才追加的参数。
2. 删除 `/etc/systemd/system.conf.d/90-cpu-affinity.conf`。
3. 重启电脑。

也可以直接用备份文件恢复启动配置：

```bash
sudo cp -a /boot/extlinux/extlinux.conf.backup.<时间戳> \
  /boot/extlinux/extlinux.conf
sudo reboot
```

简单来说：临时锁核只需要 `taskset`；想让 CPU 3 每次开机都留给指定程序，再修改启动参数和 systemd，然后重启。
