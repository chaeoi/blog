---
title: "使用Docker Compose安装Sub2API"
date: 2026-04-03
lastmod: 2026-04-03
author: ["沧海"]
tags: ["Sub2API"]
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---

这里记录一下使用 Docker Compose 部署`Sub2API`，同时启动`PostgreSQL`和`Redis`。下面配置里的密码和密钥都是随机示例，实际部署时也可以按自己的习惯重新生成。

新建目录：

```bash
mkdir -p /opt/sub2api
cd /opt/sub2api
```

新建`docker-compose.yml`：

```yaml
services:
  sub2api:
    image: weishaw/sub2api:latest
    container_name: sub2api
    restart: unless-stopped
    ulimits:
      nofile:
        soft: 100000
        hard: 100000
    ports:
      - "127.0.0.1:5090:5090"
    volumes:
      - ./data:/app/data
    environment:
      - AUTO_SETUP=true
      - SERVER_HOST=0.0.0.0
      - SERVER_PORT=5090
      - SERVER_MODE=release
      - RUN_MODE=simple

      - DATABASE_HOST=postgres
      - DATABASE_PORT=5432
      - DATABASE_USER=sub2api
      - DATABASE_PASSWORD=6f4b8c1d9e2a7f5c3b1d8e4a9c2f7b5e6d3a1c8f4b9e2d7a5c1f8e3b6d9a4c2
      - DATABASE_DBNAME=sub2api
      - DATABASE_SSLMODE=disable
      - DATABASE_MAX_OPEN_CONNS=256
      - DATABASE_MAX_IDLE_CONNS=128
      - DATABASE_CONN_MAX_LIFETIME_MINUTES=30
      - DATABASE_CONN_MAX_IDLE_TIME_MINUTES=5

      - REDIS_HOST=redis
      - REDIS_PORT=6379
      - REDIS_PASSWORD=8c2f7b5e1d4a9c3f6b8e2d7a1c5f9b4e3d8a2c7f1b6e4d9a5c3f8b2e7d1a6c4
      - REDIS_DB=0
      - REDIS_POOL_SIZE=4096
      - REDIS_MIN_IDLE_CONNS=256
      - REDIS_ENABLE_TLS=false

      - ADMIN_EMAIL=admin@example.com
      - ADMIN_PASSWORD=password

      - JWT_SECRET=4d8a2f7c1e5b9d3a6c8f2b7e1d4a9c5f3b8e2d7a1c6f4b9d5e3a8c2f7b1d6a4e
      - JWT_EXPIRE_HOUR=24

      - TOTP_ENCRYPTION_KEY=7b1e4d9a2c6f8b3d5e1a7c4f9b2d6e8a3c5f1b7d4e9a2c8f6b3d1e5a7c4f9b2d

      - TZ=Asia/Shanghai

      - GEMINI_OAUTH_CLIENT_ID=
      - GEMINI_OAUTH_CLIENT_SECRET=
      - GEMINI_OAUTH_SCOPES=
      - GEMINI_QUOTA_POLICY=

      - GEMINI_CLI_OAUTH_CLIENT_SECRET=
      - ANTIGRAVITY_OAUTH_CLIENT_SECRET=

      - SECURITY_URL_ALLOWLIST_ENABLED=false
      - SECURITY_URL_ALLOWLIST_ALLOW_INSECURE_HTTP=false
      - SECURITY_URL_ALLOWLIST_ALLOW_PRIVATE_HOSTS=false
      - SECURITY_URL_ALLOWLIST_UPSTREAM_HOSTS=

      - UPDATE_PROXY_URL=socks5h://172.17.0.1:10809

    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy

    networks:
      - sub2api-network

    healthcheck:
      test: ["CMD", "wget", "-q", "-T", "5", "-O", "/dev/null", "http://localhost:5090/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 30s

  postgres:
    image: postgres:18-alpine
    container_name: sub2api-postgres
    restart: unless-stopped
    ulimits:
      nofile:
        soft: 100000
        hard: 100000
    volumes:
      - ./postgres_data:/var/lib/postgresql/data

    environment:
      - POSTGRES_USER=sub2api
      - POSTGRES_PASSWORD=6f4b8c1d9e2a7f5c3b1d8e4a9c2f7b5e6d3a1c8f4b9e2d7a5c1f8e3b6d9a4c2
      - POSTGRES_DB=sub2api
      - PGDATA=/var/lib/postgresql/data
      - TZ=Asia/Shanghai

    networks:
      - sub2api-network

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U sub2api -d sub2api"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 10s

  redis:
    image: redis:8-alpine
    container_name: sub2api-redis
    restart: unless-stopped
    ulimits:
      nofile:
        soft: 100000
        hard: 100000
    volumes:
      - ./redis_data:/data

    command: >
      sh -c '
        redis-server
        --save 60 1
        --appendonly yes
        --appendfsync everysec
        --requirepass "8c2f7b5e1d4a9c3f6b8e2d7a1c5f9b4e3d8a2c7f1b6e4d9a5c3f8b2e7d1a6c4" '

    environment:
      - TZ=Asia/Shanghai
      - REDISCLI_AUTH=8c2f7b5e1d4a9c3f6b8e2d7a1c5f9b4e3d8a2c7f1b6e4d9a5c3f8b2e7d1a6c4

    networks:
      - sub2api-network

    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 5s

networks:
  sub2api-network:
    driver: bridge
    enable_ipv6: true
    ipam:
      config:
        - subnet: "2001:db8::/64"
```

启动：

```bash
docker compose up -d
```

查看状态：

```bash
docker compose ps
```

查看日志：

```bash
docker compose logs -f sub2api
```

需要注意，`docker-compose.yml`里配置的`UPDATE_PROXY_URL`不一定会生效。如果更新检查或拉取远端数据仍然没有走代理，可以在配置文件里单独加`update.proxy_url`。

本文挂载的是：

```yaml
volumes:
  - ./data:/app/data
```

所以容器内读取的是`/app/data/config.yaml`，对应宿主机就是当前目录下的`./data/config.yaml`，添加如下字段：

```yaml
update:
  proxy_url: "socks5h://172.17.0.1:10809"
```

保存后重启`sub2api`：

```bash
docker compose restart sub2api
```

这里把服务端口绑定到了`127.0.0.1:5090`，所以只能本机访问。如果需要公网或局域网访问，可以改成`5090:5090`，或者继续保持本机监听，再通过 Nginx、Caddy 等反向代理出去。

配置里开启了 IPv6 自定义网络：

```yaml
networks:
  sub2api-network:
    driver: bridge
    enable_ipv6: true
    ipam:
      config:
        - subnet: "2001:db8::/64"
```

`2001:db8::/64`是示例地址段，如果需要真实 IPv6 通信，需要替换成自己的可用地址段。
