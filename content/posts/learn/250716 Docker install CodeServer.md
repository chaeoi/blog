---
title: "使用Docker部署CodeServer，并配置Codex、Claude Code"
date: 2025-07-16
lastmod: 2025-07-16
author: ["沧海"]
tags: ["CodeServer", "Codex", "Claude Code"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

`CodeServer`可以把 VS Code 跑在服务器上，通过浏览器直接访问。这里记录一下用 Docker 部署 `CodeServer`，并在容器里安装 `fnm`、Node.js、Codex 和 Claude Code 的步骤。

```bash
docker run -d \
    --name codeserver \
    --restart always \
    --network host \
    --privileged \
    --user root \
    -e DOCKER_USER=$USER \
    -e PORT=5068 \
    -e PASSWORD=password \
    -e all_proxy=socks5h://127.0.0.1:10808 \
    -v /opt/codeserver/root:/root \
    -v /opt/codeserver/project:/opt/codeserver/project \
    codercom/code-server:latest
```

需要注意几点：

- `PORT=5068`是网页访问端口，启动后访问`http://服务器IP:5068`
- `PASSWORD`改成自己的密码
- `--network host`会直接使用宿主机网络，配置`all_proxy=socks5h://127.0.0.1:10808`时，容器内的`127.0.0.1`就是宿主机
- `/opt/codeserver/root`用于持久化`/root`目录，重建容器后配置不会丢
- `/opt/codeserver/project`用于存放项目文件


安装`fnm`：

```bash
curl -o- https://fnm.vercel.app/install | bash
```

安装完成后重新打开终端，或者执行：

```bash
source ~/.bashrc
```

安装 Node.js 24：

```bash
fnm install 24
```

确认版本：

```bash
node -v
npm -v
```

安装 Codex：

```bash
npm install -g @openai/codex
```

安装 Claude Code：

```bash
npm install -g @anthropic-ai/claude-code
```

如果需要让 Codex 默认不询问审批，并且允许完整文件系统访问，可以修改`~/.codex/config.toml`：

```toml
approval_policy = "never"
sandbox_mode = "danger-full-access"
```

这个配置只建议在自己的可信环境中使用。`approval_policy = "never"`表示不再弹出审批请求，`sandbox_mode = "danger-full-access"`表示取消文件系统沙箱限制。

Claude Code的`bypassPermissions`需要在环境变量中添加`IS_SANDBOX=1`才能正常开启。

```json
{
  "env": {
    "IS_SANDBOX": "1"
  },
  "permissions": {
    "defaultMode": "bypassPermissions"
  }
}
```

！！！仅建议在容器或是沙盒内开启**full-access**