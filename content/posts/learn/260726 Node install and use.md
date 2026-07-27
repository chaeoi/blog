---
title: "Node.js安装与使用"
date: 2026-07-27
lastmod: 2026-07-27
author: ["沧海"]
tags: ["Node.js", "npm"]
description: "记录Linux和Windows下安装与使用Node.js的方法。"
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

> Linux安装Node.js

安装 fnm：

```bash
curl -o- https://fnm.vercel.app/install | bash
source ~/.bashrc
```

安装 Node.js 24，并设置为默认版本：

```bash
fnm install 24
fnm use 24
fnm default 24
```

确认版本：

```bash
node -v
npm -v
```

查看已经安装和当前使用的版本：

```bash
fnm list
fnm current
```

项目目录中可以创建`.node-version`文件：

```txt
24
```

进入项目目录后执行`fnm use`即可切换到对应版本。

> Windows安装ZIP便携版

从[Node.js官方下载页面](https://nodejs.org/download/release/latest-v24.x/)下载`win-x64.zip`，解压到：

```txt
D:\Portable Software\Node
```

在当前用户的`Path`中添加：

```txt
D:\Portable Software\Node
```

重新打开终端并确认版本：

```cmd
where node
node -v
npm -v
```

因为 ZIP 便携版不会通过安装程序另外设置 npm 的`prefix`，所以 npm 全局目录默认就是 Node 解压目录`D:\Portable Software\Node`，全局包保存在其中的`node_modules`下。

如需配置 npmmirror，新建文件：

```txt
D:\Portable Software\Node\node_modules\npm\npmrc
```

写入：

```ini
registry=https://registry.npmmirror.com/
```

> npm常用命令

设置仓库：

```bash
npm config set registry https://registry.npmmirror.com/
```

设置代理：

```bash
npm config set proxy http://127.0.0.1:10808
npm config set https-proxy http://127.0.0.1:10808
```

只在本次安装时使用代理：

```bash
npm install package-name --proxy=socks5://127.0.0.1:10808
```

查看配置：

```bash
npm config list
npm config get registry
npm config get proxy
npm config get https-proxy
```

清除代理：

```bash
npm config delete proxy
npm config delete https-proxy
```
