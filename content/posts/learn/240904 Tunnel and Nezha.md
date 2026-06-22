---
title: "利用Cloudflare Tunnel部署哪吒探针"
date: 2024-09-04
lastmod: 2024-12-02
author: ["沧海"]
tags: ["Cloudflare", "Nezha"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
从部署应用程序的那一刻起，开发人员和IT人员就要花费时间来将其封锁起来：配置ACL，轮换IP地址，使用像GRE隧道这样的~~笨拙解决方案~~。有一种更简单、更安全的方法可以保护您的应用程序和web服务器免受直接攻击：**Cloudflare Tunnel**。可以借助~~良心企业~~Cloudflare完全**免费的Tunnel服务**，快速安全地加密应用程序到任何类型基础设施的流量，让您能够隐藏你的web服务器IP地址，阻止直接攻击。Tunnel后台程序在源web服务器和Cloudflare最近的数据中心之间创建一条加密隧道，同时**无需打开任何公共入站端口**。使用防火墙锁定所有源服务器端口和协议后，HTTP/S端口上的任何请求都会被丢弃，包括容量耗尽DDoS攻击。数据泄露尝试被完全阻止，例如传输中数据窥探或暴力登录攻击。同时Tunnel支持`gRPC`的流量转发，用来配置哪吒探针也没有问题。

#### 配置Cloudflare Tunnel

Cloudflare Tunnel支持多种部署方式，并且平台的适配也很完善，具体项目信息可以从[Github](https://github.com/cloudflare/cloudflared)上查看。下面以Debian为例，简单介绍一下安装方法。

在Cloudflare Dashboard中新建一个隧道，之后你可以获得一串密钥，之后下载合适的`deb`安装包
```cmd
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm64.deb
```
然后可以直接使用一键安装
```cmd
sudo dpkg -i cloudflared.deb && sudo cloudflared service install eyJhI...
```
**注意**：这样通过官网service install一键安装的方式需要在`/etc/systemd/system/cloudflared.service`中增加`--protocol http2`参数
```cmd
ExecStart=/usr/bin/cloudflared --no-autoupdate tunnel run --protocol http2 --token eyJhI...
```

更推荐使用Docker安装，部署方式如下：
```docker
docker run -d \
    --name cloudflared \
    --restart always \
    --network host \
    cloudflare/cloudflared:latest \
    tunnel --no-autoupdate --edge-ip-version auto run --protocol http2 --token eyJhI...
```

官方只提供amd64及arm64的docker镜像，其他架构可考虑采用第三方镜像：
```docker
docker run -d \
    --name cloudflared \
    --restart always \
    --network host \
    laveao/cloudflared:latest \
    tunnel --no-autoupdate --edge-ip-version auto run --protocol http2 --token eyJhI...
```

配置隧道时如果你直接穿透web服务，你可以设置到目标端口，如`http://127.0.0.1:12345`。**反代哪吒面板**需要采用**Tunnel+Nginx**这样的组合，你的服务必须使用https协议，设置Service为`https://127.0.0.1:443`，同时需要打开**No TLS Verify**和**HTTP2 connection Origin**，并配置好**Server Name**和**HTTP Host Header**。

#### 部署哪吒探针
哪吒探针的部署[官网](https://nezha.wiki/)很详细，你可以轻松的使用Docker部署
```docker
docker run -d \
    --name nezha \
    --restart always \
    --network host \
    -v /opt/nezha/dashboard/data:/dashboard/data \
    ghcr.io/nezhahq/nezha
```
新版哪吒拉取镜像启动后在设置页面即可配置`config.yaml`，无需提前写好配置文件。

**进阶**：如果想要自定义前端主题可以添加前端文件目录的映射，具体如下：
```docker
docker run -d \
    --name nezha \
    --restart always \
    --network host \
    -v /opt/nezha/dashboard/data:/dashboard/data \
    -v /opt/nezha/dashboard/user-dist:/dashboard/user-dist \
    ghcr.io/nezhahq/nezha
```
映射后需要在GitHub下载主题并解压到`/opt/nezha/dashboard/user-dist`内并`docker restart nezha`重启容器，即可应用主题。附上新版哪吒的默认前端[Nazhe Dash](https://github.com/hamster1963/nezha-dash-react/)。

#### 部署Nginx转发
这块比较简单，可以通过监控`/proto.NezhaService`,区分不同流量，直接贴出我的配置供参考，证书位置，端口等视情况修改。
```nginx
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    http2 on;
    server_tokens off;
    ssl_certificate    /opt/cert/web.crt;
    ssl_certificate_key    /opt/cert/web.key;
    ssl_session_timeout 1d;
    ssl_session_tickets off;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    server_name status.canghai.org;

    underscores_in_headers on;
    set_real_ip_from 0.0.0.0/0;
    real_ip_header CF-Connecting-IP;
   
    location ^~ /proto.NezhaService/ {
        grpc_set_header Host $host;
        grpc_set_header nz-realip $http_cf_connecting_ip;
        grpc_read_timeout 600s;
        grpc_send_timeout 600s;
        grpc_socket_keepalive on;
        client_max_body_size 10m;
        grpc_buffer_size 4m;
        grpc_pass grpc://dashboard;
    }

    location ~* ^/api/v1/ws/(server|terminal|file)(.*)$ {
        proxy_set_header Host $host;
        proxy_set_header nz-realip $http_cf_connecting_ip;
        proxy_set_header Origin https://$host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
        proxy_pass http://127.0.0.1:5015;
    }

    location / {
        proxy_set_header Host $host;
        proxy_set_header nz-realip $http_cf_connecting_ip;
        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
        proxy_buffer_size 128k;
        proxy_buffers 4 256k;
        proxy_busy_buffers_size 256k;
        proxy_max_temp_file_size 0;
        proxy_pass http://127.0.0.1:5015;
    }
}

upstream dashboard {
    server 127.0.0.1:5015;
    keepalive 512;
}
```

#### 探针自定义代码
- 前端自定义
```html
<link rel="stylesheet" href="https://registry.npmmirror.com/lxgw-wenkai-screen-web/latest/files/style.css" />
<style>
  * {
    font-family: LXGW WenKai Screen !important;
  }

  #root > div > main > div.mx-auto.w-full.max-w-5xl.px-0 > div > section > button:nth-child(1) {
    display: none !important;
  }

  #root > div > main > div.mx-auto.w-full.max-w-5xl.px-0 > div > section > button:nth-child(2) {
    display: none !important;
  }

  #root > div > main > div:nth-child(1) > section.mt-10.flex.flex-col.md\:mt-16.header-timer {
    display: none !important;
  }
  
  #root > div > main > div:nth-child(1) > section.flex.items-center.justify-between.header-top > section.flex.items-center.gap-2.header-handles > div {
    display: none !important;
  }

  #root > div > main > div:nth-child(1) > div > div {
    display: none !important;
  }

  #root > div > main > div.mx-auto.w-full.max-w-5xl.px-0 > section.grid.grid-cols-2.gap-4.lg\:grid-cols-4.server-overview {
    display: none !important;
  }

  #root > div > main > footer > section {
    display: none !important;
  }
</style>
<script>
  window.CustomLogo = "https://status.canghai.org/dashboard/logo.svg";
  window.ForceUseSvgFlag = true;
  window.ForcePeakCutEnabled = true;
  const isMobile = window.matchMedia("(max-width: 768px)").matches;
  window.ForceCardInline = !isMobile;
</script>
```
- 后端自定义
```html
<link rel="stylesheet" href="https://registry.npmmirror.com/lxgw-wenkai-screen-web/latest/files/style.css" />
<style>
    html {
        font-family: LXGW WenKai Screen !important;
    }
</style>
<script>
    window.DisableAnimatedMan = true;
</script>
```