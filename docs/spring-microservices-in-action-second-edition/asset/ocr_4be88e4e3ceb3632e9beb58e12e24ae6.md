+   - *Building filters that can inspect and act on the requests and responses coming through the gateway.* These filters allow us to inject policy enforcement points in our code and to perform a wide number of actions on all of our service calls in a consistent fashion. In other words, these filters allow us to modify the incoming and outgoing HTTP requests and responses.
+   - *Building predicates, which are objects that allow us to check if the requests fulfill a set of given conditions before executing or processing a request.* The Spring Cloud Gateway includes a set of built-in Route Predicate Factories.

To get started with Spring Cloud Gateway, let’s

+   1. Set up a Spring Boot project for Spring Cloud Gateway and configure the appropriate Maven dependencies
+   2. Configure the gateway to communicate with Eureka

# 8.2.1  Setting up the Spring Boot gateway project

In this section, we’ll set up our Spring Cloud Gateway service using Spring Boot. Like the Spring Cloud Config service and the Eureka service that we already created in previous chapters, setting up a Spring Cloud Gateway service starts with building a new Spring Boot project and then applying annotations and configurations. Let’s begin by creating that new project with the Spring Initializr (https://start.spring.io/) as shown in figure 8.3.

![](bbox=[0.174, 0.478, 0.81, 0.766])

**Figure 8.3**  Spring Initializr with our Spring Cloud Gateway information

To achieve this setup, you’ll need to follow these steps. Listing 8.1 then shows how the Gateway server pom.xml file should look.

+   1. Select Maven as the project type.
+   2. Select Java as the language.