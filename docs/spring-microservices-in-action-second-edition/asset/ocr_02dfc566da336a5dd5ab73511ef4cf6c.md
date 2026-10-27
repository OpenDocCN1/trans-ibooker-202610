```
<goals>
    <goal>build</goal>
    <goal>push</goal>
</goals>
</execution>
</executions>
</plugin>
</plugins>
</build>
```

Now that we have the plugin in our pom.xml file, we can continue the process by creating the variable docker.image.prefix mentioned in the previous code listing ①. This variable will indicate what prefix to use for our image. The following listing shows how to add the variable into the pom.xml.

# Listing 4.3 Adding the docker.image.prefix variable

```
<properties>
    <java.version>11</java.version>
    <docker.image.prefix>ostock</docker.image.prefix>
</properties>
```

Sets the value for the docker.image.prefix variable

There are several ways to define the value for the docker.image.prefix variable. Listing 4.3 shows but one option. Another way is to directly send the value using the -D option in the Maven JVM arguments. Note that if you don’t create this variable in the <properties> section of the pom.xml, when you execute the command to package and create the Docker image, the following error is thrown:

```
Failed to execute goal com.spotify:dockerfile-maven-plugin:1.4.0:build
(default-cli) on project licensing-service: Execution default-cli of goal
com.spotify:dockerfile-maven-plugin:1.4.0:build failed: The template variable
'docker.image.prefix' has no value
```

Now that we’ve imported the plugin into the pom.xml, let’s continue by adding the Dockerfile to our project. In the next sections, we will show you how to create a basic Dockerfile and a multistage build Dockerfile. Note that you can use both Dockerfiles because both files allow you to execute your microservice. The main difference between them is that with the basic Dockerfile, you’ll copy the entire JAR file of your Spring Boot microservice, and with the multistage build, you’ll copy only what’s essential to the application. In this book, we chose to use the multistage build to optimize the Docker image that we will create, but feel free to use the option that best suits your needs.

# BASIC DOCKERFILE

In this Dockerfile, we will copy the Spring Boot JAR file into the Docker image and then execute the application JAR. The following listing shows how to achieve this with a few simple steps.