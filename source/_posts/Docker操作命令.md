---
title: 二、Docker操作命令练习
date: 2024-06-02 22:32:50
tags: docker
---
### Docker操作命令

#### 一、简单的`hello-world`示例

#### 步骤 1: 拉取 `hello-world` 镜像

运行以下命令以拉取最新的 `hello-world` 镜像：

```shell
docker pull hello-world
```

#### 步骤 2: 运行 `hello-world` 容器并命名

运行以下命令启动一个 `hello-world` 容器，并给它命名为 `my-hello-world`：

```shell
docker run --name my-hello-world hello-world
```

这个命令会启动一个 `hello-world` 容器，容器运行后会显示一条欢迎消息然后退出。因为 `hello-world` 容器是为了演示目的而设计，它会立即退出并不会长时间运行。

#### 二、使用 `my-redis` 容器作为示例

因为我已经拉取了`redis`的镜像，并且已经成功的运行了`redis`容器，命名为`my-redis`，以下使用 `my-redis` 容器作为示例，提供更多的 Docker 命令。

#### 启动已停止的容器

```shell
docker start my-redis
```

#### 停止容器

```shell
docker stop my-redis
```

#### 重启容器

```shell
docker restart my-redis
```

#### 查看容器日志

```shell
docker logs my-redis
```

#### 查看容器的实时日志

```shell
docker logs -f my-redis
```

#### 检查容器的详细信息

```shell
docker inspect my-redis
```

#### 查看容器内部的进程

```shell
docker top my-redis
```

#### 查看容器的资源使用情况（如 CPU、内存）

```shell
docker stats my-redis
```

#### 进入容器内部的命令

假设你想在 `my-redis` 容器内部：

```shell
docker exec -it my-redis /bin/bash (/bin/sh)
```

执行 `Redis` 命令行界面：终端输入`redis-cli`，进入命令行界面。

#### 复制文件到/从容器

假设你想从你的主机复制一个名为 `dump.rdb` 的文件到 `my-redis` 容器的 `/data` 目录中：

```shell
docker cp dump.rdb my-redis:/data/dump.rdb
```

反向操作，从容器复制文件到主机：

```shell
docker cp my-redis:/data/dump.rdb ./dump.rdb
```

#### 三、使用`hotel-service`作为例子。

#### 查看特定数量的尾部日志行并持续更新

可以结合使用 `-n`（或 `--tail`）选项和 `-f` 选项。例如，查看最后50行日志并持续更新：

```shell
docker logs --tail 50 -f hotel-service
```

#### 进入`hotel-service`容器

```shell
docker exec -it my-redis /bin/sh
```

查看日志，我的日志目录和系统的目录是同级的，（根据实际情况来定，具体还是得看自己的日志目录配在什么地方）。

```shell
cd app-log/
```

查看错误日志，

要在 Linux 中查看文件末尾的 100 行，您可以使用 `tail` 命令配合 `-n` 选项，如下所示：

```shell
tail -n 100 syslog
```

或者直接输入文件的路径。例如，如果您想查看名为 `/var/log/syslog` 的文件的末尾 100 行，命令将是：

```shell
tail -n 100 /var/log/syslog
```

