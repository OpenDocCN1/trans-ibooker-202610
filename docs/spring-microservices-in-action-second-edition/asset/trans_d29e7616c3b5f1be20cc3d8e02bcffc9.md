
**表5.3 Spring Data和JPA框架的CRUD方法**

| 方法 | 描述 |
|---|---|
| `deleteById(id)` | 删除具有给定ID的实体 |
| `existsById(id)` | 返回是否存在具有给定ID的实体 |
| `findAll()` | 返回该类型的所有实例 |
| `findAllById(ids)` | 返回具有给定ID的给定类型T的所有实例 |
| `findById(ID id)` | 根据ID检索实体 |
| `save(entity)` | 保存给定的实体 |
| `saveAll(entities)` | 保存所有给定的实体 |

如果你想要构建更多方法，可以使用Spring Data Repository接口和基本的命名约定来构建这些方法。在启动时，Spring会解析Repository接口中的方法名称，根据名称将它们转换为SQL语句，然后在底层生成动态代理类来完成这项工作。许可服务的Repository如下面的清单所示。

**清单5.9 定义查询方法的 `LicenseRepository` 接口**

```java
package com.optimagrowth.license.repository;

import java.util.List;
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;
import com.optimagrowth.license.model.License;

@Repository
public interface LicenseRepository extends CrudRepository<License, String> {
    public List<License> findByOrganizationId(String organizationId);
    public License findByOrganizationIdAndLicenseId(String organizationId,
                                                    String licenseId);
}
```

Repository接口`LicenseRepository`标有`@Repository`，这告诉Spring它应将该接口视为一个Repository，并为其生成动态代理。在这种情况下，动态代理提供了一组功能完备、现成可用的对象。

Spring为数据访问提供了不同类型的Repository。在本例中，我们使用Spring的`CrudRepository`基类来扩展我们的`LicenseRepository`类。`CrudRepository`基类包含基本的CRUD方法。除了从CrudRepository扩展的CRUD方法外，我们还在LicenseRepository接口中添加了两个自定义查询方法，用于从许可表中检索数据。Spring Data框架会解析方法名称以构建查询，从而访问底层数据。

> **注意** Spring Data框架为各种数据库平台提供了抽象层，并且不仅限于关系型数据库。NoSQL数据库（如MongoDB和Cassandra）也同样受支持。

与第3章中许可服务的先前版本不同，现在你已将许可服务的业务逻辑和数据访问逻辑从LicenseController中分离出来，放入一个名为LicenseService的独立Service类中。清单5.10展示了该许可服务。由于我们添加了数据库连接，这个LicenseService类与之前章节中看到的版本相比有许多变化。你可以随意从以下链接下载该文件：

https://github.com/jhuaweiupo/manning-smia/tree/master/chapter5/licensing-service/src/main/java/com/optimagrowth/license/service/LicenseService.java

**清单5.10 一个用于执行数据库命令的LicenseService类**

```java
@Service
public class LicenseService {

    @Autowired
    MessageSource messages;
    @Autowired
    private LicenseRepository licenseRepository;
    @Autowired
    ServiceConfig config;

    public License getLicense(String licenseId, String organizationId){
        License license = licenseRepository
            .findByOrganizationIdAndLicenseId(organizationId, licenseId);
        if (null == license) {
            throw new IllegalArgumentException(
                String.format(messages.getMessage(
                    "license.search.error.message", null, null),
                    licenseId, organizationId));
        }
        return license.withComment(config.getProperty());
    }

    public License createLicense(License license){
        license.setLicenseId(UUID.randomUUID().toString());
        licenseRepository.save(license);
        return license.withComment(config.getProperty());
    }

    public License updateLicense(License license){
        licenseRepository.save(license);
        return license.withComment(config.getProperty());
    }

    public String deleteLicense(String licenseId){
        String responseMessage = null;
        License license = new License();
        license.setLicenseId(licenseId);
        licenseRepository.delete(license);
        responseMessage = String.format(messages.getMessage(
            "license.delete.message", null, null), licenseId);
        return responseMessage;
    }
}
```

控制器、服务和Repository类通过标准的Spring @Autowired注解装配在一起。接下来，让我们看看在LicenseService类中读取配置属性。

## 5.3.4 使用@ConfigurationProperties直接读取属性

在LicenseService类中，你可能已经注意到，我们在`getLicense()`方法中使用了来自`config.getProperty()`的值来设置`license.withComment()`的值。相关代码如下所示：

```java
return license.withComment(config.getProperty());
```

如果查看`com.optimagrowth.license.config.ServiceConfig.java`类，你会看到该类使用了以下注解。（清单5.11展示了我们将使用的@ConfigurationProperties注解。）

```java
@ConfigurationProperties(prefix = "example")
```

**清单5.11 使用ServiceConfig集中管理应用属性**

```java
package com.optimagrowth.license.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@ConfigurationProperties(prefix = "example")
public class ServiceConfig{
    private String property;
    public String getProperty(){
        return property;
    }
}
```

虽然Spring Data会“神奇地”将数据库的配置数据注入到数据库连接对象中，但所有其他自定义属性都可以通过@ConfigurationProperties注解进行注入。使用前面的示例，

```java
@ConfigurationProperties(prefix = "example")
```

会从Spring Cloud配置服务器中提取所有example属性，并将它们注入到ServiceConfig类的property属性中。

> **提示** 虽然可以直接将配置值注入到各个类的属性中，但我们发现将所有配置信息集中到一个配置类中，然后将该配置类注入到需要的地方会更有用。

## 5.3.5 使用Spring Cloud Config Server刷新属性

当开发团队想要使用Spring Cloud Configuration Server时，首先提出的问题之一是如何在属性发生变化时动态刷新他们的应用程序。请放心。Config Server始终提供属性的最新版本。通过其底层存储库对属性所做的更改将是最新的！

然而，Spring Boot应用程序只在启动时读取其属性，因此在Config Server中所做的属性更改不会被Spring Boot应用程序自动获取。但Spring Boot Actuator提供了一个@RefreshScope注解，允许开发团队访问/refresh端点，该端点将强制Spring Boot应用程序重新读取其应用程序配置。以下清单展示了这个注解的实际应用。

**清单5.12 @RefreshScope注解**

```java
package com.optimagrowth.license;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.context.config.annotation.RefreshScope;

@SpringBootApplication
@RefreshScope
public class LicenseServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(LicenseServiceApplication.class, args);
    }

}
```

关于@RefreshScope注解，有几点需要注意。这个注解只会重新加载应用程序配置中你自定义的Spring属性。
