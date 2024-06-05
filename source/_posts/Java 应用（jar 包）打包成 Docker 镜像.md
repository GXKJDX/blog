---
title: Java 应用（jar 包）打包成 Docker 镜像
date: 2024-06-04 22:32:50
tags:
---
### Java 应用（jar 包）打包成 Docker 镜像

​	要将一个 Java 应用（jar 包）打包成 Docker 镜像，你需要编写一个 `Dockerfile`，这是一个文本文件，包含了构建 Docker 镜像所需的指令。以下是一个简单的示例流程，展示如何将一个 Spring Boot 应用的 jar 包转换成 Docker 镜像。

​	1、使用Xftp，把jar上传到指定的目录下。2、创建Dockerfile文件。

![](https://qinyunjian-1316017204.cos.ap-guangzhou.myqcloud.com/images/typora/image-20240226162321338.png)

#### 步骤 1: 准备 Dockerfile

1. **创建 Dockerfile**: 在你的 Java 项目根目录下创建一个名为 `Dockerfile` 的文件（无文件扩展名）。
2. **编辑 Dockerfile**: 使用文本编辑器打开 `Dockerfile`，并添加以下内容：

```dockerfile
# 使用官方的Java运行环境作为基础镜像
FROM openjdk:8-jdk-alpine
RUN apk add --no-cache fontconfig ttf-dejavu

# 设置时区（可选）
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 将jar包添加到容器中，注意修改路径以匹配你的jar包位置
ADD jinzijing-hotel-2.0.0.jar app.jar

# 暴露容器内部的端口号，与Spring Boot应用的端口一致
EXPOSE 84

# 在容器启动时运行jar包
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

#### 步骤 2: 构建 Docker 镜像

在包含 `Dockerfile` 的目录下，打开终端或命令提示符，运行以下命令来构建 Docker 镜像：

```dockerfile
docker build -t hotel-service .
```

- `-t hotel-service` 为新构建的镜像设置了一个名字 `hotel-service`。
- `.` 指定了 Dockerfile 所在的目录（当前目录）。

#### 步骤 3: 运行 Docker 容器

使用以下命令运行你的应用：

```dockerfile
docker run -d -p 84:84 myapplication
```

- `-d` 表示后台运行容器。
- `-p 84:84` 将容器的 84 端口映射到宿主机的 84 端口，允许外部访问。

确保你的应用配置为在 84 端口上监听，或者根据需要调整端口映射。

#### 步骤4：测试SpringBoot服务

使用以下命令测试你的应用是否能正常运行

```shell
curl http://localhost:84
```

如果出现`WELCOME`则说明服务正常运行。

