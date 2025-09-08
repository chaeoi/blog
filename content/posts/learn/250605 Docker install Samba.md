---
title: "使用Docker安装Samba文件共享"
date: 2025-06-05
lastmod: 2025-06-05
author: ["沧海​"]
tags: ["Samba"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
- 自 1992 年以来，Samba 一直为所有使用 SMB/CIFS 协议的客户端（如所有版本的 DOS 和 Windows、OS/2、Linux 以及其他许多系统）提供安全、稳定且快速的文件和打印服务。推荐使用Docker一键部署。

```c
docker run -d \
    --name samba \
    --restart always \
    --network host \
    -v /mnt:/mnt \
    -v /opt/samba:/etc/samba \
    dperson/samba \
    -u "canghai;cK0OyUF0s38jl4BJKUmYq2Rby"
```

- 推荐配置如下

```conf
[global]
   workgroup = MYGROUP
   server string = Samba Server
   server role = standalone server
   log file = /dev/stdout
   max log size = 50
   dns proxy = no 
   pam password change = yes
   map to guest = never
   usershare allow guests = yes
   create mask = 0664
   force create mode = 0664
   directory mask = 0775
   force directory mode = 0775
   force user = smbuser
   force group = smb
   follow symlinks = yes
   load printers = no
   printing = bsd
   printcap name = /dev/null
   disable spoolss = yes
   strict locking = no
   aio read size = 0
   aio write size = 0
   vfs objects = catia fruit streams_xattr
   client ipc max protocol = SMB3
   client ipc min protocol = SMB2_10
   client max protocol = SMB3
   client min protocol = SMB2_10
   server max protocol = SMB3
   server min protocol = SMB2_10
   fruit:delete_empty_adfiles = yes
   fruit:time machine = yes
   fruit:veto_appledouble = no
   fruit:wipe_intentionally_left_blank_rfork = yes

[mmd]
   path = /mnt/mmd
   browsable = yes
   read only = no
   guest ok = no
   veto files = /.apdisk/.DS_Store/.TemporaryItems/.Trashes/desktop.ini/ehthumbs.db/Network Trash Folder/Temporary Items/Thumbs.db/
   delete veto files = yes
   valid users = canghai
   admin users = canghai
   write list = canghai
```

- 有几点需要注意的地方：

Windows 无法访问 Samba将
```c
map to guest = bad user
```
改为

```c
map to guest = never
```

禁用回收站，将
```c
vfs objects = catia fruit recycle streams_xattr
recycle:keeptree = yes
recycle:maxsize = 0
recycle:repository = .deleted
recycle:versions = yes
```
改为
```c
vfs objects = catia fruit streams_xattr
```
