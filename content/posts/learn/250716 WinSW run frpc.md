---
title: "使用WinSW将frpc注册为Windows服务"
date: 2025-07-16
lastmod: 2025-07-16
author: ["沧海"]
tags: ["WinSW", "FRP", "Windows"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

`WinSW`可以把普通 Windows 程序包装成系统服务，适合让一些命令行程序开机自启。这里以`frpc`为例，把 FRP 客户端注册为 Windows 服务。

WinSW 下载地址：

- 项目地址：<https://github.com/winsw/winsw>
- Releases：<https://github.com/winsw/winsw/releases>
- 64位 Windows 可下载：<https://github.com/winsw/winsw/releases/latest/download/WinSW-x64.exe>

下载`WinSW-x64.exe`后，建议重命名为`frpc-service.exe`，这样可以和真正的`frpc.exe`区分开。

目录可以这样放：

```txt
C:\frp
├── frpc.exe
├── frpc.toml
├── frpc-service.exe
└── frpc-service.xml
```

`frpc.toml`示例：

```toml
serverAddr = "1.2.3.4"
serverPort = 5031
auth.token = "password"

[[proxies]]
name = "ssh"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = 6000
```

新建`frpc-service.xml`：

```xml
<service>
  <id>frpc</id>
  <name>frpc</name>
  <description>Frpc Service</description>
  <executable>%BASE%\frpc.exe</executable>
  <onfailure action="restart" delay="10 sec"/>
  <arguments>-c frpc.toml</arguments>
  <startmode>Automatic</startmode>
  <log mode="none"></log>
</service>
```

其中`%BASE%`表示 WinSW 程序所在目录，也就是`C:\frp`。`executable`指定真正要运行的程序，这里是`frpc.exe`。`arguments`表示启动参数，`-c frpc.toml`表示使用同目录下的`frpc.toml`配置文件。

以管理员身份打开 CMD，进入目录：

```cmd
cd /d C:\frp
```

安装服务：

```cmd
frpc-service.exe install
```

启动服务：

```cmd
frpc-service.exe start
```

查看状态：

```cmd
frpc-service.exe status
```

重启服务：

```cmd
frpc-service.exe restart
```

停止服务：

```cmd
frpc-service.exe stop
```

卸载服务：

```cmd
frpc-service.exe uninstall
```

如果修改了`frpc-service.xml`，可以执行：

```cmd
frpc-service.exe refresh
```

如果只是修改`frpc.toml`，一般重启服务即可：

```cmd
frpc-service.exe restart
```

服务启动失败时，可以先在 CMD 中手动运行一次：

```cmd
frpc.exe -c frpc.toml
```

如果手动运行也失败，优先检查`serverAddr`、`serverPort`、`auth.token`和防火墙。确认手动运行正常后，再交给 WinSW 托管。
