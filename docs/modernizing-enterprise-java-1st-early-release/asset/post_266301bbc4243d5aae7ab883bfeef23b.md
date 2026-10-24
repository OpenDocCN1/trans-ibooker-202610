
We just implemented a parameterized REST endpoint for our microservice, serving the JSON representation of the items contained in our CoolStore. In this way we provided a layer to query via HTTP GET requests our `Inventory` data model we implemented in the previous step.

> **TIP**
>
> With Quarkus, there is no need to create an `Application` class. It’s supported, but not required. In addition, only one instance of the resource is created and not one per request. You can configure this using the different Scoped annotations (`ApplicationScoped`, `RequestScoped`, etc).

## Run the app in Dev Mode

Development mode in Quarkus is one of the coolest features we have today for Cloud Native Java development. It enables hot deployment with background compilation, which means that when you modify your Java files or your resource files and then refresh your browser, the changes automatically take effect. This also works for resource files such as the configuration property file. In addition to that, refreshing the browser triggers a scan of the workspace, and if any changes are detected, the Java files are recompiled and the application is redeployed; your request is then serviced by the redeployed application. If there are any issues with compilation or deployment an error page will let you know.

You can start the app in dev mode with a built-in Maven goal named `quarkus:dev`. It enables hot deployment with background compilation, which means that when you modify your Java files or your resource files and refresh your browser these changes will automatically take effect. This works too for resource files like the configuration property file.

```
./mvnw compile quarkus:dev
```

After you start the app in dev mode, you should see an output like this:

> **NOTE**
>
> When we scaffolded the project at the beginning of this chapter, we included a series of dependencies like Panache and we use them to map our data model as entities into a database.

We can also see that our app is up and running, listening to port 8080. If you open your browser now at http://localhost:8080, you will see a Quarkus Welcome page (as in Figure 2-4).

![](img/8e80afd9c3d238f2221423409b47c62a_43_0.png)

Figure 2-4. Quarkus Welcome Page

> **TIP**
>
> You can stop the app running in dev mode by using CTRL-C from the same terminal where you launched it.

You can now try querying one of the items we inserted from the import.sql file, to test if our microservice is running properly.

Just navigate to http://localhost:8080/api/inventory/329299

You should have the following output:

```
{
  "id":"329299",
  "quantity":35
}
```

The REST API returned a JSON object representing the inventory count for this product. Congratulations on your first Cloud Native microservice with Quarkus!

> **NOTE**
>
> We are going now to develop the other microservices that will consume this one, so leave this open in order to have the CoolStore up and running at the end of this chapter.

## Create a Catalog Microservice with Spring Boot

Spring Boot is an opinionated framework that makes it easy to create stand-alone Spring-based applications with embedded web containers such as Tomcat (or JBoss Web Server), Jetty and Undertow that you can run directly on the JVM using java -jar. Spring Boot also allows producing a WAR file that can be deployed on stand-alone web containers.

The opinionated approach means many choices about Spring platform and third-party libraries are already made by Spring Boot so that you can get started with minimum effort and configuration.

Spring Boot is very popular for Cloud Native Java development because, quoting the official website, it makes it easy to create stand-alone, production-grade Spring-based Applications that you can “just run”1. We will include Spring Boot in our architecture for a Catalog microservice (as shown in Figure 2-5).

Spring Initializr is an online configurator that helps to generate the project structure with all dependencies needed. In this case, we will use a Red Hat-supported Spring Boot version from Red Hat Maven repositories, and will use these Project Metadata defined in Table 2-3.

**Table 2-3. Spring Boot Maven Project**

| Key | Value | Description |
|---|---|---|
| modelVersion | 4.0.0 | POM model version (always 4.0.0). |
| groupId | com.redhat.cloudnative | Group or organization that the project belongs to. Often expressed as an inverted domain name. |
| artifactId | catalog | Name to be given to the project's library artifact (for example, the name of its JAR or WAR file). |
| version | 1.0-SNAPSHOT | Version of the project that is being built |
| name | CoolStore Catalog Service | Name of the app |
| description | CoolStore Catalog Service with Spring Boot | A description for the app |

Let’s have a look at our pom.xml file:

```
<?xml version="1.0" encoding="UTF-8"?>
<project
    xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.redhat.cloudnative</groupId>
    <artifactId>catalog</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>CoolStore Catalog Service</name>
    <description>CoolStore Catalog Service with Spring Boot</description>
    <properties>
        <spring-boot.version>2.1.6.SP3-redhat-00001</spring-boot.version>
        <spring-boot.maven.plugin.version>2.1.4.RELEASE-redhat-00001</spring-boot.maven.plugin.version>
        <spring.k8s.bom.version>1.0.3.RELEASE</spring.k8s.bom.version>
        <fabric8.maven.plugin.version>4.3.0</fabric8.maven.plugin.version>
    </properties>
    <repositories>
        <repository>
            <id>redhat-ga</id>
            <url>https://maven.repository.redhat.com/ga/</url>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>redhat-ga-plugins</id>
            <url>https://maven.repository.redhat.com/ga/</url>
        </pluginRepository>
    </pluginRepositories>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>me.snowdrop</groupId>
                <artifactId>spring-boot-bom</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-kubernetes-dependencies</artifactId>
                <version>${spring.k8s.bom.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-kubernetes-config</artifactId>
        </dependency>
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
        </dependency>
    </dependencies>
    ...
</project>
```

- ① Project metadata we generated by Initializr or manually
- ② Spring Boot version used
- ③ Dependencies we need:
  - JPA: Spring Data with JPA
  - Spring Cloud: support and tooling from Spring for Cloud Native Java apps
  - H2: an in-memory database that we will use for this purpose
- ④ TK

This is a minimal Spring Boot project with support for RESTful services and Spring Data with JPA for connecting to a database. Any new project contains no code other than the main class, in this case, the *CatalogApplication* class which is there to bootstrap the Spring Boot application.

You find it in this book's GitHub repository.
