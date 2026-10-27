## 索引

### A

- 抽象 112
- Accept-Language 标头 77
- 访问令牌
  - 传播 265–270
  - 刷新 390
- access_token 属性 257
- action 元素 290
- ActionEnum 289
- ActiveCycle 设置 200
- ActivePermissions 设置 200
- actuator/gateway/refresh 端点 221
- actuator/gateway/routes 端点 217, 221
- add() 方法 80
- 管理进程 45–46
- Amazon
  - 使用 ELK 创建 EC2 实例 353–356
  - 使用 Amazon RDS 创建 PostgreSQL 数据库 348–350
  - 在 Amazon 中创建 Redis 集群 351–352
  - 在 EC2 实例中部署 ELK 栈 356–357
- Amazon Web Services (AWS) 347
- Amazon 的 EKS 容器服务 366
- AMI（Amazon 机器镜像）353
- AOP（面向切面编程）186
- 在 Docker 中配置 Apache Kafka 285
- API 网关服务器 213
- API 网关 382
- ApiGatewayServerApplication 类 215
- APM（应用性能管理）327
- Application 类 11
- 应用指标模式 27–28
- ApplicationContext 119
- 架构师的角色 56–62
  - 分解业务问题 56–59
  - 定义服务接口 61–62
  - 确定服务粒度 59–61
- 面向切面编程（AOP）186
- AtomicRateLimiter 200
- AUTH_TOKEN 变量 271
- 认证 25
- 认证服务器 243
- 授权 25
- 授权类型 386–388
- Authorization 标头 266, 268
- 授权服务器 243
- @Autowired 注解 137, 172, 236
- AWS（Amazon Web Services）347

### B

- 后端服务 42
- baseUrl 属性 328
- @Bean 标签 53
- @Bean 注解 76
- 最佳实践
  - 微服务架构
    - API 网关 382
    - CI/CD 380–381
    - 外部化配置 380
    - 日志记录 382
    - 监控 381
    - Richardson 成熟度模型 378–380
    - Spring HATEOAS 380
  - 十二要素应用 37–46
    - 管理进程 45–46
    - 后端服务 42
    - 构建、发布、运行 42–43
    - 代码库 40
    - 并发 44
    - 配置 41
    - 依赖 40
    - 开发/生产一致性 44–45
    - 可丢弃性 44
    - 日志 45
    - 端口绑定 43
    - 进程 43
- 绑定器 284
- bootBuildImage 任务 105
- 引导类
  - Spring Boot 52–53
  - Spring Cloud Configuration Server 120
- 桥接类型 95
- 构建和部署模式 28
- 构建命令 107
- 构建、发布和运行阶段 42–43
- 构建/部署流水线 376
  - 架构 342–346
  - 创建 366–374
    - 创建 Kubernetes 流水线脚本 374
    - 在 Jenkins 中启用服务构建 367–372
    - 设置 GitHub 366–367
    - 理解并生成流水线脚本 372–374
  - 实战 364–366
- buildFallbackLicenseList() 方法 194
- Buildpacks 105
- @Bulkhead 注解 185, 198, 205
- 舱壁模式 24
  - 实现 195–198
  - 概述 180–181
- 业务问题，分解 56–59

### C

- CaaS（容器即服务）16
- cacheOrganizationObject() 方法 303
- CallNotPermittedException 188
- CD（持续交付）343, 380
- checkRedisCache() 方法 303
- CI（持续集成）342, 380
- CI/CD（持续集成和持续交付）344, 380–381
- 断路器模式 187–193
  - 添加到组织服务 191–192
  - 自定义 192–193
  - 概述 180
- @CircuitBreaker 注解 185, 189–191, 194, 196, 205
- classpath 属性 122
- 客户端凭据授权类型 385
- 客户端弹性模式 23–207
  - 舱壁模式 180–181, 195–198
  - 断路器模式 180, 187–193
  - 回退处理模式 180, 193–194
  - 重要性 181–184
  - 负载均衡 179–180
  - 限流器模式 200–202
  - Resilience4j
    - 实现 185
    - 设置许可服务以使用 186–187
    - ThreadLocal 与 202–207
  - 重试模式 198–200
- 客户端负载均衡 24, 154
- clientType 参数 169
- 云计算 11–19
  - 使用 Spring Boot 构建微服务 11–14
  - 定义 16–17
  - 将微服务部署到云端的理由 18–19
- 云原生微服务应用最佳实践 37–46
  - 管理进程 45–46
  - 后端服务 42
  - 构建、发布、运行 42–43
  - 代码库 40
  - 并发 44
  - 配置 41
  - 依赖 40
  - 开发/生产一致性 44–45
  - 可丢弃性 44
  - 日志 45
  - 端口绑定 43
  - 进程 43
- 云原生应用 37
- 云就绪应用 37
- ClusterIP 服务 362
- 代码库 40
- @Column 属性 134
- @Component 注解 53
- 并发 44
- config.getProperty() 类 137
- 配置漂移 28
- @Configuration 标签 53
- @ConfigurationProperties 137–138
- configure() 方法 262
- 一致性 63
- Consul 33
- 容器即服务（CaaS）16
- 容器
  - 容器蔓延 63
  - 与虚拟机对比 92–94
- 持续交付（CD）343, 380
- 持续集成和持续交付（CI/CD）344, 380–381
- @Controller 注解 65
- Controller 类 66
- Conway 定律 7
- 核心开发模式 21–22
- coreThreadPoolSize 197
- 关联 ID
  - 使用 Spring Cloud Gateway 将其添加到 HTTP 响应 325–326
  - 构建接收其的后置过滤器 237–238
  - 确保其传播 235–236
  - Spring Cloud Sleuth 与 309–311
    - 添加到许可和组织服务 309–310
    - 追踪结构解析 310–311
  - 在服务中使用 231–236
    - 自定义 RestTemplate 和 UserContextInterceptor 235–236
  - UserContext 234–235
  - UserContextFilter 233–234
- correlationId 元素 290
- createLicense() 方法 71, 77
- 凭据管理和传播 25
- 横切关注点 222
- CRUD（创建、读取、更新、删除）61, 134, 301
- CrudRepository 类 135
- CustomChannels 接口 304–305

### D

- 数据事务 63
- 分解 4
- /decrypt 端点 146
- @DeleteMapping 注解 71
- 依赖 40
  - 设置许可服务依赖 128–129
  - 设置 Spring Cloud Sleuth 和 Zipkin 依赖 328
  - 十二要素应用最佳实践 40
- 依赖注入框架 7
- dependency:tree 目标 51
- 部署微服务 376
  - 构建/部署流水线 376
    - 架构 342–346
    - 创建 366–374
    - 实战 364–366
  - 部署 O-stock 353–364
    - 使用 ELK 创建 EC2 实例 353–356
    - 创建 EKS 集群 357–364
    - 在 EC2 实例中部署 ELK 栈 356–357
    - 在云端设置 O-stock 核心基础设施 346–352
      - 使用 Amazon RDS 创建 PostgreSQL 数据库 348–350
      - 在 Amazon 中创建 Redis 集群 351–352
- 开发/生产一致性 44–45
- DevOps（开发运维）38
- DevOps 工程师的角色 81–89
  - 传达微服务健康状态 86–89
  - 服务组装 83–84
  - 服务引导 84–85
  - 服务注册与发现 85–86
- DiscoveryClient 类 172
- 可丢弃性 44
- 分布式缓存 297–306
- 分布式追踪 339, 382
  - Spring Cloud Sleuth
    - 关联 ID 与 309–311
    - 日志聚合与 311–326
  - Zipkin 327–338
    - 添加自定义跨度 336–338
    - 捕获消息传递追踪 334–335
    - 配置服务器 328–329
    - 配置服务指向 328
    - 设置追踪级别 329–330
    - 设置依赖 328
    - 追踪事务 330–333
    - 可视化更复杂的事务 333
- Docker 109
  - 添加 Keycloak 245
  - 在 Docker 中配置 Apache Kafka 和 Redis 285
  - 容器与虚拟机 92–94
  - 定义 94–96
  - 在 Docker 中定义并运行 ELK 栈应用 318–320
- Docker Compose 97–98
- Dockerfiles 96
- 与微服务集成 99–108
  - 创建 Docker 镜像 99–104
    - 使用 Spring Boot 创建 Docker 镜像 104–107
  - 使用 Docker Compose 启动服务 107–108
- docker build 命令 96
- Docker 客户端 95
- docker commit 命令 108
- Docker Compose 97–98, 107–108
- Docker 容器 95
- Docker 守护进程 95
- Docker 镜像
  - 创建 99–104
  - 基础 Dockerfiles 100
  - 多阶段构建 Dockerfiles 101–104





- Docker Images（续）
  - 使用 Spring Boot 创建 104–107
  - Buildpacks 105
  - 分层 JAR 106–107
- Docker images 95
- docker images 命令 104, 359
- Docker networks 95
- docker ps 命令 104, 108
- Docker registry 95
- docker run 命令 95, 104, 141
- docker tag 命令 360
- docker version 命令 103
- Docker volumes 95
- docker-compose 命令 98, 124, 162, 320, 367
- docker-compose config 命令 97
- docker-compose up 命令 97, 108, 124
- docker-machine 命令 367
- dockerfile:build 命令 103
- Dockerfiles 96
  - 基础 100
  - 多阶段构建 101–104
- doFilter() 方法 205

## E
- EC2（Elastic Compute Cloud）
  - 使用 ELK 创建实例 353–356
  - 在实例中部署 ELK 堆栈 356–357
- ECR（Elastic Container Registry）358, 365
- ECS（Elastic Container Service）16
- EDA（事件驱动架构）276–306
  - 消息传递 279–281
    - 缺点 281–282
      - 持久性 280
      - 灵活性 281
      - 松耦合 280
      - 可扩展性 280–281
    - Spring Cloud Stream 282–297
      - 在 Docker 中配置 Apache Kafka 和 Redis 285
      - 定义自定义通道 304–306
      - 分布式缓存 297–306
      - 使用 Redis 缓存查找 298–304
      - 在许可服务中编写消息消费者 293–295
      - 在组织服务中编写消息生产者 286–291
  - 同步请求-响应方法用于通信状态变更 277–279
    - 服务间脆弱性 278
    - 在组织服务变更中添加新消费者缺乏灵活性 278–279
    - 服务间紧密耦合 278
- 高效存储 395
- EKS（Elastic Kubernetes Service）345
- Eksctl 347
- Elastic Container Registry（ECR）358, 365
- Elastic Container Service（ECS）16
- 弹性 18
- ELK（Elasticsearch、Logstash 和 Kibana）堆栈
  - 配置 Kibana 320–323
  - 在服务中配置 logback
    - 添加 Logstash 编码器 314–315
    - 创建 Logstash TCP Appender 315–318
  - 创建 EC2 实例 353–356
  - 创建 EKS 集群 357–364
    - 在 EKS 集群中部署微服务 361–364
    - 配置 Kubernetes 集群 358
    - 将微服务镜像推送到仓库 358–360
  - 在 Docker 中定义并运行 ELK 堆栈应用程序 318–320
  - 在 EC2 实例中部署 356–357
  - 实现 313–314
  - 在 Kibana 中搜索 trace ID 323
- @EnableBinding 注解 287, 294
- EnableBinding 注解 294
- @EnableDiscoveryClient 注解 171
- @EnableEurekaClient 注解 36, 215
- @EnableEurekaServer 注解 162
- @EnableFeignClients 注解 174
- /encrypt 端点 146
- ENCRYPT_KEY 变量 145
- 加密
  - 加密和解密属性 146–147
  - 设置对称加密密钥 144–145
- @Entity 注解 134
- ENTRYPOINT 命令 102
- environment 部分 329
- ES_HOSTS 变量 329
- Eureka 33
  - 构建 Spring Eureka 服务 158–162
  - 配置 Spring Cloud Gateway 与其通信 215–216
  - 使用 Netflix Feign 客户端调用服务 174–175
  - 使用 Spring Eureka 注册服务 162–167
  - 仪表板 167
  - REST API 166
  - 服务发现 156–157
- Eureka 服务器 158
- eureka.client.fetchRegistry 属性 161, 165
- eureka.client.registerWithEureka 属性 161, 165
- eureka.client.serviceUrl.defaultZone 属性 161
- eureka.instance.hostname 属性 161
- eureka.instance.preferIpAddress 属性 164–165
- eureka.server.waitTimeInMsWhenSyncEmpty 属性 161
- eureka.serviceUrl.defaultZone 属性 165
- EurekaServerApplication 类 161
- 事件驱动架构。参见 EDA（事件驱动架构）
- expires_in 属性 258
- 外部化配置 380

## F
- FaaS（Function as a Service）16
- failureRateThreshold 193
- fallback 处理模式
  - 实现 193–194
  - 概述 180
- fallbackMethod 属性 194
- 容错服务发现 152
- @FeignClient 注解 175
- FeignException 176
- Filter 类 202
- FilterUtils 271
- filter() 方法 229–230, 271
- 过滤器
  - 构建接收关联 ID 的后置过滤器 237–238
  - 构建前置过滤器 228–231
  - 自定义 225–228
  - Filters Factories 222–224
  - 在服务中使用关联 ID 231–236
    - 自定义 RestTemplate 和 UserContextInterceptor 235–236
    - UserContext 234–235
    - UserContextFilter 233–234
- Filters Factories
  - 内置 224
  - 概述 222–223
- FilterUtils 类 230
- FilterUtils setCorrelationId() 方法 230–231
- findById() 方法 337
- firstName 参数 13
- 灵活查询语言 395
- FROM 命令 102
- Function as a Service（FaaS）16

## G
- getCorrelationId() 方法 230
- getInstances() 方法 172
- getLicense() 方法 69, 79–80, 137, 170
- getLicenses() 方法 205
- getLicensesByOrganization() 方法 189–190, 197, 205
- getOrganization() 方法 175, 303
- getRestTemplate() 方法 173
- getStatusCode() 方法 176
- getUsername() 方法 271
- Git 33, 139–140
- GitHub 365–367
- GKE（Google Container Engine）16
- GlobalFilter 类 229
- Globally Universal ID（GUID）228
- Google Container Engine（GKE）16
- Grafana，配置 398–402
- grant_type 参数 256
- 授权 241
- 建立粒度 59–61
- group 属性 295
- GUID（Globally Universal ID）228

## H
- HATEOAS（Hypertext as the Engine of Application State）379
- 高可用服务发现 152
- 主机类型 95
- 热备 127
- HTTP
  - 使用 Spring Cloud Gateway 向响应添加关联 ID 325–326
  - 拦截传入请求 233–234
  - 使标头易于服务访问 234–235
  - 使用 HTTPS SSL 进行所有服务通信 273
- HTTPRequest 71
- HttpSecurity 类 262
- Hypertext as the Engine of Application State（HATEOAS）379

## I
- IaaS（Infrastructure as a Service）16
- IAM Authenticator 347
- @Id 注解 134
- IDE（Integrated Development Environment）117
- IdP（身份提供者）241–242
- ignoreExceptions 参数 199
- 隐式授权类型 388–390
- inboundOrgChanges 通道 305
- Infrastructure as a Service（IaaS）16
- @Input 注解 304
- input 部分 319
- Integrated Development Environment（IDE）117
- 国际化，应用于许可服务 74–77
- intervalFunction 参数 199
- 逆向 Conway 手法 7

## J
- JAR_FILE 变量 102
- jarmode 系统属性 106
- JARs
  - 向单个服务添加 Spring Security 和 Keycloak JARs 259
  - 分层 106–107
- Java Persistence API（JPA）129
- Java，使用 Java 构建微服务 47–53, 63–80
  - 将国际化应用于许可服务 74–77
  - 启动应用程序 52–53
  - 构建进入微服务的门户 64–74
  - 实现 Spring HATEOAS 以显示相关链接 78–80
  - 设置环境 47
  - 骨架项目 47–51
- Jenkins 365, 367–372
- JPA（Java Persistence API）129
- JSON（JavaScript Object Notation）5
- JWTs（JSON Web Tokens）25, 35, 242, 270–272

## K
- KeepAliveDuration 197
- Keycloak
  - 概述 242–244
  - 保护组织服务 258–272
    - 向单个服务添加 Spring Security 和 Keycloak JARs 259
    - 配置服务指向 Keycloak 服务器 260
    - 定义谁以及什么可以访问服务 260–265
    - 解析 JWT 中的自定义字段 270–272
    - 传播访问令牌 265–270
  - 保护单个端点 244–258
    - 将 Keycloak 添加到 Docker 245
    - 对 O-stock 用户进行身份验证 255–258
    - 配置 O-stock 用户 253–255
    - 注册客户端应用程序 248–252
    - 设置 Keycloak 246–247
- KeycloakRestTemplate 类 268–269
- KeycloakWebSecurityConfigurerAdapter 类 260
- Kibana
  - 配置 320–323
  - 在其中搜索 trace ID 323
- Kibana Query Language（KQL）323
- kompose up 命令 362
- KQL（Kibana Query Language）323
- kubeconfig（Kubernetes Configuration）369
- Kubectl 347
- kubectl get nodes -o wide 命令 363
- kubectl get pods 命令 362
- kubectl get svc 命令 358
- Kubernetes
  - 创建流水线脚本 374
  - 配置集群 358
- kubernetesDeploy 命令 375

## L
- lastName 参数 13
- layertools 106
- 最小权限 273
- license.withComment() 值 137
- License 类 68, 70, 79–80, 133–134
- License 对象 70, 194
- LicenseRepository 接口 136
- LicenseController 类 64, 79, 169, 206
- LicenseController.java 类 205
- licenseId 参数 70
- LicenseRepository 类 135
- LicenseService 类 137, 170, 206
- LicenseServiceApplication 类 53
- LicenseServiceApplication 类 173, 236, 294
- LicenseServiceApplication.java 类 75
- licensing-service 164
- licensingGroup 295
- licensingService 类 136
- limitForPeriod 参数 201
- limitRefreshPeriod 200–201
- linkTo 方法 80
- 负载均衡服务发现 152
- 负载均衡模式 179–180
- @LoadBalanced 注解 173
- LoadBalancer 服务 362
- Locale 75, 77
- LocaleResolver 75
- 日志聚合 311
  - 使用 Spring Cloud Gateway 向 HTTP 响应添加关联 ID 325–326
  - 添加 Logstash 编码器 314–315
  - 配置 Kibana 320–323
  - 创建 Logstash TCP Appender 315–318
  - 在 Docker 中定义并运行 ELK 堆栈应用程序 318–320
  - 在 Kibana 中搜索 trace ID 323
  - Spring Cloud Sleuth/ELK 堆栈实现 313–314
- 日志关联 26
- 日志记录 45, 382
- 日志模式 26
- LoggingEventCompositeJsonEncoder 317
- LoggingEventCompositeJsonEncoder 类 315
- Logstash
  - 添加编码器 314–315
  - 创建 Logstash TCP Appender 315–318




logstash-logback-encoder 依赖 315
LogstashEncoder 类 315–316

M
macvlan 类型 95
main() 方法 53, 120
manningPublications 标签 319
映射诊断上下文（MDC）309
maven 命令 373
Maven 插件 365
maxConcurrentCalls 属性 197
maxRetryAttempts 参数 199
maxThreadPoolSize 属性 197
maxWaitDuration 属性 197
MDA（消息驱动架构）276
MDC（映射诊断上下文）309
Message 类 290
MessageBuilder 类 290
MessageChannel 类 289–290, 304
messageSource 75
消息传递 279–281
  缺点 281–282
  编排 282
  处理语义 281
  可见性 281
  持久性 280
  灵活性 281
  松耦合 280
  可伸缩性 280–281
方法参数 69
指标 27
指标服务 27
指标可视化套件 27
Micrometer
  实现 395–397
  设置 394–397
微服务 30, 90
  架构师的角色 56–62
  分解业务问题 56–59
  定义服务接口 61–62
  确立服务粒度 59–61
架构最佳实践 377–382
  API 网关 382
  CI/CD 380–381
  外部化配置 380
  日志记录 382
  监控 381
  Richardson 成熟度模型 378–380
  Spring HATEOAS 380
使用 Spring Boot 和 Java 构建 47–53
  启动应用程序 52–53
  设置环境 47
  骨架项目 47–51
与客户端通信 85–86
云计算 11–19
  使用 Spring Boot 构建微服务 11–14
  定义 16–17
  将微服务部署到云端的原因 18–19
定义 4–5
部署 376
  构建/部署流水线 376
  在云端设置核心基础设施 346–352
DevOps 工程师的角色 81–89
  传达微服务健康状态 86–89
  服务装配 83–84
  服务引导 84–85
  服务注册与发现 85–86
向微服务架构演进 2–7
指导原则 19–21
与 Docker 集成 99–108
  创建 Docker 镜像 99–104
  使用 Spring Boot 创建 Docker 镜像 104–107
  使用 Docker Compose 启动服务 107–108
管理配置 84–85
监控 392–402
  Grafana 398–402
  Micrometer 和 Prometheus 394–397
  Spring Boot Actuator 393
单体架构 3–4
n 层架构 2
本书概述 10–11
  材料的相关性 10–11
  相关示例 46
  材料的范围 10
打包与部署 83–84
模式
  应用程序指标模式 27–28
  构建与部署模式 28
  客户端弹性模式 23–24
  核心开发模式 21–22
  日志记录与追踪模式 26
  路由模式 22–23
  安全模式 24–25
将镜像推送到仓库 358–360
切换的原因 6–7
安全 274
  Keycloak 242–272
  锁定不需要的网络端口 274
  OAuth2 241–242
  使用服务网关访问 273
  将服务划分为公共 API 和私有 API 273

微服务（续）
软件工程师的角色 63–80
将国际化应用到许可服务 74–77
构建微服务入口 64–74
实现 Spring HATEOAS 以显示相关链接 78–80
十二要素应用最佳实践 37–46
管理进程 45–46
后端服务 42
构建、发布、运行 42–43
代码库 40
并发 44
配置 41
依赖 40
开发/生产一致性 44–45
可处置性 44
日志 45
端口绑定 43
进程 43
何时不使用 62–63
应用程序类型 63
构建分布式系统时的复杂性 62
数据事务与一致性 63
服务器或容器蔓延 63
与 Spring 一起 7–8
监控微服务 381, 392–402
Grafana 398–402
Micrometer 和 Prometheus
实现 395–397
设置 394–397
Spring Boot Actuator
添加 393
启用端点 393
单体架构 3–4
多维数据模型 395
多重集成 395
mvn 命令 320
mvn package 103
mvn spring-boot:run 命令 13, 124, 162

N
n 层架构 2
NanosToWait 设置 200
net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder 类 315
net.logstash.logback.encoder.LogstashEncoder 类 315
Netflix Eureka。参见 Eureka
Netflix Feign 客户端，使用其调用服务 174–175
nodePort 属性 362
NodePort 服务 362
none 类型 95

O
O-stock
验证用户身份 255–258
配置用户 253–255
部署 353–364
使用 ELK 创建 EC2 353–356
创建 EKS 集群 357–364
在 EC2 实例中部署 ELK 栈 356–357
在云端设置核心基础设施 346–352
使用 Amazon RDS 创建 PostgreSQL 数据库 348–350
在 Amazon 中创建 Redis 集群 351–352
OAuth2
授权类型 383–390
授权许可类型 386–388
客户端凭据授权类型 385
隐式授权类型 388–390
密码授权类型 384–385
刷新令牌 390
概述 241–242
OIDC（OpenID Connect）241–242
/openid-connect/token 端点 262
Organization 类 175
Organization 对象 303
organization-service 属性 164
OrganizationChangeHandler 类 306
OrganizationChangeModel 类 289–290, 294
OrganizationFeignClient 类 175
organizationId 元素 290
organizationId 参数 70, 175
OrganizationRestTemplateClient 类 269, 302
OrganizationService 类 291, 337
OrganizationServiceApplication 类 287
orgChangeTopic 334
output() 方法 289
@OutputChannel 注解 304
overlay 类型 95

P
PaaS（平台即服务）16
密码授权类型 384–385
password 参数 256
路径变量 11
@PathVariable 注解 13, 69, 71, 175
模式
应用程序指标模式 27–28
构建/部署模式 28
客户端弹性模式 23–24
核心开发模式 21–22
日志记录与追踪模式 26
路由模式 22–23
安全模式 24–25
点对点服务发现 152

# INDEX

+   PEP（策略执行点）209, 273
+   物理服务器 18
+   平台即服务（PaaS）16
+   POJO（普通 Java 对象）127, 284
+   策略执行点（PEP）209, 273
+   端口绑定 43
+   可移植性 91
+   后置过滤器 226
+   PostgreSQL 数据库，使用 Amazon
+   RDS 创建 348–350
+   postgresql 依赖 129
+   @PostMapping 注解 71
+   前置过滤器 225
+   谓词工厂
+   内置 223
+   概述 222–223
+   preferIpAddress 属性 165
+   进程 43
+   配置文件 130
+   Prometheus
+   实现 395–397
+   设置 394–397
+   property 属性 138
+   受保护资源 243
+   publishOrganizationChange() 方法 289
+   @PutMapping 注解 71

# Q

+   queueCapacity 属性 197

# R

+   限流器模式，实现 200–202
+   RateLimiter 200
+   @RateLimiter 注解 205
+   RDBMS（关系数据库管理
+   系统）127
+   readLicensingDataFromRedis 336
+   领域 246
+   recordExceptions 193
+   Redis
+   在 Docker 中配置 285
+   在 Amazon 中创建集群 351–352
+   用于缓存查找 298–304
+   配置许可服务 298
+   构建到 Redis 的数据库连接
+   服务器 298–300
+   定义 Spring Data Redis 仓库
+   300–301
+   使用 Redis 和许可服务存储并
+   读取组织数据 302–304
+   Redis-Template 对象 299
+   /refresh 端点 138
+   refresh_token 属性 258
+   @RefreshScope 注解 138
+   远程过程调用（RPC）378
+   RemoveRequestHeader 266
+   @Repository 注解 53, 135
+   RepresentationModel 80
+   @RequestBody 注解 71
+   @RequestHeader 注解 77
+   @RequestMapping 注解 66, 69, 175
+   Resilience4j
+   实现 185
+   设置许可服务以使用 186–187
+   ThreadLocal 与 202–207
+   resilience4j-circuitbreaker 构件 186
+   resilience4j-spring-boot2 构件 186
+   resilience4j-time-limiter 构件 186
+   弹性模式 207
+   隔板模式 180–181, 195–198
+   断路器模式 180, 187–193
+   添加到组织服务 191–192
+   自定义 192–193
+   回退处理模式 180, 193–194
+   重要性 181–184
+   负载均衡模式 179–180
+   限流器模式 200–202
+   Resilience4j
+   实现 185
+   设置许可服务以使用 186–187
+   ThreadLocal 与 202–207
+   重试模式 198–200
+   弹性服务发现 152
+   资源所有者 243
+   ResourceBundleMessageSource 75
+   响应过滤器 228
+   @ResponseBody 注解 65
+   ResponseBody 类 65
+   ResponseEntity 类 176
+   ResponseEntity 对象 70
+   ResponseFilter 238, 326
+   REST（表现层状态转移）7
+   REST 端点 126
+   Rest-Template 类 170
+   @RestController 注解 65
+   RestTemplate 类 36, 169–171, 173, 176, 235–236
+   restTemplate.exchange() 174
+   retrieveOrganizationInfo() 方法 170
+   @Retry 注解 205
+   重试模式 198–200
+   retry-exceptions 参数 199
+   retryOnExceptionPredicate 参数 199
+   retryOnResultPredicate 参数 199
+   Richardson 成熟度模型 378–380
+   ringBufferSizeInClosedState 193



## 索引

- 软件工程师（开发者）的角色 63–80
  - 将国际化应用于许可服务 74–77
  - 构建微服务网关 64–74
  - 实现 Spring HATEOAS 以显示相关链接 78–80
- Source 类 287, 289
- Source 接口 294
- source.output() 方法 290
- spanExportable 字段 316
- SpanId 字段 316
- SPIA（单页互联网应用） 66
- Spotify Docker 插件 365
- spring.application.name 属性 160, 164
- Spring Boot
  - 使用 Spring Boot 构建微服务 11–14, 47–53, 63–80
    - 将国际化应用于许可服务 74–77
  - 启动应用程序 52–53
  - 构建微服务网关 64–74
  - 实现 Spring HATEOAS 以显示相关链接 78–80
  - 设置环境 47
  - 骨架项目 47–51
- 使用 Spring Boot 创建 Docker 镜像 104–107
  - Buildpacks 105
  - 分层 JAR 106–107
- Spring Boot Actuator
  - 添加 393
  - 启用端点 393
- Spring Boot 客户端，与 Spring Cloud 配置服务器集成 127–144
  - 配置许可服务以使用 Spring Cloud Config 129–132
  - 直接读取属性 137–138
  - 与 Vault 集成 141
  - 刷新属性 138–139
  - 设置许可服务依赖 128–129
  - 与 Git 一起使用 139–140
  - Vault 界面 141–144
- Spring Boot 控制器 64–74
- Spring Cloud
  - 定义 32–33
  - Hello World 示例 35–37
  - 服务发现 156–157
    - 构建 Spring Eureka 服务 158–162
    - 使用 Spring Eureka 注册服务 162–167
  - 设置许可服务以使用 186–187
  - 工具
    - Spring Cloud API Gateway 34
    - Spring Cloud Config 33
    - Spring Cloud LoadBalancer 和 Resilience4j 33–34
- Spring Cloud Security 35
- Spring Cloud 服务发现 33
- Spring Cloud Sleuth 34
  - 关联 ID 309–311
    - 将 Spring Cloud Sleuth 添加到许可服务和组织服务 309–310
    - 追踪结构解析 310–311
  - 日志聚合 311–326
    - 使用 Spring Cloud Gateway 向 HTTP 响应添加关联 ID 325–326
    - 添加 Logstash 编码器 314–315
    - 配置 Kibana 320–323
    - 创建 Logstash TCP 追加器 315–318
    - 在 Docker 中定义并运行 ELK 技术栈应用 318–320
    - 在 Kibana 中搜索追踪 ID 323
  - Spring Cloud Sleuth/ELK 技术栈实现 313–314
  - 设置依赖 328
- Spring Cloud Stream 34, 282–306
  - 在 Docker 中配置 Apache Kafka 和 Redis 285
  - 定义自定义通道 304–306
  - 使用 Redis 缓存查找 298–304
    - 配置许可服务 298
    - 构建到 Redis 服务器的数据库连接 298–300
    - 定义 Spring Data Redis 仓库 300–301
    - 使用 Redis 和许可服务存储和读取组织数据 302–304
  - 在许可服务中编写消息消费者 293–295
  - 在组织服务中编写消息生产者 286–291
- Spring Cloud API Gateway 34
- Spring Cloud 配置服务器 33–147
  - 构建 115–126
    - 设置引导类 120
    - 为服务设置配置文件 122–126
    - 与文件系统一起使用 121–122
  - 配置管理架构 112–114
  - 实现选择 114–115
  - 与 Spring Boot 客户端集成 127–144
    - 配置许可服务以使用 Spring Cloud Config 129–132
    - 直接读取属性 137–138
    - 与 Vault 集成 141
    - 刷新属性 138–139
    - 设置许可服务依赖 128–129
    - 与 Git 一起使用 139–140
    - Vault 界面 141–144
  - 装配数据源 133–137
  - 保护敏感配置信息 144–147
    - 加密和解密属性 146–147
    - 设置对称加密密钥 144–145
- Spring Cloud Gateway 239
  - 使用 Spring Cloud Gateway 向 HTTP 响应添加关联 ID 325–326
  - 构建接收关联 ID 的后置过滤器 237–238
  - 构建前置过滤器 228–231
  - 配置为与 Eureka 通信 215–216
  - 自定义过滤器 225–228
  - 定义 209–211
  - 过滤器工厂
    - 内置 224
    - 概述 222–223
  - 谓词工厂
    - 内置 223
    - 概述 222–223
  - 路由配置 216–222
    - 自动化 216–218
    - 动态重新加载 221–222
    - 手动 218–221
  - 设置项目 212–214
  - 在服务中使用关联 ID 231–236
    - 自定义 RestTemplate 和 UserContextInterceptor 235–236
    - UserContext 234–235
    - UserContextFilter 233–234
- Spring Cloud LoadBalancer 33–34, 173–174
- 使用 Spring Discovery Client 查找服务实例 171–172
- Spring Eureka
  - 构建服务 158–162
  - 使用 Spring Eureka 注册服务 162–167
    - 仪表盘 167
    - REST API 166
- Spring HATEOAS（超媒体作为应用状态引擎） 78–80, 380
- 使用 Spring REST 模板调用服务 173–174
- SPRING_PROFILES_ACTIVE 变量 131
- SPRING_CLOUD_CONFIG_URI 变量 131
- SPRING_PROFILES_ACTIVE 变量 108
- spring-boot-starter-aop 依赖 186
- spring-boot-starter-data-jpa 依赖 129
- spring-cloud-sleuth-zipkin 依赖 328
- spring-cloud-starter-config 依赖 129
- spring-cloud-starter-netflix-eureka-client 构件 163
- spring-cloud-starter-netflix-eureka-client 依赖 36
- spring-cloud-starter-sleuth 依赖 325
- spring-data-redis 依赖 298
- 使用 Spring 构建微服务 7–8
- spring.application.name 属性 130, 163, 215
- spring.cloud.config.server.git.property 属性 140
- spring.cloud.config.server.git.searchPaths 属性 140
- spring.cloud.config.server.git.uri 属性 140
- spring.cloud.config.uri 属性 130
- spring.datasource.password 属性 146
- spring.profiles.active 属性 130, 140
- spring.sleuth.sampler.percentage 属性 330
- spring.zipkin.baseUrl 属性 328
- @SpringBootApplication 注解 53
- SSO（单点登录） 242, 248
- STORAGE_TYPE 变量 329
- @StreamListener 注解 294
- SubscribableChannel 类 304
- 对称加密密钥
  - 加密和解密属性 146–147
  - 设置 144–145
- 同步请求-响应方式 277–279
  - 服务之间的脆弱性 278
  - 在组织服务的变更中添加新消费者的不灵活性 278–279
  - 服务之间的紧耦合 278

### T

- @Table 注解 134
- 目标服务 228
- Thread-Local 变量 228
- ThreadLocal 202–207, 288
- timeoutDuration 参数 201
- tmx-correlation-ID 头 230
- tmx-correlation-id 头 205, 228, 238, 326, 334
- token_type 属性 257
- TraceId 字段 316
- Tracer 类 326
- tracer.currentSpan().context().traceIdString() 方法 326
- 追踪模式 26
- 跟踪过滤器 228
- TrackingFilter 228, 230, 232
- 十二要素应用 39
- type—NodePort 属性 362

### U

- UDDI（统一描述、发现和集成） 148
- 解绑 4
- updateLicense() 方法 71
- URL（统一资源定位符） 72
- UserContextHolder 类 203
- UserContext 类 204, 206, 232–235, 288
- UserContext 对象 202–203
- UserContextFilter 类 232–233
- UserContextHolder 类 203–204, 235
- UserContextInterceptor 类 233, 235–236

### V

- value 属性 66
- value 参数 69
- Vault
  - 与 Spring Cloud 配置服务器集成 141
  - 界面 141–144
- VAULT_DEV_LISTEN_ADDRESS 参数 141
- VAULT_DEV_ROOT_TOKEN_ID 参数 141
- 速度 341
- 版本控制 40
- 虚拟容器 18
- 虚拟机镜像 18
- VM（虚拟机） 18, 38, 92–94
- VPC（虚拟私有云） 349

### W

- waitDuration 参数 199
- waitDurationInOpenState 193

### X

- X-B3-SpanId 字段 316
- X-B3-TraceId 字段 316

### Z

- Zipkin 327–338
  - 添加自定义 Span 336–338
  - 捕获消息追踪 334–335
  - 配置服务器 328–329
  - 配置服务以指向 Zipkin 328
  - 设置追踪级别 329–330
  - 设置依赖 328
  - 追踪事务 330–333
  - 可视化更复杂的事务 333

用户拥有访问令牌。

用户

O-stock Web 客户端

O-stock Web 应用程序

Spring Cloud Gateway

Keycloak 服务器

许可服务

组织服务

1. O-stock Web 应用程序调用许可服务（位于网关之后），并将用户的访问令牌添加到 Authorization HTTP 头中。
2. 网关定位许可服务，并将带有 Authorization 头的调用转发。
3. 许可服务使用认证服务验证用户的令牌，并将令牌传播给组织服务。
4. 组织服务同样使用认证服务验证用户的令牌。



## JAVA/SPRING

### Spring微服务实战（第二版）

Carnell • Sánchez

使用Spring构建和部署微服务可以很容易！Spring Boot、Spring Cloud和Spring Cloud Gateway等库减少了基于REST服务中的样板代码。它们提供了一个有效的工具箱，让你的微服务能够在公有云和私有云上快速启动并运行。

**Spring微服务实战（第二版）**将教你使用Java和Spring构建基于微服务的应用。你将从创建基础服务开始，然后过渡到高效的日志记录和监控。学习使用Spring直观的工具重构Java应用，并掌握使用Spring Cloud Gateway进行API管理。你甚至可以使用AWS和Kubernetes部署Spring Cloud应用。

### 本书内容

-   微服务设计原则与最佳实践
-   使用Spring Cloud Config和HashiCorp Vault进行配置
-   使用Resilience4j和Spring Cloud Load Balancer实现客户端弹性
-   使用Prometheus和Grafana进行指标监控
-   使用Spring Cloud Sleuth、Zipkin和ELK Stack进行分布式追踪

适合有经验的Java和Spring开发人员。

**John Carnell** 是一位资深云工程师，拥有20年Java经验。**Illary Huaylupo Sánchez** 是一位软件工程师，拥有超过13年的经验。

> “任何专业的Spring微服务开发者的书架上必备的一本书！”
> 
> —Iain Campbell，Tango Telecom

> “使用Spring构建云原生微服务的一种全面且实用的方法。包含大量实践、建议和乐趣。”
> 
> —Satej Sahu，Honeywell

> “无论是初学者还是有经验的Java开发人员，这都是完美的参考书。”
> 
> —Philippe Vialatte，Symphony

> “关于现代安全云部署实践的绝佳指南。”
> 
> —Todd Cook，Appen

![](img/3c6dc613426280ca3eafe43dea24391e_446_0.png)

