```
[INFO] --- maven-dependency-plugin:3.1.1:tree (default-cli) @ licensing-service ---
[INFO] com.optimagrowth:licensing-service:jar:0.0.1-SNAPSHOT
[INFO] +- org.springframework.boot:spring-boot-starter-hateoas:jar:2.2.3.RELEASE:compile
[INFO] |  \- org.springframework.hateoas:spring-hateoas:jar:1.0.3.RELEASE:compile
[INFO] |  +- org.springframework:spring-aop:jar:5.2.3.RELEASE:compile
[INFO] |  +- org.springframework:spring-beans:jar:5.2.3.RELEASE:compile
[INFO] |  +- org.springframework:spring-context:jar:5.2.3.RELEASE:compile
[INFO] |  +- org.springframework.plugins:spring-plugin-core:jar:2.0.0.RELEASE:compile
[INFO] |  \- org.slf4j:slf4j-api:jar:1.7.30:compile
[INFO] +- org.springframework.boot:spring-boot-starter-actuator:jar:2.2.3.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:2.2.3.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-autoconfigure:jar:2.2.3.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-starter-logging:jar:2.2.3.RELEASE:compile
[INFO] |  |  |  +- ch.qos.logback:logback-classic:jar:1.2.3:compile
[INFO] |  |  |  |  \- ch.qos.logback:logback-core:jar:1.2.3:compile
[INFO] |  |  |  +- org.apache.logging.log4j:log4j-to-slf4j:jar:2.12.1:compile
[INFO] |  |  |  \- org.apache.logging.log4j:log4j-api:jar:2.12.1:compile
[INFO] |  |  \- org.slf4j:jul-to-slf4j:jar:1.7.30:compile
[INFO] |  +- jakarta.annotation:jakarta.annotation-api:jar:1.3.5:compile
[INFO] |  +- org.yaml:snakeyaml:jar:1.25:runtime
[INFO] +- org.springframework.boot:spring-boot-actuator-autoconfigure:jar:2.2.3.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-actuator:jar:2.2.3.RELEASE:compile
[INFO] |  +- com.fasterxml.jackson.core:jackson-databind:jar:2.10.2:compile
[INFO] |  +- com.fasterxml.jackson.core:jackson-annotations:jar:2.10.2:compile
[INFO] |  +- com.fasterxml.jackson.core:jackson-core:jar:2.10.2:compile
[INFO] |  \- com.fasterxml.jackson.datatype:jackson-datatype-jsr310:jar:2.10.2:compile
[INFO] |  \- io.micrometer:micrometer-core:jar:1.3.2:compile
[INFO] |  +- org.hdrhistogram:HdrHistogram:jar:2.1.11:compile
[INFO] |  \- org.latencyutils:LatencyUtils:jar:2.0.3:compile
[INFO] +- org.springframework.boot:spring-boot-starter-web:jar:2.2.3.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-json:jar:2.2.3.RELEASE:compile
[INFO] |  |  +- com.fasterxml.jackson.datatype:jackson-datatype-jdk8:jar:2.10.2:compile
[INFO] |  |  +- com.fasterxml.jackson.module:jackson-module-parameter-names:jar:2.10.2:compile
[INFO] +- org.springframework.boot:spring-boot-starter-tomcat:jar:2.2.3.RELEASE:compile
[INFO] |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:9.0.30:compile
[INFO] |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:9.0.30:compile
[INFO] |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:9.0.30:compile
[INFO] +- org.springframework.boot:spring-boot-starter-validation:jar:2.2.3.RELEASE:compile
[INFO] |  +- jakarta.validation:jakarta.validation-api:jar:2.0.2:compile
[INFO] |  \- org.hibernate.validator:hibernate-validator:jar:6.0.18.Final:compile
[INFO] |  +- org.jboss.logging:jboss-logging:jar:3.4.1.Final:compile
[INFO] |  \- com.fasterxml:classmate:jar:1.5.1:compile
[INFO] +- org.springframework:spring-web:jar:5.2.3.RELEASE:compile
[INFO] +- org.springframework:spring-webmvc:jar:5.2.3.RELEASE:compile
[INFO] |  \- org.springframework:spring-expression:jar:5.2.3.RELEASE:compile
[INFO] +- org.springframework.boot:spring-boot-starter-test:jar:2.2.3.RELEASE:test
[INFO] |  +- org.springframework.boot:spring-boot-test:jar:2.2.3.RELEASE:test
[INFO] |  +- org.springframework.boot:spring-boot-test-autoconfigure:jar:2.2.3.RELEASE:test
```

The Maven dependency tree displays a tree of all dependencies used in the current project.

Tomcat 9.0.30 version

Spring framework 5.2.3.RELEASE version

Figure 2.15 Dependency tree for O-stock's licensing service. The dependency tree displays all of the dependencies declared and used in the service.

# 2.5.3 Booting your Spring Boot application: Writing the bootstrap class

Our goal in this section is to get a simple microservice up and running in Spring Boot and then iterate over it to deliver some functionality. To this end, you need to create two classes in your licensing service microservice:

+   - A Spring bootstrap class, which Spring Boot uses to start up and initialize the application
+   - A Spring controller class, which exposes the HTTP endpoints that can be invoked on the microservice

As you'll soon see, Spring Boot uses annotations to simplify setting up and configuring the service. This becomes evident as you look at the bootstrap class in the following code listing. You'll find this bootstrap class is in the LicenseServiceApplication.java file located in src/main/java/com/optimagrowth/license.