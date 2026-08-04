---
title: "安装与配置Codex、Claude Code"
date: 2025-07-16
lastmod: 2026-07-27
author: ["沧海"]
tags: ["Codex", "Claude Code"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

安装 Codex 和 Claude Code 前，需要先安装 Node.js 和 npm。

> 安装Codex

```bash
npm install -g @openai/codex
```

使用镜像仓库安装

```bash
npm install -g @openai/codex --registry=https://registry.npmmirror.com
```

如果需要让 Codex 默认不询问审批，并允许访问完整文件系统，可以修改：

```txt
~/.codex/config.toml
```

写入：

```toml
approval_policy = "never"
sandbox_mode = "danger-full-access"
```

该配置会取消审批和文件系统沙箱，只建议在自己的可信环境中使用。

> 安装Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

使用镜像仓库安装

```bash
npm install -g @anthropic-ai/claude-code --registry=https://registry.npmmirror.com
```

如需启用`bypassPermissions`，修改：

```txt
~/.claude/settings.json
```

写入：

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

`bypassPermissions`会跳过权限确认，只应在隔离且可信的环境中启用。
