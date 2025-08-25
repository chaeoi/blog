---
title: "Ubuntu自动挂载硬盘"
date: 2024-05-08
lastmod: 2024-05-08
author: ["沧海"]
tags: ["Linux"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
在Ubuntu系统中，您可以通过编辑`/etc/fstab`文件来配置在系统启动时自动挂载硬盘。

> 确认硬盘信息

在挂载硬盘之前，首先要确认硬盘的信息，包括硬盘的 UUID（通用唯一标识符）或者设备路径。您可以使用以下命令列出所有已连接的硬盘及其信息：

```shell
sudo blkid
```

> 编辑 /etc/fstab 文件

打开终端，使用文本编辑器，如 nano 或者 vi 编辑`/etc/fstab`文件：

```shell
sudo vim /etc/fstab
```

> 添加挂载信息

在`/etc/fstab`文件中添加一行来描述要挂载的硬盘。行的格式如下：

```
UUID=<硬盘UUID> <挂载点> <文件系统类型> <挂载选项> <文件系统检查顺序> <备份频率>
```

- **<硬盘UUID>**：硬盘的 UUID，可以通过上面的`blkid`命令获取。
- **<挂载点>**：硬盘挂载的目标路径，通常是`/mnt`下的子目录，如`/mnt/data`。
- **<文件系统类型>**：硬盘的文件系统类型，如`ext4`、`ntfs`等。
- **<挂载选项>**：挂载选项，通常使用`defaults`。
- **<文件系统检查顺序>**：文件系统检查顺序，通常设置为`0`以禁用检查。
- **<备份频率>**：备份频率，通常设置为`0`以禁用备份。

示例：

```
UUID=01234567-89ab-cdef-0123-456789abcdef /mnt/data ext4 defaults,nofail 0 0
```

> 测试挂载

在没有重启系统的情况下，可以通过以下命令来测试挂载：

```shell
sudo mount -a
```

> 注意

如果`fstab`配置不当，可能就会出现机器无法启动的情况，此处均添加了`nofail`参数，数据库挂载不正常也可以启动服务器。