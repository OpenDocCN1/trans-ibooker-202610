![](bbox=[0.13, 0.08, 0.89, 0.28])

# Figure 5.4 Spring Initializr Config Server and Spring Boot Actuator dependencies

Once you’ve created and imported the form as a Maven project into your preferred IDE, you should have a pom.xml file in the root of the config server project directory that looks like the code in the following listing.

# Listing 5.1 Maven pom file for the Spring Configuration Server

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

Spring Boot version

Spring Cloud version to use

Spring Cloud projects and other dependencies needed to run the ConfigServer