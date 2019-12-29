# Docker 概览

Docker 是当下最为流行的 Linux 容器，这篇日志记录个人在学习和使用 Docker 的过程中的一些笔记。

## Docker 引擎

Docker 引擎是一个 client-server 模式的应用，主要由以下三部分组成：

- 一个长期在后台运行的 daemon process 守护进程（即 `dockerd` 命令）
- 一套可以与上述守护进程交互的 REST API
- 一个基于上述 REST API 的 command line interface (CLI) 命令行接口（即 `docker` 命令）

![image](engine-components-flow.png)

Docker CLI 通过脚本或者 CLI 命令，使用 Docker REST API 控制 Docker daemon，并且与之交互。Docker daemon 创建和管理 Docker 对象，例如 images（镜像）、containers（容器）、networks（网络）、volumes（数据卷）。

## Docker 架构

Docker 使用 client-server 架构。Docker client 通知 Docker daemon 进行繁重的容器构建、运行、发布。Docker client 和 Docker daemon 可以部署在同一台机器上，也可以通过 Docker client 连接远程的 Docker daemon。Docker client 和 Docker daemon 通过 UNIX 套接字或者网络，使用 REST API 进行通讯。

![image](architecture.svg)

### Docker daemon

Docker daemon（即 `dockerd` 命令）监听来自 Docker API 的请求，并且负责管理各种 Docker 对象。一个 Docker daemon 还可以和其它 docker daemon 通讯，为了管理 Docker services。

### Docker client

Docker client（即 `docker` 命令）是许多 Docker 用户与 Docker 交互的主要方式。当使用诸如 `docker run` 之类的命令时，Docker client 会将这些命令发送给 Docker daemon，并交由 Docker daemon 执行。`docker` 命令会使用 Docker API，Docker client 可以同时与多个 Docker daemon 通讯。

### Docker registries

Docker registry 负责存储 Docker image。[Docker Hub](https://hub.docker.com/) 是一个任何人都可以使用的公开仓库，并且 Docker 默认配置为在 Docker Hub 上寻找 Docker image。

使用 `docker pull` 或 `docker run` 命令时，Docker 会从配置 Docker registry 中拉取所需要 Docker image。使用 `docker push` 时，Docker 会推送 Docker image 至 Docker registry。

### Docker objects

#### IMAGES

Docker image 是 read-only 只读的模板文件，其中包含创建 Docker container 的指令。通常来说，一个 Docker image 是基于另一个 Docker image ，并添加一些额外的自定义指令的。例如用户可以基于 `ubuntu` image 构建自己的 Docker image，随后安装 Apache web server 和应用程序，以及运行应用程序所需的配置信息。

用户可以使用自己构建的 Docker image，也可以使用别人构建并推送到 Docker registry 的 Docker image。如果需要自己构建，用户需要使用简单的语法创建一个 Dockerfile，并在其中定义创建 Docker image 的步骤，随后运行 `docker build` 命令。Dockerfile 中的每一个指令都会在 Docker image 中创建一个 Docker image layer。当用户修改 Dockerfile 并重新构建 Docker image 的时候，Docker 只会重新构建那些被修改的 Docker image layer。这是 Docker image 相比于其它虚拟化技术，为何如此轻量、小巧、快速的原因。

#### CONTAINERS

Docker container 是 Docker image 的可运行实例。用户可以使用 Docker API 或者 Docker CLI 创建、开始、停止、移动、删除 Docker container。用户可以连接 Docker container 至一个或多个网络，为 Docker container 绑定宿主存储目录至，甚至可以基于 Docker container 创建新的 Docker image。

默认情况下，一个 Docker container 和其它 Docker container、host machine 之间的隔离程度相对较高。用户可以控制 Docker container 的网络、存储或其它底层子系统和其它 Docker container、host machine 之间的隔离程度。

一个 Docker container 由 Docker image 和创建或启动 Docker container 时提供的配置项定义。当 Docker container 移除之后，任何未持久化的状态更改都将会消失。

#### SERVICES

// TODO
