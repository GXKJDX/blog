---
title: Docker安装minio
date: 2024-06-12 17:33:50
tags:
---

MinIO 是一个高性能的分布式对象存储服务，它可以用于存储大量非结构化数据，如照片、视频、日志文件等。通过 Docker 安装和运行 MinIO 是一种快速且方便的方法。下面是在 Docker 上安装和运行 MinIO 的步骤：

## Docker安装minio

### 1. 拉取 MinIO Docker 镜像

首先，你需要从 Docker Hub 拉取最新的 MinIO 镜像。在你的终端中运行以下命令：

```shell
docker pull minio/minio
```

### 2.启动minio容器

```shell
docker run -d \
 --name minio \
 --cgroupns host \
 --env MINIO_ROOT_USER=admin \
 --env MINIO_ROOT_PASSWORD=******** \
 -p 9000:9000/tcp \
 -p 9090:9090/tcp \
 --restart=always \
 -v /mydata/minio/data:/data \
 minio/minio server /data --console-address :9090 --address :9000 
```

 Docker 运行 MinIO 服务，一个高性能的分布式对象存储服务，常用于存储大规模的非结构化数据。下面我将详细解释这个命令的各个部分：

1. `docker run -d`：
   - `docker run`：告诉 Docker 运行一个新的容器。
   - `-d`：代表后台运行，让容器在后台执行。
2. `--name minio`：
   - `--name`：设置容器的名称，这里名称被设为 `minio`。
3. `--cgroupns host`：
   - `--cgroupns`：指定容器使用的 cgroup 命名空间类型。这里使用的是 `host`，意味着容器将使用宿主机的 cgroup 命名空间，而不是创建新的。
4. `--env MINIO_ROOT_USER=admin` 和 `--env MINIO_ROOT_PASSWORD=********`：
   - `--env`：设置环境变量。
   - `MINIO_ROOT_USER` 和 `MINIO_ROOT_PASSWORD`：分别设置 MinIO 服务的根用户名称和密码。
5. `-p 9000:9000/tcp` 和 `-p 9090:9090/tcp`：
   - `-p`：端口映射，格式为 `宿主机端口:容器端口/tcp`。
   - `9000:9000/tcp`：将容器的 9000 端口映射到宿主机的 9000 端口，MinIO 的主服务通常在此端口运行。
   - `9090:9090/tcp`：将容器的 9090 端口映射到宿主机的 9090 端口，MinIO 的管理控制台通常在此端口运行。
6. `--restart=always`：
   - `--restart`：设置容器的重启策略。`always` 意味着无论容器的退出状态如何，只要 Docker 守护进程被重新启动，容器也将被重新启动。
7. `-v /mydata/minio/data:/data`：
   - `-v`：挂载卷，格式为 `宿主机路径:容器内路径`。
   - `/mydata/minio/data:/data`：将宿主机的 `/mydata/minio/data` 目录挂载到容器内的 `/data` 目录，用于数据持久化。
8. `minio/minio server /data`：
   - `minio/minio`：Docker 镜像名称，表示使用 MinIO 的官方 Docker 镜像。
   - `server /data`：MinIO 服务的启动命令，指定 `/data` 为数据存储位置。
9. `--console-address :9090 --address :9000`：
   - `--console-address :9090`：设置 MinIO 控制台的监听地址和端口。
   - `--address :9000`：设置 MinIO 服务的监听地址和端口。

整个命令的作用是在 Docker 中运行一个名为 `minio` 的容器，配置了环境变量、端口映射和卷挂载，并设定了服务及控制台的监听端口，用于提供一个持久化的分布式对象存储服务。

## Docker安装MinIO Client (`mc`)

### 1. 拉取 `mc` Docker 镜像

```shell
docker pull minio/mc
```

这个命令从 Docker Hub 上拉取最新的 MinIO Client (`mc`) 镜像。这样可以确保你使用的是最新版的客户端。

### 2. 通过 Docker 运行 `mc` 并进入其 Shell

```shell
docker run -it --entrypoint=/bin/sh minio/mc
```

### 3. 配置 MinIO 客户端

```shell
mc config host add <ALIAS> <YOUR-S3-ENDPOINT> <YOUR-ACCESS-KEY> <YOUR-SECRET-KEY> [--api API-SIGNATURE]
```

这个命令用于配置 `mc`，使其可以连接到一个特定的 MinIO 服务或兼容 S3 的存储服务。

- `<ALIAS>` 是你为这个存储服务定义的简称。
- `<YOUR-S3-ENDPOINT>` 是服务的访问 URL。
- `<YOUR-ACCESS-KEY>` 和 `<YOUR-SECRET-KEY>` 是你的访问密钥和密钥密码，用于身份验证。
- `[--api API-SIGNATURE]` 是可选的，用于指定 API 签名类型，通常是 `S3v4`。

### 4. 添加特定的 MinIO 服务配置

```shell
mc config host add minio http://47.113.216.154:9000 admin ********
```

这个命令添加一个名为 `minio` 的 MinIO 服务配置，使用的是 IP 地址 `117.72.14.166` 和端口 `9000`，以及提供的访问密钥和密钥密码。

### 5. 列出存储桶创建名`qyj`的储存桶

```shell
mc ls minio
mc mb minio/qyj
```

这个命令列出与别名 `minio` 相关联的存储服务中的所有存储桶。

### 6. 启用匿名访问模式

```shell
mc anonymous
```

这个命令启动 `mc` 的匿名访问模式，允许用户在没有提供 API 密钥的情况下执行操作。

### 7. 设置匿名下载权限

```shell
mc anonymous set download minio/qyj
```