---
title: "通过跳板机实现SSH透明代理"
date: 2026-08-11
lastmod: 2026-08-11
author: ["沧海"]
tags: ["SSH"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

访问多台内网服务器时，可以使用 SSH 跳板机统一转发连接。本文先为跳板机配置独立密钥，再把跳板机、密钥和目标主机规则写入本机 SSH 配置。

> 生成跳板机密钥

先创建本机 SSH 目录并设置权限：

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

为跳板机单独生成一对 ED25519 密钥：

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_bastion -C "bastion"
```

生成后会得到两个文件：

- `~/.ssh/id_ed25519_bastion`：私钥，只能保存在本机，不能发给别人。
- `~/.ssh/id_ed25519_bastion.pub`：公钥，可以安装到跳板机。

命令会询问是否设置密钥口令。密钥口令用于保护本机私钥，不是跳板机的账户密码。建议设置密钥口令；如果同名密钥已经存在，不要直接覆盖，应先确认它是否仍在使用。

> 将公钥安装到跳板机

通过跳板机当前的密码认证安装公钥：

```bash
ssh-copy-id -i ~/.ssh/id_ed25519_bastion.pub \
  -p 2222 jumpuser@jump.example.com
```

各参数的含义如下：

- `-i ~/.ssh/id_ed25519_bastion.pub`：指定要安装的公钥。
- `-p 2222`：指定跳板机的 SSH 端口。
- `jumpuser@jump.example.com`：跳板机的用户名和地址。

首次连接时，SSH 会先要求确认跳板机的主机指纹，然后提示输入一次跳板机账户密码。安装成功后，公钥会写入跳板机用户的`~/.ssh/authorized_keys`。

在写入配置文件前，可以直接测试密钥：

```bash
ssh -i ~/.ssh/id_ed25519_bastion \
  -o IdentitiesOnly=yes \
  -p 2222 jumpuser@jump.example.com
```

如果私钥设置了密钥口令，此时可能要求输入该口令。这是在本机解锁私钥，不是再次输入跳板机账户密码。

可以把私钥加入 SSH 代理，避免在同一登录会话中反复输入密钥口令：

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_bastion
```

> 配置带密钥的跳板机

编辑本机 SSH 配置文件：

```bash
nano ~/.ssh/config
```

写入以下配置，并将示例值替换为实际值：

```sshconfig
# 跳板机：始终使用指定的独立密钥
Host bastion
    HostName jump.example.com
    User jumpuser
    Port 2222
    IdentityFile ~/.ssh/id_ed25519_bastion
    IdentitiesOnly yes

# 目标主机统一通过跳板机连接
Host server??.local
    User targetuser
    ProxyJump bastion
```

保存后设置配置文件权限：

```bash
chmod 600 ~/.ssh/config
```

`IdentityFile`指定刚才生成的私钥，`IdentitiesOnly yes`要求 SSH 只使用该条目指定的身份凭据连接跳板机，避免尝试过多其他密钥。

如果不同目标主机使用不同用户名，可以删除`User targetuser`，在连接时指定用户名：

```bash
ssh anotheruser@server01.local
```

`server??.local`中的两个`?`分别匹配一个字符，因此可以匹配`server01.local`、`server25.local`和`server99.local`。它也可能匹配由其他两个字符组成的名称；如果必须严格限定主机范围，可以在同一个`Host`后逐一列出允许的主机名。

> 连接目标主机

先验证跳板机别名：

```bash
ssh bastion
```

再连接目标主机：

```bash
ssh server01.local
ssh server25.local
ssh server99.local
```

SSH 会使用指定私钥完成跳板机认证，再由跳板机连接目标服务器。跳板机配置密钥后，通常不会再要求输入跳板机账户密码；如果目标主机仍使用密码认证，SSH 接下来还会提示输入目标机账户密码。

跳板机密钥只负责登录跳板机，不会自动让目标主机也免密。若目标主机同样需要密钥登录，应为目标主机单独配置和安装密钥。

> 常见检查

查看目标主机最终采用的 SSH 配置：

```bash
ssh -G server01.local | grep -E \
  '^(hostname|user|port|proxyjump|identityfile) '
```

查看详细连接过程：

```bash
ssh -v server01.local
```

如果跳板机可以登录但目标主机无法连接，应确认目标主机名能否被跳板机解析、跳板机能否访问目标机的 SSH 端口，以及防火墙是否放行相应连接。
