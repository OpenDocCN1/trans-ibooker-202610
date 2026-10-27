src/main/java/com/optimagrowth/eureka/EurekaServerApplication.java class file. The following listing shows where to add the annotations.

# Listing 6.4 Annotating the bootstrap class to enable the Eureka Server

```
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

Enables the Eureka Server in the Spring service

At this point, we only use a new annotation, @EnableEurekaServer, to enable our service as an Eureka service. Now we can start the Eureka service by running the mvn spring-boot:run or run docker-compose commands. Once the startup command is executed, we should have a running Eureka service with no services registered in it. We first need to run the Spring Config service because it contains the Eureka application configuration. If you don’t run your configuration service first, you will get the following error:

```
Connect Timeout Exception on Url - http://localhost:8071.
Will be trying the next url if available.
    com.sun.jersey.api.client.ClientHandlerException:
    java.net.ConnectException: Connection refused (Connection refused)
```

To avoid the previous issue, try running the services with Docker Compose. Remember, you can find the docker-compose.yml file updated in the chapter repository on GitHub. Now, let’s move on to building out the organization service. Then we will register the licensing and the organization services with our Eureka service.

# 6.4 Registering services with Spring Eureka

At this point, we have a Spring-based Eureka Server up and running. In this section, we’ll configure the organization and licensing services to register themselves with our Eureka Server. This work is done in preparation for having a service client look up a service from our Eureka registry. By the time we’re done with this section, you should have a firm understanding of how to register a Spring Boot microservice with Eureka.

Registering a Spring Boot–based microservice with Eureka is a straightforward exercise. For the purposes of this chapter, we’re not going to walk through all of the Java code involved with writing the service (we purposely kept that amount of code