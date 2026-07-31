---
title: "Multipass安装与命令"
date: 2026-06-11
lastmod: 2026-07-31
author: ["沧海"]
tags: ["Multipass"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
>安装Multipass

`Multipass`是 Canonical 出的轻量虚拟机管理工具，可以在命令行里快速拉起 Ubuntu 虚拟机，不用像 VMware 那样手动挂镜像装系统，一条命令就能开出一台干净的环境。这里记录一下安装、创建虚拟机以及相关命令。

Ubuntu 上直接用 snap 安装：

```bash
snap install multipass
```

安装完确认一下版本：

```bash
multipass version
```

能正常输出`multipass`和`multipassd`两个版本号就说明装好了。

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

>完整命令表

下面列出除创建虚拟机所用的`launch`之外，Multipass 当前提供的全部一级命令。

| 命令 | 说明 |
| --- | --- |
| `multipass find` | 查找可用镜像 |
| `multipass list` | 列出虚拟机 |
| `multipass info codeserver` | 查看详细信息 |
| `multipass shell codeserver` | 进入虚拟机 |
| `multipass exec codeserver -- ls -la` | 在虚拟机中执行命令 |
| `multipass start codeserver` | 启动虚拟机 |
| `multipass stop codeserver` | 关闭虚拟机 |
| `multipass restart codeserver` | 重启虚拟机 |
| `multipass suspend codeserver` | 挂起虚拟机 |
| `multipass snapshot codeserver -n backup-01` | 创建快照，需先关机 |
| `multipass restore codeserver.backup-01` | 恢复快照，需先关机 |
| `multipass clone codeserver -n codeserver-copy` | 克隆已关机的虚拟机 |
| `multipass transfer ./test.txt codeserver:/home/ubuntu/` | 传输文件 |
| `multipass mount ./project codeserver:/home/ubuntu/project` | 挂载本机目录 |
| `multipass umount codeserver:/home/ubuntu/project` | 取消挂载 |
| `multipass delete codeserver` | 删除虚拟机或快照 |
| `multipass recover codeserver` | 撤销虚拟机删除 |
| `multipass purge` | 永久清理已删除虚拟机 |
| `multipass networks` | 列出可用网络接口 |
| `multipass get local.driver` | 查看配置 |
| `multipass set local.driver=qemu` | 修改配置 |
| `multipass alias codeserver:ls ls-vm` | 创建命令别名 |
| `multipass aliases` | 列出命令别名 |
| `multipass prefer default` | 切换别名上下文 |
| `multipass unalias ls-vm` | 删除命令别名 |
| `multipass authenticate` | 认证当前客户端 |
| `multipass version` | 查看版本 |
| `multipass help snapshot` | 查看命令帮助 |
| `multipass wait-ready --timeout 30` | 等待服务就绪 |

`recover`只能撤销虚拟机删除，恢复快照要用`restore`。`purge`后的虚拟机和被删除的快照都无法恢复。

快照仍保存在本机，不能代替异机备份。Multipass 没有原生的`backup`、`export`或`import`命令。

完整参数随版本可能变化，可以用`multipass help <command>`查看本机版本支持的选项，也可以查阅[Multipass 官方命令行参考](https://documentation.ubuntu.com/multipass/latest/reference/command-line-interface/)。
