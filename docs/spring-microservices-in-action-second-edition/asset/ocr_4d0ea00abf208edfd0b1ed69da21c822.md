small), but instead, focus on registering the service with the Eureka service registry you created in the previous section.

In this section, we introduce a new service that we’ll call the *organization service*. This service will contain the CRUD endpoints. You can download the code for the licensing and organization services from this link:

https://github.com/ihuaylupo/manning-smia/tree/master/chapter6/Initial

> **NOTE**  At this point, you can use other microservices you might have. Just pay attention to the service ID names as you register them with service discovery.

The first thing we need to do is to add the Spring Eureka dependency to our organization and licensing services’ pom.xml files. The following listing shows how.

```
Listing 6.5  Adding the Spring Eureka dependency to the organization’s service pom.xml

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>
        spring-cloud-starter-netflix-eureka-client
    </artifactId>
</dependency>
```

Includes the Eureka libraries so that the service can register with Eureka
```

The spring-cloud-starter-netflix-eureka-client artifact holds the JAR files that Spring Cloud uses to interact with your Eureka service. After we’ve set up the pom.xml file, we need to make sure we have set the spring.application.name in the bootstrap.yml file of the service we want to register. The following listings, 6.6 and 6.7, indicate how to do this.

```
Listing 6.6  Adding the spring.application.name to the organization service

```yaml
spring:
  application:
    name: organization-service
    profiles:
      active: dev
  cloud:
    config:
      uri: http://localhost:8071
```

Logical name of the service that will be registered with Eureka
```

```
Listing 6.7  Adding the spring.application.name to the licensing service

```yaml
spring:
  application:
    name: licensing-service
    profiles:
      active: dev
  cloud:
    config:
      uri: http://localhost:8071
```

Logical name of the service that will be registered with Eureka
```