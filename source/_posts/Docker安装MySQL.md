---
title: Docker安装MySQL
date: 2024-06-04 22:32:50
tags:
---

## Docker安装MySQL

### 安装 MySQL 5.7 到 Docker 的详细步骤

#### 步骤 1: 拉取 MySQL 5.7 镜像

**操作**：

```shell
docker pull mysql:5.7
```

**解释**：

- 这个命令会从 Docker Hub（Docker 的官方镜像仓库）拉取 MySQL 5.7 的镜像。
- `docker pull` 是 Docker 获取镜像的命令。
- `mysql:5.7` 指定了您想要拉取的镜像及其版本。这里是 MySQL 版本 5.7。

#### 注：可以先使用`docker volume create --name mysql-data`

#### 步骤 2: 创建并运行 MySQL 容器

**操作**：

```shell
docker run --name mysql-5.7 -e MYSQL_ROOT_PASSWORD=123456 -v mysql-data:/var/lib/mysql -p 3306:3306 -d mysql:5.7
```

**解释**：

- **docker run**：
  - 这是 Docker 用于启动新容器的基本命令。
- **--name mysql-5.7**：
  - `--name` 选项用于为容器指定一个名字，这里命名为 `mysql-5.7`。这个名字用于识别和引用容器。
- **-e MYSQL_ROOT_PASSWORD=123456**：
  - `-e` 选项用于设置环境变量。在这个例子中，它设置了 MySQL 的 root 用户的密码为 `123456`。这是 MySQL 容器启动所必需的。
- **-v mysql-data:/var/lib/mysql**：
  - `-v` 选项用于挂载卷（volume）。这里，它将名为 `mysql-data` 的 Docker 卷挂载到容器内的 `/var/lib/mysql` 目录。
  - 这个操作的目的是数据持久化。即使容器被停止或删除，存储在 `/var/lib/mysql` 目录中的数据（即 MySQL 数据库文件）仍然会保留在 `mysql-data` 卷中。
- **-p 3306:3306**：
  - `-p` 选项用于端口映射。这里，它将容器内的 3306 端口（MySQL 默认端口）映射到宿主机的同一端口。
  - 这样可以通过宿主机的 3306 端口访问 MySQL 容器。
- **-d**：
  - `-d` 选项告诉 Docker 以“分离模式”运行容器，即在后台运行。
- **mysql:5.7**：
  - 指定使用的 Docker 镜像和标签。在这个例子中，它使用的是 MySQL 5.7 版本的官方 Docker 镜像

#### 步骤 3: 检查容器运行状态

**操作**：

```shell
docker ps
```

**解释**：

- 使用 `docker ps` 可以查看当前正在运行的 Docker 容器的列表。
- 如果 MySQL 容器已经启动并运行，它应该会出现在这个列表中。

#### 步骤4：重新安装MySQL

停止原先`mysql-5.7`容器

```shell
docker stop mysql-5.7
```

删除原先`mysql-5.7`容器

```shell
docker rm mysql-5.7
```

因为我删除了原先的数据库，但是保留了宿主机上的配置，我希望把保留的配置和数据卷重新映射到容器，以下是我重新安装mysql-5.7的命令。

```shell
docker run -d \
  --name mysql-5.7 \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -p 3306:3306 \
  -v mysql-data:/var/lib/mysql \
  -v /etc/my.cnf.d/mysql-clients.cnf:/etc/mysql/conf.d/mysql-clients.cnf \
  mysql:5.7
```

这条命令通过Docker在后台启动了一个MySQL 5.7版本的容器，并配置了一系列的参数来满足特定的需求：

* **`-d`**: 表示容器在后台运行，让终端不会被当前容器的运行过程占用。

* **`--name mysql-5.7`**: 为运行的容器指定一个名字`mysql-5.7`，这样可以更方便地对容器进行管理。

* **`-e MYSQL_ROOT_PASSWORD=my-secret-pw`**: 设置环境变量`MYSQL_ROOT_PASSWORD`为`my-secret-pw`，这是为MySQL的root用户设置的密码，是MySQL容器启动时必需的配置项。

* **`-p 3306:3306`**: 将容器内部使用的3306端口映射到宿主机的3306端口上，这样可以通过宿主机的端口来访问容器中的MySQL服务。

* **`-v mysql-data:/var/lib/mysql`**: 将名为`mysql-data`的数据卷挂载到容器内的`/var/lib/mysql`目录。这是MySQL存储数据的默认位置，使用数据卷可以实现数据的持久化保存，确保数据不会因为容器的删除而丢失，并且可以在不同的容器间共享这部分数据。

* **`-v /etc/my.cnf.d/mysql-clients.cnf:/etc/mysql/conf.d/mysql-clients.cnf`**: 将宿主机上的`/etc/my.cnf.d/mysql-clients.cnf`配置文件挂载到容器内的`/etc/mysql/conf.d/mysql-clients.cnf`位置。这样做是为了自定义MySQL服务器的配置，可以根据需要调整MySQL的行为和性能。

* **`mysql:5.7`**: 指定使用`mysql:5.7`镜像创建容器，这个镜像是MySQL官方发布的，版本号为5.7，保证了MySQL服务的标准化和一致性。

![](https://qinyunjian-1316017204.cos.ap-guangzhou.myqcloud.com/images/typora/about.png)

