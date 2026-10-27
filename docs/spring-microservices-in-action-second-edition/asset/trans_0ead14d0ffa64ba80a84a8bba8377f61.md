
# Spring
# 微服务
# 实战
第二版

约翰·卡内尔
玛丽·瓦伊卢波·桑切斯

![](img/3c6dc613426280ca3eafe43dea24391e_0_0.png)

MANNING

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

有关本书及其他Manning图书的在线信息和订购，请访问 www.manning.com。出版社对批量订购提供折扣。如需更多信息，请联系

特别销售部
Manning Publications Co.
20 Baldwin Road
PO Box 761
Shelter Island, NY 11964
电子邮件：orders@manning.com

©2021 Manning Publications Co. 保留所有权利。

未经出版者事先书面许可，不得以任何形式或通过电子、机械、影印或其他方式复制、存储于检索系统或传播本出版物的任何部分。

制造商和销售商用来区分其产品的许多名称被声明为商标。凡在本书中出现这些名称，且Manning Publications已知悉商标声明时，这些名称均以首字母大写或全部大写形式印刷。

Manning认识到保护所写内容的重要性，其政策是将我们出版的书籍印刷在无酸纸上，并为此尽最大努力。同时认识到我们保护地球资源的责任，Manning的书籍印刷用纸至少含有15%的再生纤维，且加工过程中不使用元素氯。

Manning Publications Co.
20 Baldwin Road
PO Box 761
Shelter Island, NY 11964

开发编辑：Leslie Trites
技术开发编辑：Robert Wenner
审稿编辑：Aleksandar Dragosavljević
制作编辑：Deirdre Hiam
文字编辑：Frances Buran
校对：Katie Tennant
技术校对：Stephan Pirnbaum
排版：Gordan Salinovic
封面设计：Marija Tudor

ISBN 9781617296956
美国印刷

我将此书献给所有
正在追求STEM职业的女性。

只要努力，一切皆有可能。

## 简要目录

- 1. 欢迎来到云，Spring 1
- 2. 使用Spring Cloud探索微服务世界 31
- 3. 使用Spring Boot构建微服务 55
- 4. 欢迎来到Docker 91
- 5. 使用Spring Cloud配置服务器控制你的配置 110
- 6. 关于服务发现 148
- 7. 当坏事发生时：使用Spring Cloud和Resilience4j的弹性模式 177
- 8. 使用Spring Cloud Gateway进行服务路由 208
- 9. 保护你的微服务安全 240
- 10. 使用Spring Cloud Stream的事件驱动架构 275
- 11. 使用Spring Cloud Sleuth和Zipkin进行分布式追踪 307
- 12. 部署你的微服务 340

## 目录

前言 xvii
致谢 xix
关于本书 xxi
关于作者 xxv
关于封面插图 xxvii

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
