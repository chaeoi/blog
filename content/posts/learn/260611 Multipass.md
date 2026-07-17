---
title: "Multipass安装与常用命令"
date: 2026-06-11
lastmod: 2026-06-11
author: ["沧海"]
tags: ["Multipass"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
>安装Multipass

`Multipass`是 Canonical 出的轻量虚拟机管理工具，可以在命令行里快速拉起 Ubuntu 虚拟机，不用像 VMware 那样手动挂镜像装系统，一条命令就能开出一台干净的环境。这里记录一下安装和平时常用的命令。

Ubuntu 上直接用 snap 安装：

```bash
snap install multipass
```

安装完确认一下版本：

```bash
multipass version
```

能正常输出`multipass`和`multipassd`两个版本号就说明装好了。

>查找可用镜像

创建虚拟机之前可以先看看有哪些镜像可以用：

```bash
multipass find
```

会列出可用的 Ubuntu 版本，比如`22.04`、`24.04`，还有对应的别名（如`jammy`、`noble`、`lts`）。创建的时候写版本号或者别名都可以。

>创建虚拟机

```bash
multipass launch 22.04 -n codeserver -c 8 -m 8G -d 100G
```

参数含义：

- `22.04`是镜像版本，不写的话默认用最新的 LTS
- `-n codeserver`指定虚拟机名字，后面所有操作都靠这个名字
- `-c 8`分配 8 核 CPU
- `-m 8G`分配 8G 内存
- `-d 100G`磁盘 100G

第一次创建会先下载镜像，之后再开新机器就很快了。需要注意磁盘和内存创建后再调整比较麻烦，建议一开始就按需求给够。这里的磁盘是按需占用的，写 100G 不代表立刻占掉宿主机 100G 空间。

>查看和进入虚拟机

列出所有虚拟机，可以看到名字、状态和 IP：

```bash
multipass list
```

查看某台机器的详细信息，包括 CPU、内存、磁盘用量：

```bash
multipass info codeserver
```

进入虚拟机的 shell：

```bash
multipass shell codeserver
```

进去之后默认是`ubuntu`用户，有免密 sudo 权限。退出直接`exit`就回到宿主机了。

不进 shell 直接在虚拟机里执行命令也可以：

```bash
multipass exec codeserver -- ls -la
```

`--`后面接要执行的命令，适合写脚本的时候用。

>启动、停止、重启

```bash
multipass stop codeserver
multipass start codeserver
multipass restart codeserver
```

不用的时候 stop 掉可以把内存释放出来，数据不会丢，下次 start 接着用。还有一个`suspend`是挂起，恢复比冷启动快一些：

```bash
multipass suspend codeserver
```

>宿主机和虚拟机之间传文件

单个文件用`transfer`：

```bash
multipass transfer ./test.txt codeserver:/home/ubuntu/test.txt
multipass transfer codeserver:/home/ubuntu/test.txt ./test.txt
```

如果要长期共享目录，用`mount`把宿主机目录挂进去更方便：

```bash
multipass mount /opt/project codeserver:/home/ubuntu/project
```

取消挂载：

```bash
multipass umount codeserver:/home/ubuntu/project
```

>删除虚拟机

```bash
multipass delete codeserver
```

需要注意`delete`只是把虚拟机标记为已删除，`multipass list`里还能看到，状态是`Deleted`，这时候还可以用`recover`恢复：

```bash
multipass recover codeserver
```

确定不要了就执行清理，磁盘空间才会真正释放：

```bash
multipass purge
```

`purge`之后就找不回来了，执行前最好先`list`确认一下删除列表里没有还想留的机器。

>其他配置

Multipass 本身有一些全局配置，用`get`查看、`set`修改。比如后端虚拟化驱动，Linux 上默认是 QEMU，也可以换成 LXD：

```bash
multipass get local.driver
multipass set local.driver=lxd
```

所有可配置的键可以列出来看：

```bash
multipass get --keys
```
