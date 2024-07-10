---
title: Nginx基础知识
date: 2024-01-20 22:31:50
tags: nginx
---

## 一、Nginx安装

在 CentOS 7.8 上安装 Nginx 通常分为几个步骤：

### 1. 更新系统

在开始之前，最好确保所有的系统软件都是最新的。打开终端并输入以下命令来更新您的系统：

```sh
sudo yum update
```

这个命令会更新系统中所有已安装的软件包到最新版本。

### 2. 添加 EPEL 仓库

由于 Nginx 不包含在 CentOS 的默认 YUM 仓库中，您需要添加 EPEL（Extra Packages for Enterprise Linux）仓库来获取 Nginx 的软件包。运行以下命令添加 EPEL 仓库：

```sh
sudo yum install epel-release
```

### 3. 安装 Nginx

现在 EPEL 仓库已经添加，您可以安装 Nginx 了。使用以下命令安装：

```sh
sudo yum install nginx
```

### 4. 启动 Nginx 服务

安装完成后，您需要启动 Nginx 服务。使用以下命令：

```sh
sudo systemctl start nginx
```

### 5. 自动启动 Nginx

为了确保 Nginx 在每次系统启动时自动运行，使用以下命令：

```sh
sudo systemctl enable nginx
```

### 6. 调整防火墙设置

如果您的服务器运行的是 firewalld（CentOS 7 的默认防火墙），您需要允许 HTTP 和 HTTPS 流量。运行以下命令来更新防火墙设置：

```sh
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```

### 7. 验证 Nginx 安装

在浏览器中输入您服务器的 IP 地址。如果 Nginx 安装成功，您应该会看到 Nginx 的默认欢迎页面。

### 8. 配置 Nginx（可选）

Nginx 的主配置文件位于 `/etc/nginx/nginx.conf`。您可能需要根据您的需求来编辑此文件。网站的配置文件通常位于 `/etc/nginx/conf.d/` 目录中。



## 二、Nginx的配置文件

已经成功的安装了Nginx，以下是对配置文件的解释说明。
