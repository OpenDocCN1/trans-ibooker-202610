You can find all the source code for this example in the [book’s GitHub repository](...).

# Create Quarkus Maven project

With Quarkus, you can scaffold a new project either with *Maven* than *Gradle*.

> **TIP**
> 
> Both Maven and Gradle are popular ways to setup a Java project and manage all dependencies. They differs in the dependency management strategy and they have different configuration formats (XML vs Kotlin DSL), but they are mostly equivalent in terms of capabilities. In this book we will use Maven as it has wider support through IDEs and tools.

We set up a new Maven project using `quarkus-maven-plugin` with this command:

```
mvn io.quarkus:quarkus-maven-plugin:2.1.4.Final:create \
    -DprojectGroupId=com.redhat.cloudnative \
    -DprojectArtifactId=inventory-quarkus \
    -DprojectVersion=1.0.0-SNAPSHOT \
    -DclassName="com.redhat.cloudnative.InventoryResource" \
    -Dextensions="quarkus-resteasy,quarkus-junit5,rest-assured,\
    quarkus-resteasy-jsonb,quarkus-hibernate-orm-panache,quarkus-\
    jdbc-h2" \
```

> **TIP**
> 
> You can also bootstrap a Quarkus app with the online configurator available at [https://code.quarkus.io/](https://code.quarkus.io/)

This will create a skeleton project, with an `InventoryResource` class that we will use for implementing our e-commerce Inventory microservice.

Let’s have a look at the generated `pom.xml` file: