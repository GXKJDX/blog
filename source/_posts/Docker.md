---
title: Docker基础知识
date: 2024-01-20 22:32:50
tags:
---

## 一、Docker学习记录

对于刚开始学习 Docker 的初学者来说，以下是一系列推荐的步骤和概念，可以帮助您建立对 Docker 的基本理解并开始实践：

### 1. 理解 Docker 的基本概念

- **容器（Containers）**：轻量级、可执行的独立软件包，包含运行应用所需的一切：代码、运行时、库、环境变量和配置文件。
- **镜像（Images）**：容器的蓝图，包含创建容器所需的指令。
- **Dockerfile**：一种脚本，包含了一系列指令和步骤，用于创建 Docker 镜像。

### 2. 运行您的第一个容器

- 通过运行一个简单的容器来开始，例如 hello-world镜像：

  ```shell
  docker run hello-world
  ```

  这个命令会下载一个测试镜像并在容器中运行它。

### 3. 学习 Docker 基本命令

- 熟悉常用的 Docker 命令：
  - `docker run`：运行一个容器。
  - `docker ps`：列出运行中的容器。
  - `docker images`：列出本地存储的镜像。
  - `docker pull`：从 Docker Hub 下载一个镜像。
  - `docker build`：根据 Dockerfile 构建一个新镜像。
  - `docker rm`：删除一个或多个容器。
  - `docker rmi`：删除一个或多个镜像。

### 4. 实践构建自己的 Docker 镜像

- 学习编写 Dockerfile 并构建自己的镜像：
  - 创建一个简单的 Dockerfile，例如，设置基础镜像，复制文件，设置工作目录和启动命令。
  - 使用 `docker build` 命令构建镜像。

### 5. 学习容器的网络和存储

- 了解如何使用 Docker 网络来连接容器。
- 学习如何使用卷（Volumes）持久化容器数据。

### 6. 使用 Docker Compose

- Docker Compose 允许您使用 YAML 文件定义多容器应用。
- 学习编写 `docker-compose.yml` 文件并使用 `docker-compose up` 和 `docker-compose down` 来管理应用。

## 二、安装docker

在 CentOS 7.8 上使用阿里云的 Docker 仓库来安装 Docker，您可以按照以下步骤进行：

### 1：安装必要的依赖包

首先，安装一些必要的软件包，这些软件包允许您通过 HTTPS 使用仓库：

```sh
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 2：设置阿里云 Docker 仓库

要使用阿里云的 Docker 仓库，您需要添加阿里云的 Docker 仓库地址。通常，您可以在阿里云的容器服务页面获取专属于您账户的 Docker 仓库地址。以下是一个示例命令，但请使用您的实际阿里云 Docker 仓库地址：

```sh
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 3：安装 Docker Engine

现在，从阿里云仓库安装 Docker：

```sh
sudo yum install docker-ce docker-ce-cli containerd.io
```

### 4：启动 Docker

安装完成后，启动 Docker 服务：

```sh
sudo systemctl start docker
```

### 5：验证安装

运行 hello-world 镜像来验证 Docker 是否正确安装，通常在这里是不会有信息响应，因为还需要拉取镜像：

```sh
sudo docker run hello-world
```

如果能看到欢迎信息，说明 Docker 已经正确安装和运行。

### 6：使 Docker 开机自启

为了确保 Docker 在启动时自动启动，请使用：

```sh
sudo systemctl enable docker
```

### 7：（可选）添加非 root 用户到 Docker 组

为了避免每次运行 Docker 命令时都使用 `sudo`，您可以将您的用户添加到 `docker` 组：

```sh
sudo usermod -aG docker $USER
```

这里的 `-aG` 选项意味着将用户添加到 `docker` 组并保留其在其他组的成员资格。注销并重新登录后，您可以以非 root 用户身份运行 Docker 命令。

