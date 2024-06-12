---
title: Ubuntu安装docker
date: 2024-06-12 17:33:50
tags:
---

安装 Docker 时使用阿里云的镜像源可以加快下载速度。以下是在 Ubuntu 系统上安装 Docker 的步骤，使用阿里云提供的公共镜像源：

### 1. 更新软件包索引

打开你的终端，并执行以下命令以更新你的软件包索引：

```sh
sudo apt-get update
```

### 2. 安装 HTTPS 支持和证书

首先需要安装软件包，以确保可以通过 HTTPS 安全地下载软件包。可以使用以下命令来安装必要的依赖：

```sh
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

### 3. 添加 Docker 的官方 GPG 密钥

为了确保下载的 Docker 软件包的真实性，需要导入 Docker 的官方 GPG 密钥。使用阿里云的镜像服务器来替代 Docker 的官方密钥服务器，可以加快这一过程：

```sh
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

### 4. 设置 Docker 稳定版仓库

要使用阿里云的 Docker 仓库地址设置 APT 源，执行以下命令来添加阿里云的 Docker 稳定版仓库：

```sh
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

### 5. 安装 Docker Engine

现在，更新 apt 索引，并安装 Docker Engine 和 containerd：

```sh
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### 6. 验证 Docker 是否安装成功

安装完成后，启动 Docker 并运行 `hello-world` 镜像来验证是否正确安装：

```sh
sudo systemctl start docker
sudo docker run hello-world
```

如果看到返回消息，说明 Docker 已经正确安装并运行。

### 7. 配置 Docker Hub

为了进一步提高镜像拉取速度，可以配置 Docker 使用阿里云的镜像加速器：

1. 创建或编辑 `/etc/docker/daemon.json`：

   ```sh
   sudo nano /inc/docker/daemon.json
   ```

2. 添加以下内容（替换 `<你的阿里云加速器地址>` 为你实际的加速器地址）：

   ```json
   {
     "registry-mirrors": [
       "https://docker.m.daocloud.io",
       "https://docker.nju.edu.cn",
       "https://hub-mirror.c.163.com",
       "https://mirror.baidubce.com",
       "https://2e63y970.mirror.aliyuncs.com"
     ]
   }
   
   ```

3. 保存并关闭文件，然后重启 Docker 服务：

   ```sh
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```

默认情况下，只有 root 或者 有 sudo 权限的用户可以执行 Docker 命令。想要以非 root 用户执行 Docker 命令，你需要将你的用户添加到 Docker 用户组，该用户组在 Docker CE 软件包安装过程中被创建。想要这么做，输入：

```shell
sudo usermod -aG docker $USER
```

`$USER`是一个环境变量，代表当前用户名。