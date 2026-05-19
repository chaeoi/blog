---
title: "简单的Nginx配置"
date: 2022-10-15
lastmod: 2025-09-08
author: ["沧海"]
tags: ["Docker", "Nginx"]
description: ""
comments: true
showToc: false
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
#### Debian下快速安装Nginx
- 推荐使用docker安装
```c
mkdir -p /opt/nginx/config && vim /opt/nginx/config/nginx.conf
```
```conf
user nginx;
worker_processes auto;

error_log /var/log/nginx/error.log notice;
pid /var/run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log  main;

    sendfile on;
    tcp_nopush on;

    keepalive_timeout 65;

    gzip on;

    include /etc/nginx/conf.d/*.conf;
}

stream{
    log_format proxy '$remote_addr [$time_local] '
                     '$protocol $status $bytes_sent $bytes_received '
                     '$session_time "$upstream_addr" '
                     '"$upstream_bytes_sent" "$upstream_bytes_received" "$upstream_connect_time"';
    
    access_log /var/log/nginx/stream.log proxy;

    include /etc/nginx/conf.d/*.stream;
}
```
```docker
docker run -d \
    --name nginx \
    --restart always \
    --network host \
    -v /opt/nginx/config/conf.d:/etc/nginx/conf.d \
    -v /opt/nginx/config/nginx.conf:/etc/nginx/nginx.conf \
    -v /opt/nginx/log:/var/log/nginx \
    -v /opt/nginx/cert:/opt/cert \
    nginx:stable
```

#### 配置Cloudflare证书
- 编辑证书
```c
vim /opt/nginx/cert/web.crt
```
```cmd
-----BEGIN CERTIFICATE-----
MIIEFTCCAv2gAwIBAgIUTNa8COmbEvnnmyHFCWCSNbIlrowwDQYJKoZIhvcNAQEL
BQAwgagxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQH
Ew1TYW4gRnJhbmNpc2NvMRkwFwYDVQQKExBDbG91ZGZsYXJlLCBJbmMuMRswGQYD
VQQLExJ3d3cuY2xvdWRmbGFyZS5jb20xNDAyBgNVBAMTK01hbmFnZWQgQ0EgNjdh
NGUxOWE1YjBlYzNlOGEyMGI0MDNiZTQzYmI4OTcwHhcNMjIwODIyMDU0NzAwWhcN
MzcwODE4MDU0NzAwWjAiMQswCQYDVQQGEwJVUzETMBEGA1UEAxMKQ2xvdWRmbGFy
ZTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAL/Sy+00iQlMJYRCHTss
LxUbTn3DvAIUGvK8IPuYTl24ZyGVJXjI3mq10/3slUkUJLKjowScaMwwVCHwsM8M
rWIc97zRec8mjQA7ixayPSEtkXoaRC403nQEDdUqtY5bgpug6PFzZv2tUy3yxVG8
KLQze+O1kIkw+pXvCL9nxPUEZ2SBZgDV/uBnQAZU1iZaQXtZPJaUxjQ1Wn+IBnj0
KZUp7JidkaajcgmDq63gO++Ce6qNqQSHRNnfjFB9OoCcPR2zHOkn+cih9yNyM5XC
WCFKuPhHhuvqeIFTrt6lHMwtUrPzeq1Q63r5FemOZl45gj8NFvdoAcRF2eaJWHBI
vGUCAwEAAaOBuzCBuDATBgNVHSUEDDAKBggrBgEFBQcDAjAMBgNVHRMBAf8EAjAA
MB0GA1UdDgQWBBRnPvIl0HzOeOYRAYO0dUqWcZg5ZDAfBgNVHSMEGDAWgBRqKMUf
A2v8OOV10yFDxd0H0UD7ODBTBgNVHR8ETDBKMEigRqBEhkJodHRwOi8vY3JsLmNs
b3VkZmxhcmUuY29tL2I2M2NhZGM0LTU4Y2QtNGRhMC05ZmZlLTE0YTEzODIxMTI3
NS5jcmwwDQYJKoZIhvcNAQELBQADggEBAAG6uWdDYpG9+9r3hMbCYUpA1hQOwh2D
viFQLgChN8OvwpAfL0MHBxpafaxOYAVODc/gLgYacAHOnYTytvX6NG6p98FivG/X
qpH2CqxUTY9zgD4aMNFSXA1LvVJ//0JcAMVe1P8MdtnIO419On3sQYwTVMTgO+Qw
mcjxX8JHQjhohO1xRdJutBA1fbcHzymjHdcIstabxyZf1eG3nLvF9SZcA8GSCSu7
5QWVWM28EVR+VSAxMvXNbnkkuaRY2dOJdFKfc8SG9lSA/nCHprz6F32TKP4HZQay
GoxJuobHKaeI/K3WjQlDH1NAd2LWvIsPzvUBc9ugSR2DWsXYvgHs1qc=
-----END CERTIFICATE-----
```
- 编辑私钥
```c
vim /opt/nginx/cert/web.key
```
```cmd
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQC/0svtNIkJTCWE
Qh07LC8VG059w7wCFBryvCD7mE5duGchlSV4yN5qtdP97JVJFCSyo6MEnGjMMFQh
8LDPDK1iHPe80XnPJo0AO4sWsj0hLZF6GkQuNN50BA3VKrWOW4KboOjxc2b9rVMt
8sVRvCi0M3vjtZCJMPqV7wi/Z8T1BGdkgWYA1f7gZ0AGVNYmWkF7WTyWlMY0NVp/
iAZ49CmVKeyYnZGmo3IJg6ut4DvvgnuqjakEh0TZ34xQfTqAnD0dsxzpJ/nIofcj
cjOVwlghSrj4R4br6niBU67epRzMLVKz83qtUOt6+RXpjmZeOYI/DRb3aAHERdnm
iVhwSLxlAgMBAAECggEABKroJZzGWHcSLKGaA3zj2MzuzOmf4AisNTIPUJmLddIm
x3u3fmgCdxdDxTIZQD6tiLlDN+zN/s+OpIeUtDarQclrQwMLEz4HAIJYzccT7Ldz
cRF4wKUU5D7ONVzFYsuzcstodhBoT6dRie5jD1+XbMPmvlfTLTttxYs1xWG/7aeD
saHbzN7lUiWu5GWgbBYHkeU6KOr83MakGVI82Z6sCJhuQe6fTlvjiqsF5GtyvrZ8
N2BZZVleCWAE0uXntFmezWI6HvtxSBBFSNbqequr3r0IjL8qK8qvJ2k3xYLidERi
JX5jDJ+8opSN++Ia+lcpVZQ9F9jGJfpzKvPObVFqWwKBgQDfWepiT9nXAg0UM00y
+01JXogm6aHWDgBkQTqjGyC5cuHzF3TssY7pGtyoy7OAcsEqgpyEnUTsd7WbN6Ux
Pj+TpVULOtFlJrQR+zIWWCCfC6byXw/S8m9GpBvZescYW4QkOEtQlLIi7HUYlm+Y
dHy9eWJewn5b1bI0d4WX6UhQDwKBgQDb3RIa3hAcdsKryZJNN6j/QJFQBYnv+n5Y
UXrfFEAsdX2RU0xO8otss51OMrkbZS8qCmTC+hZgvYeUhAKf9SRnO/BH18sZskNW
psws2Lq6MM0ZOjZqeVoaEvf6ESLf/bbO/IKoRbtdWdCNilQ0FYpJE27MKcLQ+miB
AjXbego4SwKBgQDODxeFkiPpEHRekaIEigLY0MUOGXf8kzhbRi7B8jIzxcCd1KNE
B+BQQT8Y364AsF50SMH8O1guTZcX17OpBcQEBIG8dYxgJN/2wuiH4tBdy5M/guKH
fLGa26bx8ysh1rTH8cPSWQ0r2TmC8K+OWNIIwKc3w3puYW4ip65x44CakwKBgQCi
91URJyIoBvs6nBleNPCF6pULDF/2yeRWkGaT7Y23poqham24Yt1ngCcMLFq6bKCt
97BCOV7W7AUP112etPT7tBjhF5mKfXCeTNowL6EQm1Wa6mQlPbfEdeTqrUL9ZjDX
caFjGvTLN+R21V6ekIzEp6vLlvS5M7K8VSgYe3gRywKBgDz3aSUDhCC3zLvpFlW6
jDX4bOjKoOidWdOdJKA8jtv57c7DhWTtz1Pd+9uf9xvx1dwKmMrvsVmoyd2hmWXq
ZrTlwbmZlo1CqWQ8yXzFBPXRXAc+ZJvmzUWIOBS6+i8qo7UuZIHucsJGDlFbvAfE
wz7MQiazno/JrI0hYXi3jwcG
-----END PRIVATE KEY-----
```

#### 快速配置Nginx
按照推荐的`nginx.conf`配置，其中**http**模块包含`/etc/nginx/conf.d/`内的所有`*.conf`文件的配置，**stream**模块包含`/etc/nginx/conf.d/`内的所有`*.stream`文件的配置，下面有几个例子可以参考
- 转发从**任意域名**访问服务器**80**、**443**端口到 ~~404~~
```nginx
server {
    listen 80;
    listen [::]:80;
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
    location / {
        return 404;
    }
}
```
- 转发从**example.com**访问服务器**443**端口到 ~~127.0.0.1:0000~~
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
    server_name example.com;
    location / {
        proxy_pass http://127.0.0.1:0000;
        proxy_redirect off;
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
- 转发从**example.com**访问服务器**443**端口到 ~~/var/www/example.com/index.html~~
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
    server_name example.com;
    root /var/www/example.com;
    index index.html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```
- 转发所有`TCP/UDP`流量到指定地址
```nginx
server{
    listen [::]:12345;
    proxy_ssl on;
    proxy_ssl_protocols TLSv1.2 TLSv1.3;
    proxy_ssl_ciphers HIGH:!aNULL:!MD5;
    proxy_pass smtp.larksuite.com:465;
}
```
#### 重新加载Nginx
```docker
docker restart nginx
```
更多详细进阶用法请访问 [Nginx](https://github.com/nginx/nginx) 。