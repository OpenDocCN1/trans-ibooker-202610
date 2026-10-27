# Spring 微服务实战 第二版




应用程序A和B使用许可证服务来执行工作。

应用程序C使用库存服务。

应用程序A

应用程序B

应用程序C

许可证服务

许可证服务调用组织服务来执行一些工作。

许可证数据源

组织服务

云

库存服务

组织服务调用库存服务来执行一些工作。

组织数据源

NAS（写入共享文件系统）

有趣的部分开始了。对NAS的一处小改动导致库存服务出现性能问题。砰！一切都崩溃了。

我将此书献给所有
正在追求STEM职业的女性。

只要努力，一切皆有可能。

## 1 欢迎来到云，Spring 1

### 1.1 向微服务架构的演进 2
N层架构 2 • 什么是单体架构？3 • 什么是微服务？4 • 为什么改变我们构建应用程序的方式？6

### 1.2 使用Spring的微服务 7

### 1.3 我们正在构建什么？8

### 1.4 这本书是关于什么的？10
你将在这本书中学到什么 10 • 为什么这本书与你相关？10

### 1.5 云和基于微服务的应用程序 11
使用Spring Boot构建微服务 11 • 云计算到底是什么？16 • 为什么选择云和微服务？18

### 1.6 微服务不仅仅是编写代码 19

### 1.7 核心微服务开发模式 21

### 1.8 微服务路由模式 22

### 1.9 微服务客户端弹性 23

### 1.10 微服务安全模式 24

### 1.11 微服务日志和追踪模式 26

### 1.12 应用程序指标模式 27

### 1.13 微服务构建/部署模式 28

## 2 使用Spring Cloud探索微服务世界 31

### 2.1 什么是Spring Cloud？32

Spring Cloud Config 33 • Spring Cloud服务发现 33

Spring Cloud LoadBalancer和Resilience4j 33 • Spring Cloud API Gateway 34 • Spring Cloud Stream 34 • Spring Cloud Sleuth 34 • Spring Cloud Security 35

### 2.2 Spring Cloud示例 35

### 2.3 如何构建云原生微服务 37

代码库 40 • 依赖 40 • 配置 41 • 后端服务 42 • 构建、发布、运行 42 • 进程 43 • 端口绑定 43 • 并发 44 • 可丢弃性 44 • 开发/生产环境一致性 44 • 日志 45 • 管理进程 45

### 2.4 确保我们的示例具有相关性 46

### 2.5 使用Spring Boot和Java构建微服务 47

设置环境 47 • 开始使用骨架项目 47 • 启动你的Spring Boot应用程序：编写引导类 52

## 3 使用Spring Boot构建微服务 55

### 3.1 架构师的故事：设计微服务架构 56

分解业务问题 56 • 建立服务粒度 59 • 定义服务接口 61

### 3.2 何时不使用微服务 62

构建分布式系统的复杂性 62 • 服务器或容器蔓延 63 • 应用程序类型 63 • 数据事务和一致性 63

### 3.3 开发者的故事：使用Spring Boot和Java构建微服务 63

构建微服务的入口：Spring Boot控制器 64 • 为许可证服务添加国际化 74 • 实现Spring HATEOAS以显示相关链接 78

### 3.4 DevOps的故事：为严格的运行时而构建 81

服务组装：打包和部署你的微服务 83

服务引导：管理你的微服务配置 84

服务注册和发现：客户端如何与你的微服务通信 85 • 传达微服务的健康状况 86

### 3.5 整合所有视角 89

## 4 欢迎来到Docker 91

### 4.1 容器还是虚拟机？92

### 4.2 什么是Docker？94

### 4.3 Dockerfile 96

### 4.4 Docker Compose 97

### 4.5 将Docker与我们的微服务集成 99

构建Docker镜像 99 • 使用Spring Boot创建Docker镜像 104 • 使用Docker Compose启动服务 107

## 5 使用Spring Cloud配置服务器控制你的配置 110

### 5.1 关于管理配置（和复杂性）111

你的配置管理架构 112

实现选择 114

### 5.2 构建我们的Spring Cloud配置服务器 115

设置Spring Cloud Config引导类 120 • 将Spring Cloud Config Server与文件系统一起使用 121 • 为服务设置配置文件 122

### 5.3 将Spring Cloud Config与Spring Boot客户端集成 127

设置许可证服务的Spring Cloud Config服务依赖 128 • 配置许可证服务以使用Spring Cloud Config 129 • 使用Spring Cloud Config Server连接数据源 133 • 使用@ConfigurationProperties直接读取属性 137 • 使用Spring Cloud Config Server刷新你的属性 138 • 将Spring Cloud Configuration Server与Git一起使用 139 • 将Vault与Spring Cloud Config服务集成 141 • Vault UI 141

### 5.4 保护敏感配置信息 144

设置对称加密密钥 144 • 加密和解密属性 146

### 5.5 结语 147

## 6 关于服务发现 148

### 6.1 我的服务在哪里？149

### 6.2 云中的服务发现 152

服务发现的架构 153 • 使用Spring和Netflix Eureka实践服务发现 156

### 6.3 构建我们的Spring Eureka服务 158

### 6.4 使用Spring Eureka注册服务 162

Eureka的REST API 166 • Eureka仪表盘 167

### 6.5 使用服务发现查找服务 169

使用Spring Discovery Client查找服务实例 171

使用支持负载均衡器的Spring REST模板调用服务 173 • 使用Netflix Feign客户端调用服务 174

## 7 当坏事发生时：使用Spring Cloud和Resilience4j的弹性模式 177

### 7.1 什么是客户端弹性模式？178

客户端负载均衡 179 • 断路器 180

回退处理 180 • 隔板 180

### 7.2 为什么客户端弹性很重要 181

### 7.3 实现Resilience4j 185

### 7.4 设置许可证服务以使用Spring Cloud和Resilience4j 186

### 7.5 实现断路器 187

向组织服务添加断路器 191

自定义断路器 192

### 7.6 回退处理 193

### 7.7 实现隔板模式 195

### 7.8 实现重试模式 198

### 7.9 实现速率限制器模式 200

### 7.10 ThreadLocal与Resilience4j 202

## 8 使用Spring Cloud Gateway进行服务路由 208

### 8.1 什么是服务网关？209

### 8.2 介绍Spring Cloud Gateway 211

设置Spring Boot网关项目 212 • 配置Spring Cloud Gateway与Eureka通信 215




## 8.3 在 Spring Cloud Gateway 中配置路由 216
- 通过服务发现自动映射路由 216
- 使用服务发现手动映射路由 218
- 动态重新加载路由配置 221

## 8.4 Spring Cloud Gateway 的真正威力：Predicate 和 Filter 工厂 222
- 内置 Predicate 工厂 223
- 内置 Filter 工厂 224
- 自定义过滤器 225

## 8.5 构建前置过滤器 228

## 8.6 在服务中使用关联 ID 231
- UserContextFilter：拦截传入的 HTTP 请求 233
- UserContext：使 HTTP 头部易于服务访问 234
- 自定义 RestTemplate 和 UserContextInterceptor：确保关联 ID 得到传播 235

## 8.7 构建接收关联 ID 的后置过滤器 237

## 9 保护你的微服务 240

## 9.1 什么是 OAuth2？ 241

## 9.2 Keycloak 简介 242

## 9.3 从小开始：使用 Spring 和 Keycloak 保护单个端点 244
- 将 Keycloak 添加到 Docker 245
- 设置 Keycloak 246
- 注册客户端应用 248
- 配置 O-stock 用户 253
- 对我们的 O-stock 用户进行身份验证 255

## 9.4 使用 Keycloak 保护组织服务 258
- 向各个服务添加 Spring Security 和 Keycloak JAR 259
- 配置服务指向我们的 Keycloak 服务器 260
- 定义谁以及什么可以访问服务 260
- 传播访问令牌 265
- 解析 JWT 中的自定义字段 270

## 9.5 关于微服务安全的一些收尾思考 272
- 对所有服务通信使用 HTTPS 安全套接层（SSL） 273
- 使用服务网关访问你的微服务 273
- 将服务划分为公共 API 和私有 API 273
- 通过锁定不需要的网络端口来限制微服务的攻击面 274

## 10 使用 Spring Cloud Stream 的事件驱动架构 275
### 10.1 消息传递、EDA 和微服务的理由 276
使用同步请求-响应方法来通信状态变更 277 • 使用消息传递在服务之间通信状态变更 279 • 消息架构的缺点 281

### 10.2 介绍 Spring Cloud Stream 282

### 10.3 编写简单的消息生产者和消费者 284
在 Docker 中配置 Apache Kafka 和 Redis 285 • 在组织服务中编写消息生产者 286 • 在许可服务中编写消息消费者 293 • 查看消息服务运行情况 296

### 10.4 Spring Cloud Stream 用例：分布式缓存 297
使用 Redis 缓存查询 298 • 定义自定义通道 304

## 11 使用 Spring Cloud Sleuth 和 Zipkin 进行分布式追踪 307
### 11.1 Spring Cloud Sleuth 和关联 ID 309
向许可和组织添加 Spring Cloud Sleuth 309
Spring Cloud Sleuth 追踪的解剖 310

### 11.2 日志聚合和 Spring Cloud Sleuth 311
Spring Cloud Sleuth/ELK Stack 实现实战 313
在我们的服务中配置 Logback 314 • 在 Docker 中定义并运行 ELK Stack 应用 318 • 配置 Kibana 320 • 在 Kibana 中搜索 Spring Cloud Sleuth 追踪 ID 323 • 使用 Spring Cloud Gateway 将关联 ID 添加到 HTTP 响应 325

### 11.3 使用 Zipkin 进行分布式追踪 327
设置 Spring Cloud Sleuth 和 Zipkin 依赖 328
配置服务指向 Zipkin 328 • 配置 Zipkin 服务器 328 • 设置追踪级别 329 • 使用 Zipkin 追踪事务 330 • 可视化更复杂的事务 333
捕获消息追踪 334 • 添加自定义跨度 336

## 12 部署你的微服务 340
### 12.1 构建/部署管道的架构 342

### 12.2 在云中设置 O-stock 的核心基础设施 346
使用 Amazon RDS 创建 PostgreSQL 数据库 348
在 Amazon 中创建 Redis 集群 351

### 12.3 超越基础设施：部署 Q-stock 和 ELK 353
创建带有 ELK 的 EC2 353 • 在 EC2 实例中部署 ELK Stack 356 • 创建 EKS 集群 357

### 12.4 你的构建/部署管道实战 364

### 12.5 创建我们的构建/部署管道 366
设置 GitHub 366 • 使我们的服务能够在 Jenkins 中构建 367 • 理解并生成管道脚本 372 • 创建 Kubernetes 管道脚本 374

### 12.6 关于构建/部署管道的收尾思考 375

附录 A 微服务架构最佳实践 377

附录 B OAuth2 授权类型 383

附录 C 监控你的微服务 392

索引 403
