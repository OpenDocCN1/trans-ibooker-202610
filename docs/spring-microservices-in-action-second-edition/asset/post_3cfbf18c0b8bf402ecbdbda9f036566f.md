
![](img/3c6dc613426280ca3eafe43dea24391e_314_0.png)

Figure 10.4 When the organization service data changes, it publishes a message to orgChangeTopic.

## 10.3.1 Configuring Apache Kafka and Redis in Docker

In this section, we’ll explain how to add the Kafka and Redis services to our Docker environment for our message producer. To achieve this, let’s start by adding the code shown in the following listing to our docker-compose.yml file.

### Listing 10.1 Adding Kafka and Redis services to docker-compose.yml

```yaml
# Parts of docker-compose.yml removed for conciseness
...
zookeeper:
  image: wurstmeister/zookeeper:latest
  ports:
    - 2181:2181
  networks:
    backend:
      aliases:
        - "zookeeper"
kafkaserver:
  image: wurstmeister/kafka:latest
  ports:
    - 9092:9092
  environment:
    - KAFKA_ADVERTISED_HOST_NAME=kafka
    - KAFKA_ADVERTISED_PORT=9092
    - KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181
    - KAFKA_CREATE_TOPICS=addresses:1:1,ratings:1:1
  volumes:
    - "/var/run/docker.sock:/var/run/docker.sock"
  depends_on:
    - zookeeper
  networks:
    backend:
      aliases:
        - "kafka"
redisserver:
  image: redis:alpine
  ports:
    - 6379:6379
  networks:
    backend:
      aliases:
        - "redis"
```

## 10.3.2 Writing the message producer in the organization service

To focus on how to use topics in our architecture, we’ll begin by modifying the organization service so that every time organization data is added, updated, or deleted, the organization service will publish a message to a Kafka topic, indicating that an organization change event has occurred. The published message will include the organization ID associated with the change event and what action occurred (add, update, or delete).

The first thing we need to do is set up our Maven dependencies in the organization service’s Maven pom.xml file. You’ll find the pom.xml file in the root directory for the organization service. In the pom.xml, we need to add two dependencies, one for the core Spring Cloud Stream libraries and the other for the Spring Cloud Stream Kafka libraries:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-stream</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-stream-kafka</artifactId>
</dependency>
```

> **NOTE** We’re using Docker to run all the examples. If you want to run this locally, you need to have Apache Kafka installed on your computer. If you are using Docker, you can find the up-to-date docker-compose.yml file with the Kafka and Zookeeper containers here: https://github.com/ihuaylupo/manning-smia/tree/master/chapter10/docker.

Remember to execute the services by executing the following commands in the root directory where the parent pom.xml is located:

```bash
mvn clean package dockerfile:build && docker-compose
    -f docker/docker-compose.yml up
```

Once we’ve defined the Maven dependencies, we need to tell our application that it’s going to bind to a Spring Cloud Stream message broker. We can do this by annotating the organization service’s bootstrap class, OrganizationServiceApplication, with @EnableBinding. You’ll find the code for this class in /organization-service/src/main/java/com/optimagrowth/organization/OrganizationServiceApplication.java. For your convenience, the following listing shows the OrganizationServiceApplication.java source code.

### Listing 10.2 The annotated OrganizationServiceApplication.java class

```java
package com.optimagrowth.organization;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.context.config.annotation.RefreshScope;
import org.springframework.cloud.stream.annotation.EnableBinding;
import org.springframework.cloud.stream.messaging.Source;
import org.springframework.security.oauth2.config.annotation.web.configuration.EnableResourceServer;

@SpringBootApplication
@RefreshScope
@EnableResourceServer
@EnableBinding(Source.class)
public class OrganizationServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrganizationServiceApplication.class, args);
    }
}
```

![](img/3c6dc613426280ca3eafe43dea24391e_316_0.png)

In listing 10.2, the @EnableBinding annotation tells Spring Cloud Stream that we want to bind the service to a message broker. The use of Source.class in @EnableBinding tells Spring Cloud Stream that this service will communicate with the message broker via a set of channels defined in the Source class. Remember, channels sit above a message queue. Spring Cloud Stream has a default set of channels that can be configured to speak to a message broker.

At this point, we haven’t told Spring Cloud Stream what message broker we want the organization service to bind to. We’ll get to that shortly. Now, we can go ahead and implement the code that publishes a message. The first step is to change the UserContext class in `/organization-service/src/main/java/com/optimagrowth/organization/utils/UserContext.java`. This change will make our variables thread local. The following listing shows the code for the ThreadLocal class.

### Listing 10.3 Making our UserContext variables ThreadLocal

```java
package com.optimagrowth.organization.utils;
//Imports removed for conciseness

@Component
public class UserContext {
    public static final String CORRELATION_ID = "tmx-correlation-id";
    public static final String AUTH_TOKEN       = "Authorization";
    public static final String USER_ID          = "tmx-user-id";
    public static final String ORG_ID           = "tmx-org-id";

    private static final ThreadLocal<String> correlationId =
            new ThreadLocal<String>();
    private static final ThreadLocal<String> authToken =
            new ThreadLocal<String>();
    private static final ThreadLocal<String> userId =
            new ThreadLocal<String>();
    private static final ThreadLocal<String> orgId =
            new ThreadLocal<String>();

    public static HttpHeaders getHttpHeaders(){
        HttpHeaders httpHeaders = new HttpHeaders();
        httpHeaders.set(CORRELATION_ID, getCorrelationId());

        return httpHeaders;
    }
}
```

> Defining our variables as ThreadLocal lets us store data individually for the current thread. The information set here can only be read by the thread that set the value.

The next step is to create the logic to publish the message. You’ll find the code to publish our message in the `/organization-service/src/main/java/com/optimagrowth/organization/events/source/SimpleSourceBean.java` class file. The following listing shows the code for this class.

### Listing 10.4 Publishing a message to the message broker

```java
package com.optimagrowth.organization.events.source;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cloud.stream.messaging.Source;
import org.springframework.messaging.support.MessageBuilder;
import org.springframework.stereotype.Component;

import com.optimagrowth.organization.events.model.OrganizationChangeModel;
import com.optimagrowth.organization.utils.UserContext;

@Component
public class SimpleSourceBean {
    private Source source;

    private static final Logger logger =
        LoggerFactory.getLogger(SimpleSourceBean.class);

    public SimpleSourceBean(Source source){
        this.source = source;
    }

    public void publishOrganizationChange(ActionEnum action,
                                           String organizationId){
        logger.debug("Sending Kafka message {} for Organization Id: {}",
                     action, organizationId);
        OrganizationChangeModel change = new OrganizationChangeModel(
            OrganizationChangeModel.class.getTypeName(),
            action.toString(),
            organizationId,
            UserContext.getCorrelationId());

        source.output().send(MessageBuilder
                              .withPayload(change)
                              .build());
    }
}
```

![](img/3c6dc613426280ca3eafe43dea24391e_318_0.png)

In listing 10.4, we injected the Spring Cloud Source class into our code. Remember, all communication to a specific message topic occurs through a Spring Cloud Stream construct called a channel, which is represented by a Java interface class. In the listing, we used the Source interface, which exposes a single method called output().

The Source interface is convenient to use when our service only needs to publish to a single channel. The output() method returns a class of type MessageChannel. With this type, we’ll send messages to the message broker. (Later in this chapter, we’ll show you how to expose multiple messaging channels using a custom interface.) The ActionEnum passed by the parameters in the output() method contains the following actions:

```java
public enum ActionEnum {
    GET,
    CREATED,
    UPDATED,
    DELETED
}
```

The actual publication of the message occurs in the publishOrganizationChange() method. This method builds a Java POJO called OrganizationChangeModel. The following listing shows the code for this POJO.
