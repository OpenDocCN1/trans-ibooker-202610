
<?xml version="1.0" encoding="UTF-8"?>
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.redhat.cloudnative</groupId>
    <artifactId>inventory-quarkus</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <properties>
        <quarkus-plugin.version>2.1.4.Final</quarkus-plugin.version>
        <quarkus.platform.artifact-id>quarkus-bom</quarkus.platform.artifact-id>
        <quarkus.platform.group-id>io.quarkus</quarkus.platform.group-id>
        <quarkus.platform.version>2.1.4.Final</quarkus.platform.version>
        <compiler-plugin.version>3.8.1</compiler-plugin.version>
        <surefire-plugin.version>3.0.0-M5</surefire-plugin.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    <maven.compiler.parameters>true</maven.compiler.parameters>
    </properties>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>io.quarkus</groupId>
                <artifactId>quarkus-bom</artifactId>
                <version>${quarkus.platform.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.quarkus</groupId>
            <artifactId>quarkus-resteasy</artifactId>
        </dependency>
        <dependency>
            <groupId>io.quarkus</groupId>
            <artifactId>quarkus-junit5</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>io.rest-assured</groupId>
<artifactId>rest-assured</artifactId>
<scope>test</scope>
</dependency>
<dependency>
<groupId>io.quarkus</groupId>
<artifactId>quarkus-resteasy-jsonb</artifactId>
</dependency>
<dependency>
<groupId>io.quarkus</groupId>
<artifactId>quarkus-hibernate-orm-panache</artifactId>
</dependency>
<dependency>
<groupId>io.quarkus</groupId>
<artifactId>quarkus-jdbc-h2</artifactId>
</dependency>
</dependencies>
...
<build>
<plugins>
<plugin>
<groupId>io.quarkus</groupId>
<artifactId>quarkus-maven-plugin</artifactId>
<version>${quarkus-plugin.version}</version>
<executions>
<execution>
<goals>
<goal>native-image</goal>
</goals>
<configuration>
<enableHttpUrlHandler>true</enableHttpUrlHandler>
</configuration>
</execution>
</executions>
</plugin>
...
</profiles>
</project>

1 Here we setup groupId, artifactId and version. For a full list of available options, please see Table 2-1

2

Here you find the import of the Quarkus BOM, allowing you to omit the version on the different Quarkus dependencies.

Here you find all the dependencies for the project, that we expressed as extensions to add. We included for our purposes:

- [JSON REST Services](#): It allows you to develop REST services to consume and produce JSON payloads.
- [Hibernate ORM Panache](#): The de facto JPA implementation and offers you the full breadth of an Object Relational Mapper. Hibernate ORM with Panache focuses on simplifying Hibernate-based persistence layer, making your entities easier to write and maintain.
- [Datasources (H2)](#): Datasources are the main way of obtaining connections to a database, in this example, we will use H2, an in-memory database ready to use for Java apps.

The `quarkus-maven-plugin` which is responsible for the packaging of the application and also providing the development mode

Table

2-1. Quarkus Maven Project

| Attribute | Default Value | Description |
|---|---|---|
| projectGroupId | org.acme.sample | The group id of the created project |
| projectArtifactId | *mandatory* | The artifact id of the created project. Not passing it triggers the interactive mode. |
| projectVersion | 1.0-SNAPSHOT | The version of the created project |
| platformGroupId | io.quarkus | The group id of the target platform. Given that all the existing platforms are coming from io.quarkus this one won’t practically be used explicitly. But it’s still an option. |
| platformArtifactId | quarkus-universe-bom | The artifact id of the target platform BOM. It should be quarkus-bom in order to use the locally built Quarkus. |
| platformVersion | If it’s not specified, the latest one will be resolved. | The version of the platform you want the project to use. It can also accept a version range, in which case the latest from the specified range will be used. |
| className | *Not created if omitted* | The fully qualified name of the generated resource |
| path | /hello | The resource path, only relevant if className is set. |
| extensions | [] | The list of extensions to add to the project (comma-separated) |

> TIP
> 
> To check all the extensions available, use this command from project dir: `./mvnw quarkus:list-extensions`

## Create a Domain Model

It’s time to write some code now and create a *Domain model* and a RESTful endpoint to create the Inventory service. Domain model is a popular pattern in software engineering, and it fits very well also in the Cloud Native World. The level of abstraction given by the pattern, makes it still valid as an object-oriented way of modeling microservices business logic.

You can find the Domain Model definition in the *Inventory* class in this [book’s GitHub repository](book’s GitHub repository).

Our Domain Model implementation consists to an Entity mapped to the Persistence Layer, representing an inventory of items.

```
package com.redhat.cloudnative;

import javax.persistence.Entity;
import javax.persistence.Table;

import io.quarkus.hibernate.orm.panache.PanacheEntity;

import javax.persistence.Column;

@Entity ①
@Table(name = "INVENTORY") ②
public class Inventory extends PanacheEntity{ ③

    @Column
    public int quantity; ③

    @Override
    public String toString() {
        return "Inventory [Id='" + id + '\'' + ", quantity=" + quantity + ']';
    }
}
```

- ① @Entity marks the class as a JPA entity
- ② @Table customizes the table creation process by defining a table name and database constraint, in this case it is INVENTORY

Quarkus will generate getter/setter for you when using public attributes and when you extend PanacheEntity. Additionally, you have an `id` attribute automatically added.

Once we defined the model, we can update our Properties expressed in the `application.properties` file, in order to provide the instructions on how to populate data for our microservice.

```
quarkus.datasource.jdbc.url=jdbc:h2:mem:inventory;\
DB_CLOSE_ON_EXIT=FALSE;DB_CLOSE_DELAY=-1
quarkus.datasource.db-kind=h2
quarkus.hibernate-orm.database.generation=drop-and-create
quarkus.hibernate-orm.log.sql=true
quarkus.hibernate-orm.sql-load-script=import.sql
%prod.quarkus.package.uber-jar=true
```

- 1. JDBC path for the in-memory DB, this can be changed for other types of DB like any RDBMS.
- 2. An SQL script that we’ll use to populate the Coolstore with some data
- 3. An uber-jar contains all the dependencies required packaged in the jar to enable running the application with java -jar. By default, in Quarkus, the generation of the uber-jar is disabled. With the %prod prefix, this option is only activated when building the jar intended for deployments.

## Create a RESTful Service

Quarkus uses JAX-RS standard for building REST services. When scaffolding a new project as we saw before, a *hello* example service is created in the *className* path we defined. Now we want to expose REST service to retrieve the number of available items in the store from the inventory, defined as in [Table 2-2](#table-2-2).

| Path | HTTP Method | Description |
|---|---|---|
| /api/inventory/{item} | GET | Returns the quantity for a given item id present in the inventory database |

Let’s change the InventoryResource class definition as the following.

```
package com.redhat.cloudnative;

import javax.enterprise.context.ApplicationScoped;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.PathParam;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;

@Path("/api/inventory")
@ApplicationScoped
public class InventoryResource {

    @GET
    @Path("/{itemId}")
    @Produces(MediaType.APPLICATION_JSON)
    public Inventory getAvailability(@PathParam("itemId") long itemId) {
        Inventory inventory = Inventory.findById(itemId);
        return inventory;
    }
}
```

① By extending PanacheEntity, we’re using the Active Record persistence pattern instead of a DAO. This means that all persistence methods are blended with our own Entity.
