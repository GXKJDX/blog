---
title: 为qinyunjian.cloud安装SSL证书
date: 2024-06-04 22:32:50
tags: nginx
---
### 安装SSL证书。

​	前置条件，我现在有一个域名（qinyunjian.cloud），默认使用的是http协议，现在我需要为这个域名安装证书，

#### 步骤 1: 安装 Certbot

Certbot 是 Let's Encrypt 的官方客户端，用于自动化安装和更新 SSL 证书。

1. **启用 EPEL 仓库**：(在安装docker的时候已经安装EPEL 仓库，不必重复安装)

   ```shell
   sudo yum install epel-release
   ```

2. **安装 Certbot**：

   对于Nginx：

   ```shell
   sudo yum -y install certbot-nginx
   ```

#### 步骤 2: 获取和安装证书

使用 Certbot 为您的域名获取和安装证书：

- Nginx

  ```shell
  sudo certbot --nginx -d qinyunjian.cloud -d www.qinyunjian.cloud
  ```

- 如果 Certbot 成功安装证书，它将自动修改您的 Nginx 配置文件以使用 SSL，如果说运行了这一步，但是`nginx.conf`文件没有能被正确的更改，则需要手动配置`nginx.conf`。

#### 步骤 3: 自动续订证书

Let's Encrypt 证书有效期为 90 天，可以通过 Certbot 自动续订：

- 测试自动续订

  ```shell
  sudo certbot renew --dry-run
  ```

- 如果测试成功，Certbot 将自动设置定时任务来续订证书。

#### 步骤 4: 确认 HTTPS 生效

在完成上述步骤后，您可以通过访问 `https://qinyunjian.cloud` 来确认 SSL 证书是否成功安装。浏览器应该显示一个锁图标，表示连接是安全的。

#### 手动配置`nginx.conf`

​	如果 Certbot 运行失败，没有自动修改 Nginx 的配置文件来启用 HTTPS 和配置 HTTP 到 HTTPS 的重定向，您可以手动编辑 Nginx 的配置文件来实现这些功能。以下是一个基本的示例，展示如何为您的域名 `qinyunjian.cloud` 配置 SSL 证书和重定向。

#### 步骤 1: 打开您的 Nginx 配置文件

对于大多数 Nginx 安装来说，配置文件通常位于 `/etc/nginx/nginx.conf` 或者 `/etc/nginx/sites-available/` 目录下的某个文件。如果您使用的是后者，那么您的配置可能位于一个特定的域名文件中，例如 `/etc/nginx/sites-available/qinyunjian.cloud.conf`。

#### 步骤 2: 配置 SSL

以下是一个配置 SSL 的示例。请确保您已经通过 Certbot 获取了证书，并知道证书和私钥文件的路径。

```shell
server {
    listen 80;
    server_name qinyunjian.cloud www.qinyunjian.cloud;

    # 重定向所有 HTTP 请求到 HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name qinyunjian.cloud www.qinyunjian.cloud;

    # 指定 SSL 证书和密钥的路径
    ssl_certificate /etc/letsencrypt/live/qinyunjian.cloud/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/qinyunjian.cloud/privkey.pem;

    # 其他 SSL 设置...
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 5m;
    ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256';
    ssl_prefer_server_ciphers on;

    # 配置网站根目录
    root /var/www/qinyunjian.cloud/html;

    # 其他配置...
}
```

#### 步骤 3: 检查 Nginx 配置并重启服务

在编辑配置文件之后，您应该检查配置是否正确：

```shell
sudo nginx -t
```

如果显示配置文件语法正确，那么您可以重启 Nginx 以应用更改：

```shell
sudo systemctl restart nginx
```