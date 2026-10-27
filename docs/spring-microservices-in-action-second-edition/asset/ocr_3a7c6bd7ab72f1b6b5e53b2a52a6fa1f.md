# Endpoint names matter

Before you get too far down the path of writing microservices, make sure that you (and potentially other teams in your organization) establish standards for the endpoints you want to expose via your services. The URLs (Uniform Resource Locator) for the microservice should be used to clearly communicate the intent of the service, the resources the service manages, and the relationships that exist between the resources managed within the service. We’ve found the following guidelines useful for naming service endpoints:

+   - *Use clear URL names* that establish what resource the service represents. Having a canonical format for defining URLs will help your API feel more intuitive and easier to use—and be consistent in your naming conventions.
+   - *Use the URL to establish relationships between resources.* Often times, you’ll have a parent-child relationship between resources within your microservices where the child doesn’t exist outside the context of the parent. Hence, you might not have a separate microservice for the child. Use the URLs to express these relationships. If you find that your URLs tend to be long and nested, your microservice may be doing too much.
+   - *Establish a versioning scheme for URLs early.* The URL and its corresponding endpoints represent a contract between the service owner and the consumer of the service. One common pattern is to prepend all endpoints with a version number. Establish your versioning scheme early and stick to it. It’s extremely difficult to retrofit versioning to URLs (for example, using `/v1/` in the URL mappings) after you already have several consumers using them.

At this point, you have several services. From a command-line window, go to your project root directory, where you’ll find pom.xml, and execute the following Maven command. (Figure 3.4 shows the expected output of this command.)

```
mvn spring-boot:run
```

![](bbox=[0.141, 0.677, 0.875, 0.771])

The license server starting on port 8080

Figure 3.4  Output showing that the licensing service successfully starts