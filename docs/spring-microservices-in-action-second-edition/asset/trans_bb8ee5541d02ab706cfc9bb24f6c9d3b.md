
（续）

+   提高弹性，以便在你的主数据存储或数据库出现性能问题时，服务能够优雅地降级。根据你在缓存中保存的数据量，缓存解决方案有助于减少访问数据存储时可能遇到的错误数量。

Redis 远不止是一个缓存解决方案。不过，如果你需要分布式缓存，它完全可以胜任这个角色。

## 10.4.1 使用 Redis 缓存查找

在本节中，我们首先将许可服务设置为使用 Redis。幸运的是，Spring Data 让我们可以轻松地将 Redis 引入许可服务。要在许可服务中使用 Redis，我们需要执行以下操作：

+   1. 配置许可服务，使其包含 Spring Data Redis 依赖。
+   2. 构建到 Redis 的数据库连接。
+   3. 定义我们的代码将与 Redis 哈希交互所用的 Spring Data Redis 仓库。
+   4. 使用 Redis 和许可服务来存储和读取组织数据。

### 使用 Spring Data Redis 依赖配置许可服务

我们需要做的第一件事是将 spring-data-redis 依赖以及 jedis 包含到许可服务的 pom.xml 文件中。下一个清单展示了这些依赖。

### 清单 10.10 添加 Spring Redis 依赖

```
//Some code removed for conciseness
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-redis</artifactId>
</dependency>

<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <type>jar</type>
</dependency>
```

### 构建到 Redis 服务器的数据库连接

既然我们已经在 Maven 中添加了依赖，就需要建立到 Redis 服务器的连接。Spring 使用 Jedis 开源项目（https://github.com/xetorthio/jedis）与 Redis 服务器通信。为了与特定的 Redis 实例通信，我们将 JedisConnectionFactory 类作为 Spring bean 暴露出来。你可以在 /licensing-service/src/main/java/com/optimagrowth/license/LicenseServiceApplication.java 中找到该类的源码。

一旦我们建立了到 Redis 的连接，就会使用该连接创建一个 Spring `RedisTemplate` 对象。我们稍后实现的 Spring Data 仓库类将使用 `RedisTemplate` 对象来执行查询，并将组织服务数据保存到 Redis 服务中。下一个清单展示了这段代码。

### 清单 10.11 建立许可服务与 Redis 的通信方式

```
```java
package com.optimagrowth.license;

import org.springframework.data.redis.connection.RedisPassword;
import org.springframework.data.redis.connection.RedisStandaloneConfiguration;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;

//Most of the imports and annotations removed for conciseness

@SpringBootApplication
@EnableBinding(Sink.class)
public class LicenseServiceApplication {

    @Autowired
    private ServiceConfig serviceConfig;

    //All other methods in the class have been removed for conciseness

    @Bean
    JedisConnectionFactory jedisConnectionFactory() {
        String hostname = serviceConfig.getRedisServer();
        int port = Integer.parseInt(serviceConfig.getRedisPort());
        RedisStandaloneConfiguration redisStandaloneConfiguration
            = new RedisStandaloneConfiguration(hostname, port);
        return new JedisConnectionFactory(redisStandaloneConfiguration);
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(jedisConnectionFactory());
        return template;
    }

    //Rest of the code removed for conciseness
}
```
```

设置许可服务与 Redis 通信的基础工作已经完成。现在让我们继续编写用于获取、添加、更新和删除数据的逻辑。

`ServiceConfig` 是一个简单的类，包含获取我们将在许可服务配置文件中定义的自定义参数的逻辑；在本次

![](img/3c6dc613426280ca3eafe43dea24391e_328_0.png)

特定场景中，这些参数是 Redis 主机和端口。下一个清单展示了该类的代码。

### 清单 10.12 使用 Redis 数据设置 ServiceConfig 类

```
package com.optimagrowth.license.config;
//Imports removed for conciseness

@Component @Getter
public class ServiceConfig{

//Some code removed for conciseness

    @Value("${redis.server}")
    private String redisServer="";

    @Value("${redis.port}")
    private String redisPort="";
}
```

Spring Cloud Config 服务仓库在 /configserver/src/main/resources/config/licensing-service.properties 文件中为 Redis 服务器定义了以下主机和端口：

```
redis.server = localhost
redis.port = 6379
```

注意 我们使用 Docker 运行所有示例。如果你想在本地运行这个示例，需要在计算机上安装 Redis。但如果你使用 Docker，你可以在以下链接找到与 Redis 容器保持同步的 docker-compose.yml 文件：https://github.com/ihuaylupo/manning-smia/tree/master/chapter10/docker。

### 定义 Spring Data Redis 仓库

Redis 是一个键值数据存储，就像一个大型的、分布式的内存 HashMap。在最简单的情况下，它使用键来存储和查找数据。它没有复杂的查询语言来检索数据。它的简单性正是其优势所在，也是这么多开发者在项目中采用它的原因之一。

因为我们使用 Spring Data 来访问 Redis 存储，所以需要定义一个仓库类。你可能还记得前几章中，Spring Data 使用用户定义的仓库类，为 Java 类提供一种简单机制来访问 Postgres 数据库，而无需编写底层 SQL 查询。对于许可服务，我们将为 Redis 仓库定义两个文件。第一个文件是一个 Java 接口，将被注入到任何需要访问 Redis 的许可服务类中。下一个清单展示了 /licensing-service/src/main/java/com/optimagrowth/license/repository/OrganizationRedis-Repository.java 接口。

### 清单 10.13 OrganizationRedisRepository 定义了用于调用 Redis 的方法

```
package com.optimagrowth.license.repository;

import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;
import com.optimagrowth.license.model.Organization;

@Repository
public interface OrganizationRedisRepository extends
    CrudRepository<Organization,String>{
}
```

通过继承 CrudRepository，OrganizationRedisRepository 包含了用于从 Redis 存储和检索数据的所有 CRUD（创建、读取、更新、删除）逻辑（在这种情况下）。第二个文件是我们将用于仓库的模型。这个类是一个 POJO，包含我们将存储在 Redis 缓存中的数据。/licensing-service/src/main/java/com/optimagrowth/license/model/Organization.java 类如以下清单所示。

### 清单 10.14 Redis 哈希的组织模型

```
package com.optimagrowth.license.model;

import org.springframework.data.redis.core.RedisHash;
import org.springframework.hateoas.RepresentationModel;
import javax.persistence.Id;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter @Setter @ToString
@RedisHash("organization")
public class Organization extends RepresentationModel<Organization> {

    @Id
    String id;
    String name;
    String contactName;
    String contactEmail;
    String contactPhone;
}
```

设置 Redis 服务器中存储组织数据的哈希名称

从清单 10.14 的代码中需要注意的一个重要之处是，Redis 服务器中可以包含多个哈希和数据结构。因此，在与 Redis 的每次交互中，我们都需要告诉 Redis 我们要对其执行操作的数据结构的名称。

