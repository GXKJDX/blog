---
title: Docker安装Redis
date: 2024-06-04 22:32:50
tags: docker
---

## docker安装Redis

### 安装 Redis 使用 Docker 的步骤

#### 步骤 1: 拉取 Redis 镜像

首先，您需要从 Docker Hub 获取 Redis 的官方镜像。在您的终端或命令行界面中，运行以下命令：

```
docker pull redis
```

这会下载 Redis 的最新官方镜像。如果您需要特定版本的 Redis，可以指定版本号，如 `redis:6.0`。

#### 步骤 2: 运行 Redis 容器（含数据持久化）

使用以下命令启动一个包含数据持久化的 Redis 容器实例：

```sh
docker run --name my-redis -p 6379:6379 -d -v redis-data:/data redis
```

这里的命令参数解释如下：

- `--name my-redis`：为容器指定一个名称，这里使用 `my-redis`。
- `-p 6379:6379`：将容器的 6379 端口（Redis 的默认端口）映射到宿主机的同一端口。
- `-d`：以“分离模式”运行容器，即容器在后台运行。
- `-v redis-data:/data`：创建并挂载一个名为 `redis-data` 的 Docker 卷到容器的 `/data` 目录。Redis 会将其数据存储在 `/data` 目录中，从而实现数据的持久化。
- `redis`：指定使用 Redis 的官方 Docker 镜像。

#### 步骤 3: 验证 Redis 容器

要确认 Redis 容器已经成功启动并运行，可以运行以下命令：

```sh
docker ps
```

这将列出所有正在运行的容器。您应该能在列表中看到名为 `my-redis` 的容器。