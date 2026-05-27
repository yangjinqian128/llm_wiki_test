---
type: knowledge-node
tags: [core-concept, auto-generated, container, tool]
last_compiled: 2026-05-22
---

# Docker

Docker 的三个最基本概念是 [[Docker Image]]、[[Docker Container]] 和 [[Docker Repository]][^1]。

## Docker Image

Image 可以看成是一个配置过的发行版（如带 apache 配置的 Ubuntu 发行版）[^1]。

### 基本操作

**下载镜像**[^1]：
```bash
docker pull ubuntu
```

**查看镜像**[^1]：
```bash
docker images
```

**修改 tag**[^1]：
```bash
docker tag 6b46dcca842d docker_test/with_net_tools:add_ping_change_tag
```

**删除镜像**[^1]：
```bash
docker rmi
```

注意：如果镜像被停止的容器使用，需要先删除相关容器[^1]。

## Docker Container

在容器中运行镜像[^1]：
```bash
docker run -t -i ubuntu:latest /bin/bash
```

### 容器状态

用 `docker ps -a` 查看容器[^1]：
- `Up XXX days`：容器正在运行
- `Exited (0) XXX days ago`：容器已退出

### 容器操作

**重新启动退出状态的容器**[^1]：
```bash
docker start -i 容器id
```

**接入运行状态的容器**[^1]：
```bash
docker attach 容器id
```

**删除容器**[^1]：
```bash
docker rm
```

## 构建镜像

容器退出后，需要把改动提交形成新的镜像[^1]：
```bash
docker commit -m "Add: basic tools" -a "Docker basic" 518aa47b44c4 ubuntu/latest
```

下次运行可以指定[^1]：
- Image ID
- repo:tag 方式

## 上传镜像

上传到 DockerHub[^1]：
```bash
docker login
docker push repo_name[:tag]
```

注意：repo_name 要用 `DockerHub_account/XXX` 的形式[^1]。

## 相关主题

- [[Docker Image]]
- [[Docker Container]]
- [[Docker Repository]]
- [[容器化]]

---

[^1]: 来源于原始素材 raw/notes/docker_note。