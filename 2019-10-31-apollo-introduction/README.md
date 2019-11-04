# Apollo

Apollo 是携程框架部门研发的分布式配置中心，能够集中化管理不同环境、不同集群的应用配置，配置修改之后能够实时推送到应用端，并且具备规范的权限，流程治理等特性，适用于微服务配置管理场景。

下面记录一些关于 Apollo 的核心概念和架构设计，详情可参考 Apolo 的
官方文档地址：[https://github.com/ctripcorp/apollo/wiki](https://github.com/ctripcorp/apollo/wiki)

---

Apollo 支持以下功能特性：

- 支持从 application、environment、cluster、namespace 维度来管理 Key-Value 配置
- 支持修改配置后实时生效，支持修改配置的灰度发布
- 支持配置修改和发布的版本管理以及回滚功能
- 管理后台支持权限管理、发布审核、操作审核
- ......

---

Apollo 中的核心概念：

- **application** 使用配置的应用，Apollo 客户端在运行时需要知道当前应用是谁，从而可以去获取对应的配置。在 Apollo 中，每个应用都会使用 AppId 来标识其唯一身份
- **environment** 应用的运行环境，例如测试环境、生产环境等等
- **cluster** 应用的部署集群，例如典型的按照数据中心划分为杭州集群、上海集群等等
- **namespace** 应用所使用的不同配置的命名空间，例如 `jdbc.username` 和 `jdbc.password` 为一个命名空间，`threadpool.corePoolSize` 和 `threadpool.maxPoolSize` 为另一个命名空间

---

Apollo 的基本工作模式如下：

1. 用户在配置中心对配置进行修改并发布
2. 配置中心通知 Apollo 客户端更新配置
3. Apollo 客户端从配置中心获取最新的配置、更新本地配置并通知应用

![image](1.png)

---

Apollo 的主要模块如下：

![image.png](2.png)

- **Config Service** 提供配置的读取、推送等功能，服务对象是 Apollo 客户端
- **Admin Service** 提供配置的修改、发布等功能，服务对象是 Apollo Portal（管理界面）
- **Meta Server** 用于封装 Eureka 的服务发现接口
- **Client** 通过域名访问 Meta Server 获取 Config Service 服务列表（IP+Port），而后直接通过 IP+Port 访问服务，同时在 Client 侧会做 load balance、错误重试
- **Portal** 通过域名访问 Meta Server 获取 Admin Service 服务列表（IP+Port），而后直接通过 IP+Port 访问服务，同时在 Portal 侧会做 load balance、错误重试

这其中需要注意：

1. Config Service 和 Admin Service 都是多实例、无状态部署，所以需要将自己注册到 Eureka 中并保持心跳
2. 为了简化部署，实际上会 Config Service、Eureka 和 Meta Server 三个逻辑角色都会部署在同一个 JVM 进程中
