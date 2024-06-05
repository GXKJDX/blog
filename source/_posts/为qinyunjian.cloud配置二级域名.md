---
title: 为qinyunjian.cloud配置二级域名
date: 2024-06-04 22:32:50
tags:
---
### 为`qinyunjian.cloud`配置二级域名

​	前提，我现在购买了一个域名`qinyunjian.cloud`，现在我需要为这个域名添加二级域名`hotel.qinyunjian.cloud`，同时可以把前端Web应用和后端服务部署到这个域名。

#### 步骤 1: 确保域名解析

​	首先，确保您的二级域名 `hotel.qinyunjian.cloud` 已经通过 DNS 正确解析到您的服务器的公网 IP 地址。我这里购买的是阿里云的域名，所以需要到阿里云解析中心，添加记录。

​	1、点击域名解析。2、点击你的域名。3、点击添加记录。

![](https://qinyunjian-1316017204.cos.ap-guangzhou.myqcloud.com/images/typora/image-20240226093320058.png)

#### 步骤 2: 使用 Certbot 获取证书

​	运行 Certbot 并选择 Nginx 插件来自动获取和配置 Let's Encrypt SSL 证书：

```shell
sudo certbot --nginx -d hotel.qinyunjian.cloud
```

这个命令会自动为您的二级域名 `hotel.qinyunjian.cloud` 获取 SSL 证书，并更新 Nginx 的配置文件来使用这个证书。

#### 步骤3：配置`/etc/nginx/conf.d`文件

​	编辑您的 Nginx 配置文件（通常位于 `/etc/nginx/nginx.conf` 或 `/etc/nginx/sites-available/` 目录下的某个文件），添加一个新的 `server` 块，以便将 `hotel.qinyunjian.cloud` ，（以下文件是需要自己新建的）的请求代理到您的 SpringBoot 服务：

![](https://qinyunjian-1316017204.cos.ap-guangzhou.myqcloud.com/images/typora/9132726f9a8ad29222730bed828cae8.png)

​	以下是配置文件详细说明。

```
# 重定向所有 HTTP 请求到 HTTPS
server {
    listen 80; # 监听 80 端口
    server_name hotel.qinyunjian.cloud; # 为 hotel.qinyunjian.cloud 配置
    return 301 https://$host$request_uri; # 将所有 HTTP 请求重定向到 HTTPS
}

# 处理 HTTPS 请求
server {
    listen 443 ssl http2; # 监听 443 端口，启用 SSL 和 HTTP/2
    server_name hotel.qinyunjian.cloud; # 为 hotel.qinyunjian.cloud 配置

    # SSL 证书配置
    ssl_certificate /etc/letsencrypt/live/hotel.qinyunjian.cloud/fullchain.pem; # 指定证书文件
    ssl_certificate_key /etc/letsencrypt/live/hotel.qinyunjian.cloud/privkey.pem; # 指定证书密钥文件

    # SSL 性能优化配置
    ssl_session_cache shared:SSL:10m; # 启用 SSL 会话缓存，提高性能
    ssl_session_timeout 10m; # 设置 SSL 会话的超时时间
    ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-CHACHA20-POLY1305'; # 指定加密套件
    ssl_prefer_server_ciphers on; # 服务器优先选择加密算法

    # Web 应用根目录
    root /var/www/hotelweb; # 设置 Web 应用的根目录

    # 前端应用路由处理
    location / {
        try_files $uri $uri/ /index.html; # 尝试返回请求的文件或目录，如果不存在则返回 index.html
    }

    # 代理 /service 路径到后端 Spring Boot 应用
    location /service {
        rewrite ^/service(.*) /$1 break; # 重写 URL，去除 /service 前缀
        proxy_pass http://localhost:84; # 代理请求到本地的 84 端口
        proxy_http_version 1.1; # 使用 HTTP/1.1 与代理服务器通信
        proxy_set_header Upgrade $http_upgrade; # 传递升级头部，用于 WebSocket 支持
        proxy_set_header Connection 'upgrade'; # 传递连接头部，同样用于 WebSocket
        proxy_set_header Host $host; # 传递原始请求的 Host 头部
        proxy_cache_bypass $http_upgrade; # 绕过缓存处理 WebSocket 请求
    }
}
```

#### 第一个 server 块

1. `listen 80;`：这行配置指示 Nginx 监听 80 端口（HTTP）上的请求。
2. `server_name hotel.qinyunjian.cloud;`：定义了此配置块处理请求的域名。
3. `return 301 https://$host$request_uri;`：所有 http (80 端口) 的请求将被永久重定向到 https 版本的相同 URI。这是一种常见的做法，用于强制使用 HTTPS。

#### 第二个 server 块

1. `listen 443 ssl http2;`：这行配置指示 Nginx 监听 443 端口上的请求，并启用 SSL 和 HTTP/2 支持。443 端口是 HTTPS 默认使用的端口。
2. `server_name hotel.qinyunjian.cloud;`：同样定义了此配置块处理请求的域名。
3. `ssl_certificate /etc/letsencrypt/live/hotel.qinyunjian.cloud/fullchain.pem;`：指定 SSL 证书的位置。这个证书用于 HTTPS 加密。
4. `ssl_certificate_key /etc/letsencrypt/live/hotel.qinyunjian.cloud/privkey.pem;`：指定 SSL 证书的私钥位置。
5. `ssl_session_cache shared:SSL:10m;`：启用 SSL 会话缓存，以提高后续请求的性能。
6. `ssl_session_timeout 10m;`：设置 SSL 会话的超时时间为 10 分钟。
7. `ssl_ciphers [...]`：指定加密套件的列表，这些加密套件用于 SSL/TLS 握手过程中。
8. `ssl_prefer_server_ciphers on;`：指示服务器优先使用自己的加密套件偏好，而不是客户端提供的。
9. `root /var/www/hotelweb;`：定义了服务器的根目录。这是 Nginx 用来查找文件的路径。
10. `location / {`：这个块指定了对于根路径（`/`）的请求，如何处理。
    - `try_files $uri $uri/ /index.html;`：尝试按顺序返回请求的文件、目录或 `/index.html`。这用于单页应用，确保路由可以由前端 JavaScript 框架处理。
11. `location /service {`：指定了对于 `/service` 路径的请求，如何处理。
    - `rewrite ^/service(.*) /$1 break;`：这行将请求中的 `/service` 重写为 `/`，并停止处理后续的 rewrite 规则。这允许将 API 请求代理到应用而不改变路径。
    - `proxy_pass http://localhost:84;`：将请求代理到本地 84 端口的应用（通常是一个后端服务，如 Spring Boot 应用）。
    - `proxy_http_version 1.1;`：使用 HTTP 1.1 协议与代理服务器通信。
    - `proxy_set_header Upgrade $http_upgrade;`：设置 HTTP 升级头部，用于 WebSockets 支持。
    - `proxy_set_header Connection 'upgrade';`：设置连接头部，同样是为了 WebSockets。
    - `proxy_set_header Host $host;`：设置请求的 Host 头部为原始请求的 Host，确保后端服务能正确识别请求的域名。
    - `proxy_cache_bypass $http_upgrade;`：如果有 HTTP 升级请求，绕过缓存处理。

#### 步骤 4: 检查 Nginx 配置并重启服务

​	在编辑配置文件后，您需要检查 Nginx 配置文件的语法是否正确：

```
sudo nginx -t
```

​	如果没有问题，重启 Nginx 服务以应用更改：

```shell
systemctl restart nginx
```

​	现在，当访问 `https://hotel.qinyunjian.cloud` 时，请求应该会被自动代理到运行在服务器 84 端口的 SpringBoot 服务。

