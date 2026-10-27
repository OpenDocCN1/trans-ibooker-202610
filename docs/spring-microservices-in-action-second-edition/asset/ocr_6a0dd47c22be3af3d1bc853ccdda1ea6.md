# ## 2.5 Building a microservice with Spring Boot and Java

In this section, we will build the skeleton of a microservice called *licensing service* for the Optima Growth company mentioned in the previous section. We’ll create all of the microservices using Spring Boot.

Spring Boot, as previously mentioned, is an abstraction layer over the Spring libraries that allows us to quickly build Groovy- and Java-based web applications and microservices with significantly less ceremony and configuration than in a full-blown Spring application. For the licensing service example, we’ll use Java as our core programming language and Apache Maven as our build tool. Over the next sections, we will

+   1. Create the basic skeleton of the microservice and a Maven script to build the application
+   2. Implement a Spring bootstrap class that will start the Spring container for the microservice and initiate the kick-off of any initialization work for the class

# ### 2.5.1 Setting up the environment

To start building our microservices, you should have the following components:

+   - Java 11 (http://mng.bz/ZP4m)
+   - Maven 3.5.4 or later (https://maven.apache.org/download.cgi)
+   - Spring Tools 4 (https://spring.io/tools) or you can download it within your selected Integrated Development Environment (IDE)
+   - IDEs such as
+     - Eclipse (https://www.eclipse.org/downloads/)
+     - IntelliJ IDEA (https://www.jetbrains.com/idea/download/)
+     - NetBeans (https://netbeans.org/features/index.html)

> **NOTE** All of the code listings from now on will be created using Spring Framework 5 and Spring Boot 2. It is important to understand that we’re not going to explain all the features of Spring Boot, we’re just going to highlight the essential ones in order to create the microservice. Another important fact is that we’ll use Java 11 in this book in order to reach the largest audience possible.

# ### 2.5.2 Getting started with the skeleton project

To begin, you’ll create a skeleton project for O-stock’s licensing service using Spring Initializr. Spring Initializr (https://start.spring.io/) enables you to create a new Spring Boot project with the possibility of choosing dependencies from an extensive list. Also, it allows you to change specific project configurations that you are about to create. Figures 2.12 and 2.13 show how the Spring Initializr page should look like for the licensing service.