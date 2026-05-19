---
title: "Bitwarden之Docker部署"
date: 2022-08-16
lastmod: 2022-08-16
author: ["沧海"]
tags: ["Docker", "Bitwarden"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
[Vaultwarden](https://github.com/dani-garcia/vaultwarden)为民间基于开源项目[Bitwarden](https://github.com/bitwarden/server)开发，对服务器配置要求低，更适合个人使用
- 启动容器
```c
docker run -d \
    --name vaultwarden \
    --restart always \
    --network host \
    -e ROCKET_PORT=5002 \
    -v /opt/vaultwarden/data:/data \
    vaultwarden/server:latest
```
- 配置文件位于`/opt/vaultwarden/data/config.json`，参考如下
```json
{
	"domain": "https://example.com",
    "sends_allowed": true,
	"incomplete_2fa_time_limit": 3,
	"disable_icon_download": false,
	"signups_allowed": true,
	"signups_verify": false,
	"signups_verify_resend_time": 3600,
	"signups_verify_resend_limit": 6,
	"invitations_allowed": true,
	"emergency_access_allowed": true,
	"password_iterations": 100000,
	"password_hints_allowed": true,
	"show_password_hint": false,
	"admin_token": "Password",
	"invitation_org_name": "Vaultwarden",
	"ip_header": "X-Real-IP",
	"icon_redirect_code": 302,
	"icon_cache_ttl": 2592000,
	"icon_cache_negttl": 259200,
	"icon_download_timeout": 10,
	"icon_blacklist_non_global_ips": true,
	"disable_2fa_remember": false,
	"authenticator_disable_time_drift": false,
	"require_device_email": false,
	"reload_templates": false,
	"log_timestamp_format": "%Y-%m-%d %H:%M:%S.%3f",
	"disable_admin_token": false,
	"_enable_yubico": true,
	"_enable_duo": false,
	"_enable_smtp": true,
	"smtp_host": "smtp.mail.ru",
	"smtp_security": "force_tls",
	"smtp_port": 465,
	"smtp_from": "username@mail.ru",
	"smtp_from_name": "username",
	"smtp_username": "username@mail.ru",
	"smtp_password": "Password",
	"smtp_auth_mechanism": "\"Login\"",
	"smtp_timeout": 15,
	"smtp_accept_invalid_certs": false,
	"smtp_accept_invalid_hostnames": false,
	"_enable_email_2fa": true,
	"email_token_size": 6,
	"email_expiration_time": 600,
	"email_attempts_limit": 3
}
```