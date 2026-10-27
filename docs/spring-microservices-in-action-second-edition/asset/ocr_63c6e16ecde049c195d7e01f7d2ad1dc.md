# Listing 4.4 A basic Dockerfile

```
#Start with a base image containing Java runtime
FROM openjdk:11-slim

# Add Maintainer Info
LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"

# The application's jar file
ARG JAR_FILE

# Add the application's jar to the container
COPY ${JAR_FILE} app.jar

#execute the application
ENTRYPOINT ["java","-jar","/app.jar"]
```

Specifies the Docker image to use in our Docker run time (in this case, openjdk:11-slim)

Defines the JAR_FILE variable set by dockerfile-maven-plugin

Copies the JAR file to the filesystem of the image named app.jar

Targets the licensing service application in the image when the container is created

# MULTISTAGE BUILD DOCKERFILE

In the Dockerfile for this section, we use a multistage build. Why multistage? This will allow us to discard anything that isn’t essential to the execution of the application. For example, with Spring Boot, instead of copying the entire target directory to the Docker image, we only need to copy what’s necessary to run the Spring Boot application. This practice will optimize the Docker image we create. The following listing shows how your Dockerfile should look.

# Listing 4.5 A Dockerfile with a multistage build

```
#stage 1
#Start with a base image containing Java runtime
FROM openjdk:11-slim as build

# Add Maintainer Info
LABEL maintainer="Illary Huaylupo <illaryhs@gmail.com>"

# The application's jar file
ARG JAR_FILE

# Add the application's jar to the container
COPY ${JAR_FILE} app.jar

#unpackage jar file
RUN mkdir -p target/dependency &&
    (cd target/dependency; jar -xf /app.jar)

#stage 2
#Same Java runtime
FROM openjdk:11-slim

#Add volume pointing to /tmp
VOLUME /tmp
```

Unpacks the app.jar copied previously into the filesystem of the build image

This new image contains the different layers of a Spring Boot app instead of the complete JAR file.