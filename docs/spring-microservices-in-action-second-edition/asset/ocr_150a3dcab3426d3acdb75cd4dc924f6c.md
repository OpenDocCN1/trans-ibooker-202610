# BUILD PACKS

Buildpacks are tools that provide application and framework dependencies, transforming our source code into a runnable application image. In other words, Buildpacks detect and obtain everything the application needs to run.

Spring Boot 2.3.0 introduces support for building Docker images using Cloud Native Buildpacks. This support was added to the Maven and Gradle plugins using the spring-boot:build-image goal for Maven and the bootBuildImage task for Gradle. For more information, see the following links:

+   - Spring Boot Maven Plugin at https://docs.spring.io/spring-boot/docs/2.3.0.M1/maven-plugin/html/
+   - Spring Boot Gradle Plugin at https://docs.spring.io/spring-boot/docs/2.3.0.M1/gradle-plugin/reference/html/

In this book, we will only explain how to use the Maven scenario. To build the image using this new feature, execute the following command from the root directory of your Spring Boot microservice:

```
./mvnw spring-boot:build-image
```

Once the command executes, you should be able to see output similar to that shown in the next code snippet:

```
[INFO]      [creator]      Setting default process type 'web'
[INFO]      [creator]      *** Images (045116f040d2):
[INFO]      [creator]      docker.io/library/licensing-service:0.0.1-SNAPSHOT
[INFO]
[INFO] Successfully built image 'docker.io/library/licensing-service:0.0.1-SNAPSHOT'
```

If you want to customize the name of the image created, you can add the following plugin to your pom.xml file and then define the name under the configuration section:

```
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <image>
            <name>${docker.image.prefix}/${project.artifactId}:latest</name>
        </image>
    </configuration>
</plugin>
```

Once the image is successfully built, you can execute the following command to start the container via Docker:

```
docker run -it -p8080:8080 docker.io/library/licensing-service:0.0.1-SNAPSHOT
```