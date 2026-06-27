---
title: "Linux环境下 Swap 配置方法"
date: 2019-10-22
lastmod: 2019-10-22
author: ["沧海"]
tags: ["Swap"]
description: "记录在 Linux 中创建、启用、持久化和调优 swap 文件的常用步骤。"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

#### 检查当前内存和 Swap

```bash
free -h
```

如果输出里 `Swap` 全是 `0B`，说明当前没有可用 swap。

#### 创建 Swap 文件

```bash
dd if=/dev/zero of=/var/swap bs=1024 count=2048000
```

参数说明：

- `if`：输入文件，通常用 `/dev/zero`
- `of`：输出文件，也就是目标 swap 文件
- `bs`：块大小
- `count`：块数量

则总大小`1024*2048000=2097152000bytes≈2GB`

#### 格式化 Swap 文件

```bash
mkswap -f /var/swap
```

#### 启用 Swap

```bash
swapon /var/swap
```

如果出现权限提示，例如 `0644, 0600 suggested`，说明文件权限过宽，建议先修正：

```bash
chmod 600 /var/swap
```

然后重新启用。

#### 验证是否生效

```bash
free -h
```

或：

```bash
swapon -s
```
```bash
cat /proc/swaps
```

#### 设置开机自动挂载

编辑 `/etc/fstab`：

```bash
vim /etc/fstab
```

在文件末尾加入：

```fstab
/var/swap swap swap defaults 0 0
```

#### 关闭并移除 Swap

不再需要时，可以先关闭：

```bash
swapoff /var/swap
```

随后删除 `/etc/fstab` 中对应行，**再删除文件本身**。

#### 调整 swappiness

`swappiness` 决定系统使用 swap 的积极程度。

- `0`：尽量优先使用物理内存
- `100`：更积极地把内存页换出到 swap

先查看当前值：

```bash
cat /proc/sys/vm/swappiness
```

临时修改为 `10`：

```bash
sysctl vm.swappiness=10
```

要想重启后仍然生效，编辑 `/etc/sysctl.conf`：

```bash
vi /etc/sysctl.conf
```

追加：

```ini
vm.swappiness = 10
```
