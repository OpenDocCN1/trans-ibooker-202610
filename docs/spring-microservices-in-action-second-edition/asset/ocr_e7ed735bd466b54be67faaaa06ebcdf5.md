```
```properties
spring.datasource.testWhileIdle = true
spring.datasource.validationQuery = SELECT 1
management.endpoints.web.exposure.include=*
management.endpoints.enabled-by-default=true
```
```

> **Think before you implement**
> 
> We advise against using a filesystem-based solution for medium to large cloud applications. Using the filesystem approach means that you need to implement a shared file mount point for all Configuration Servers that want to access the application’s configuration data. Setting up shared filesystem servers in the cloud is doable, but it puts the onus of maintaining this environment on you.
> 
> We’re showing the filesystem approach as the easiest example to use when getting your feet wet with the Spring Cloud Configuration Server. In a later section, we’ll show you how to configure the Config Server to use GitHub and HashiCorp Vault to store your application configuration.

To continue, let’s create a licensing-service-dev.properties file that contains only the development data. The dev properties file should contain the following parameters:

```
```properties
example.property = I AM DEV
spring.datasource.url = jdbc:postgresql://localhost:5432/ostock_dev
spring.datasource.username = postgres
spring.datasource.password = postgres
```
```

Now that we have enough work done to start the Configuration Server, let’s go ahead and do that by using the `mvn spring-boot:run` command or the `docker-compose up` command.

> **NOTE**  From this point on, you will find a README file in the folder repository for each chapter. This file contains a section called “How to Use.” This section describes how to run all the services together using the docker-compose command.

The server should now come up with the Spring Boot splash screen on the command line. If you point your browser to `http://localhost:8071/licensing-service/default`, you’ll see a JSON payload returned with all of the properties contained within the licensing-service.properties file. Figure 5.6 shows the results of calling this endpoint.