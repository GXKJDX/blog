---
title: Nginx和Certbot安装和minio的域名映射
date: 2024-06-12 17:33:50
tags: ubuntu
---

### Nginx和Certbot安装和minio的域名映射

### 步骤1: 更新软件包列表

首先，打开终端并更新Ubuntu的软件包列表以确保安装最新版本的软件。运行以下命令：

```shell
sudo apt update
```

### 步骤2: 安装Nginx

使用`apt`命令安装Nginx：

```shell
sudo apt install nginx
```

### 步骤3: 安装 Certbot

对于大多数 Linux 发行版，如 Ubuntu，你可以使用以下命令安装 Certbot：

```shell
sudo apt-get install certbot
sudo apt-get install python3-certbot-nginx
```

### 步骤4: 使用 Certbot 获取和安装证书

对于 Nginx：

```shell
sudo certbot --nginx -d minio.qinyunjian.cloud
```

### 步骤5：编辑配置，路径：/etc/nginx/conf.d

```shell
server {
    listen 443 ssl;
    server_name minio.qinyunjian.cloud;

    ssl_certificate /etc/letsencrypt/live/minio.qinyunjian.cloud/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/minio.qinyunjian.cloud/privkey.pem;

    location / {
        proxy_pass http://localhost:9090;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### 步骤6：

1. **保存配置文件**：保存Nginx 配置文件。

   ```shell
   sudo nano /etc/nginx/conf.d/minio.qinyunjian.cloud.conf
   ```

2. **检查配置并重启 Nginx**：在保存修改后，再次运行  来验证配置文件是否正确。`nginx -t`

   ```shell
   sudo nginx -t
   ```

   如果输出显示“test is successful”，则表示配置文件没有语法错误。 然后，你可以安全地重启 Nginx：

   ```shell
   sudo systemctl restart nginx
   ```