

让我们回顾一下我们在第 5.1 节中列出的四个原则（隔离、抽象、集中化和加固），并看看这些原则在服务启动时如何应用。图 5.2 更详细地展示了启动过程，并显示了配置服务在此步骤中发挥的关键作用。

![](img/3c6dc613426280ca3eafe43dea24391e_142_0.png)

图 5.2 配置管理概念架构

在图 5.2 中，正在进行多个活动。以下是图中每一步的概述：

+   1. 当微服务实例启动时，它会调用一个服务端点来读取其配置信息，该信息特定于其运行的环境。配置管理的连接信息（连接凭据、服务端点等）随后在微服务启动时传递给微服务。

实际配置存储在存储库中。根据配置存储库的实现方式，您可以选择不同的方式来保存配置数据。这可以包括源代码控制下的文件、关系数据库或键值数据存储。

应用程序配置数据的实际管理与应用程序的部署方式无关。对配置管理的更改通常通过构建和部署管道处理，其中修改可以标记版本信息并部署到不同的环境（开发、 staging、生产等）。

当配置管理发生变化时，使用该应用程序配置数据的服务必须被通知该更改并刷新其应用程序数据的副本。

到目前为止，我们已经研究了说明配置管理模式不同组件及其如何协同工作的概念架构。现在我们将查看实现配置管理的不同解决方案，然后再查看具体的实现。

## 5.1.2 实现选择

幸运的是，您可以从大量经过实战检验的开源项目中选择来实现配置管理解决方案。让我们来看一下可用的几种不同选择并进行比较。表 5.1 列出了这些选项。

**表 5.1 用于实现配置管理系统 的开源项目**

| 项目名称 | 描述 | 特征 |
|---|---|---|
| etcd | 用 Go 编写。用于服务发现和键值管理。使用 raft 协议（https://raft.github.io/）作为其分布式计算模型。 | 非常快且可扩展；可分发；命令行驱动；易于使用和设置 |
| Eureka | 由 Netflix 编写。极其经过实战检验。用于服务发现和键值管理。 | 分布式键值存储；灵活但设置需要一些精力；开箱即用提供动态客户端刷新 |
| Consul | 由 HashiCorp 编写。与 etcd 和 Eureka 类似，但使用不同的算法作为其分布式计算模型。 | 快速；提供原生的服务发现，可直接与 DNS 集成；开箱即用不提供客户端动态刷新 |
| Zookeeper | Apache 项目。提供分布式锁定功能。常被用作配置管理解决方案来访问键值数据。 | 最老、最经过实战检验的解决方案<br>使用最复杂<br>可用于配置管理，但仅在您已在架构的其他部分使用 Zookeeper 时才考虑 |
| Spring Cloud Configuration Server | 开源项目。提供通用的配置管理解决方案，具有不同的后端。 | 非分布式键值存储<br>为 Spring 和非 Spring 服务提供紧密集成<br>可使用多个后端存储配置数据，包括共享文件系统、Eureka、Consul 或 Git |

表 5.1 中的所有解决方案都可以轻松用于构建配置管理解决方案。本章中的示例，以及本书其余部分的示例，我们将使用 Spring Cloud Configuration Server（通常称为 Spring Cloud Config Server 或简称为 Config Server），它非常适合我们的 Spring 微服务架构。我们选择此解决方案的原因是

- Spring Cloud Configuration Server 易于设置和使用。
- Spring Cloud Config 与 Spring Boot 紧密集成。您只需使用几个简单易用的注解即可读取应用程序的所有配置数据。
- Config Server 为存储配置数据提供多个后端。
- 在表 5.1 的所有解决方案中，Config Server 可以直接与 Git 源代码控制平台和 HashiCorp Vault 集成。我们将在本章后面解释这些主题。

在本章的其余部分，我们将执行以下操作：

+   1. 设置一个 Spring Cloud Configuration Server。我们将演示三种提供应用程序配置数据的不同机制——一种使用文件系统，另一种使用 Git 仓库，另一种使用 HashiCorp Vault。
+   2. 继续构建许可服务以从数据库检索数据。
+   3. 将 Spring Cloud Config 服务连接到您的许可服务以提供应用程序的配置数据。

## 5.2 构建我们的 Spring Cloud Configuration Server

Spring Cloud Configuration Server 是一个基于 REST 的应用程序，构建在 Spring Boot 之上。Config Server 不作为独立服务器提供。相反，您可以选择将其嵌入到现有的 Spring Boot 应用程序中，或启动一个新 Spring Boot 项目并将服务器嵌入其中。最佳实践是保持分离。

构建配置服务器的第一件事是使用 Spring Initializr（https://start.spring.io/）创建一个 Spring Boot 项目。为此，我们将在 Initializr 的表单中实现以下步骤。填写后，您的 Spring Initializr 表单应如下图所示 5.3 和 5.4 所示。在表单中，您将

+   1. 选择 Maven 作为项目类型。
+   2. 选择 Java 作为语言。
+   3. 选择最新或更稳定的 Spring 版本。
+   4. 将 `com.optimagrowth` 写为 group，将 `configserver` 写为 artifact。
+   5. 展开选项列表并写入。
      a. `Configuration Server` 作为其名称。
      b. `Configuration server` 作为其描述。
      c. `com.optimagrowth.configserver` 作为其包名。
+   6. 选择 JAR Packaging。
+   7. 选择 Java 11 作为 Java 版本。
+   8. 添加 Config Server 和 Spring Boot Actuator 依赖项。

![](img/3c6dc613426280ca3eafe43dea24391e_145_0.png)

图 5.3 Spring Configuration Server 的 Spring Initializr 配置

![](img/3c6dc613426280ca3eafe43dea24391e_146_0.png)

# 图 5.4 Spring Initializr Config Server 和 Spring Boot Actuator 依赖项

一旦您创建并将表单作为 Maven 项目导入到您喜欢的 IDE 中，您应该在 config server 项目目录的根目录中有一个 pom.xml 文件，其内容如下所示。

### 列表 5.1 Spring Configuration Server 的 Maven pom 文件

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.optimagrowth</groupId>
    <artifactId>configserver</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Configuration Server</name>
    <description>Configuration Server</description>
    <properties>
        <java.version>11</java.version>
        <spring-cloud.version>
            Hoxton.SR1
        </spring-cloud.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
```

Spring Boot 版本

要使用的 Spring Cloud 版本

运行 ConfigServer 所需的 Spring Cloud 项目和其他依赖项

