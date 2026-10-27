
## 6.3 Building our Spring Eureka service

In this section, we’ll set up our Eureka service using Spring Boot. Like the Spring Cloud Config service, setting up a Spring Cloud Eureka service starts with building a new Spring Boot project and applying annotations and configurations. Let’s begin by creating this project with the Spring Initializr (https://start.spring.io/). To achieve this, we’ll follow these steps in the Spring Initializr:

- 1. Select Maven as the project type.
- 2. Select Java as the language.
- 3. Select the 2.2.x latest or a more stable Spring version.
- 4. Write com.optimagrowth as the group and eureka server as the artifact.
- 5. Expand the options list and write Eureka Server as the name, Eureka server as the description, and com.optimagrowth.eureka as its package name.
- 6. Select JAR Packaging.
- 7. Select Java 11 as the Java version.
- 8. Add the Eureka Server, Config Client, and Spring Boot Actuator dependencies as shown in figure 6.5. Listing 6.1 shows the Eureka Server pom.xml file.

![](img/3c6dc613426280ca3eafe43dea24391e_187_0.png)

*Figure 6.5 Eureka Server dependencies in Spring Initializr*

### Listing 6.1 Maven pom file for the Eureka Server

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.optimagrowth</groupId>
    <artifactId>eurekaserver</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Eureka Server</name>
    <description>Eureka Server</description>

    <properties>
        <java.version>11</java.version>
        <spring-cloud.version>Hoxton.SR1</spring-cloud.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-starter-ribbon</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>com.netflix.ribbon</groupId>
                    <artifactId>ribbon-eureka</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-loadbalancer</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
</project>
```

- Tells Maven to include the client that connects to a Spring Config Server to retrieve the application’s configuration.
- Tells Maven to include the Eureka libraries.
- Excludes the Netflix Ribbon libraries.
- Tells Maven to include the Spring Cloud Load Balancer libraries.

![](img/3c6dc613426280ca3eafe43dea24391e_190_0.png)

The key properties set in listing 6.3 are as follows:

- `server.port`—Sets the default port.
- `eureka.instance.hostname`—Sets the Eureka instance hostname for the Eureka service.
- `eureka.client.registerWithEureka`—Tells the Eureka server not to register with Eureka when the Spring Boot Eureka application starts.
- `eureka.client.fetchRegistry`—When set to false, tells the Eureka service that as it starts, it doesn’t need to cache its registry information locally. When running a Eureka client, you’ll want to change this value for the Spring Boot services that are going to register with Eureka.
- `eureka.client.serviceUrl.defaultZone`—Provides the service URL for any client. It is a combination of the `eureka.instance.hostname` and the `server.port` attributes.
- `eureka.server.waitTimeInMsWhenSyncEmpty`—Sets the time to wait before the server takes requests.

You’ll notice that the last attribute in listing 6.3, `eureka.server.waitTimeInMsWhenSyncEmpty`, indicates the time to wait in milliseconds before starting. When you’re testing your service locally, you should use this line because Eureka won’t immediately advertise any services that register with it. By default, it waits 5 minutes to give all of the services a chance to register with it before advertising them. Using this line for local testing helps to speed up the amount of time it takes for the Eureka service to start and to show the services registered with it.

> **NOTE** Individual services registering with Eureka take up to 30 seconds to show up in the Eureka service. That’s because Eureka requires three consecutive heartbeat pings from the service, which are spaced 10 seconds apart, before it will say the service is ready for use. Keep this in mind as you’re deploying and testing your own services.

The last piece of setup work you need to do for your Eureka service is to add an annotation to the application bootstrap class you use to start your Eureka service. For the Eureka service, you can find the application bootstrap class, `EurekaServerApplication`, in the `src/main/java/com/optimagrowth/eureka/EurekaServerApplication.java` class file. The following listing shows where to add the annotations.

### Listing 6.4 Annotating the bootstrap class to enable the Eureka Server

```java
package com.optimagrowth.eureka;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }

}
```

*Enables the Eureka Server in the Spring service*

At this point, we only use a new annotation, @EnableEurekaServer, to enable our service as an Eureka service. Now we can start the Eureka service by running the `mvn spring-boot:run` or `docker-compose` commands. Once the startup command is executed, we should have a running Eureka service with no services registered in it. We first need to run the Spring Config service because it contains the Eureka application configuration. If you don’t run your configuration service first, you will get the following error:

```text
Connect Timeout Exception on Url - http://localhost:8071.
Will be trying the next url if available.
    com.sun.jersey.api.client.ClientHandlerException:
    java.net.ConnectException: Connection refused (Connection refused)
```

To avoid the previous issue, try running the services with Docker Compose. Remember, you can find the docker-compose.yml file updated in the chapter repository on GitHub. Now, let’s move on to building out the organization service. Then we will register the licensing and the organization services with our Eureka service.

## 6.4 Registering services with Spring Eureka

At this point, we have a Spring-based Eureka Server up and running. In this section, we’ll configure the organization and licensing services to register themselves with our Eureka Server. This work is done in preparation for having a service client look up a service from our Eureka registry. By the time we’re done with this section, you should have a firm understanding of how to register a Spring Boot microservice with Eureka.

Registering a Spring Boot–based microservice with Eureka is a straightforward exercise. For the purposes of this chapter, we’re not going to walk through all of the Java code involved with writing the service (we purposely kept that amount of code
