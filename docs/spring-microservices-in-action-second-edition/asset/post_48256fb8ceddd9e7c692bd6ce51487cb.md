
## How this book is organized: A roadmap

This book consists of 12 chapters and 3 appendixes:

- Chapter 1 introduces you to why microservices architecture is an important and relevant approach to building applications, especially cloud-based applications.
- Chapter 2 walks you through the Spring Cloud technologies that we’ll use and provides a guide on how to build cloud-native microservices following the twelve-factor application best practices. This chapter also walks you through how to build your first REST-based microservice using Spring Boot.
- Chapter 3 shows you how to look at your microservices through the eyes of an architect, application engineer, or DevOps engineer and provides a guide on how to implement some of the microservice best practices in your first REST-based microservice.
- Chapter 4 walks you through the container world, highlighting the main differences between containers and virtual machines (VMs). This chapter also shows you how to containerize your microservices using several Maven plugins and Docker commands.
- Chapter 5 introduces you to how to manage the configuration of your microservices using Spring Cloud Config. Spring Cloud Config helps guarantee that your service configurations are centralized in a single repository, versioned, and repeatable across all instances of your services.
- Chapter 6 introduces you to the service discovery routing pattern. You will learn how to use Spring Cloud and Netflix’s Eureka service to abstract the location of your services away from the clients consuming them. You’ll also learn how to implement client-side load balancing using the Spring Cloud LoadBalancer and a Netflix Feign client.
- Chapter 7 is about protecting the consumers of your microservices when one or more microservice instances are down or in a degraded state. This chapter demonstrates how to use Spring Cloud and Resilience4j to implement the circuit breaker, fallback, and bulkhead patterns.
- Chapter 8 covers the service gateway routing pattern. Using Spring Cloud Gateway, we build a single entry point to call all our microservices. We will demonstrate how to use the Spring Cloud Gateway filters to build policies that can be enforced against all services flowing through the service gateway.
- Chapter 9 covers how to implement service authentication and authorization using Keycloak. In this chapter, we cover some basic principles of OAuth2 and how to use Spring and Keycloak to protect your microservices architecture.
- Chapter 10 looks at how we can introduce asynchronous messaging into our microservices using Spring Cloud Stream and Apache Kafka. This chapter also shows you how to use Redis to cache lookups.
- Chapter 11 shows how to implement common logging patterns like log correlation, log aggregation, and tracing with Spring Cloud Sleuth, Zipkin, and the ELK Stack.

- Chapter 12 is the cornerstone project for this book. We take the services we have built throughout the book and deploy them to an Amazon Elastic Kubernetes Service (Amazon EKS). We also discuss how to automate the build and deployment of your microservices using tools like Jenkins.
- Appendix A shows additional microservices architecture best practices and explains the Richardson Maturity Model.
- Appendix B contains supplemental material on OAuth2. OAuth2 is an extremely flexible authentication model, and this chapter provides a brief overview of the different ways in which you can use OAuth2 to protect your application and its corresponding microservices.
- Appendix C covers how to monitor your Spring Boot microservices using several technologies such as Spring Boot Actuator, Micrometer, Prometheus, and Grafana.

In general, developers should read chapters 1, 2, and 3, which provide essential information about best practices and about implementing microservices using Spring Boot with Java 11. If you’re a reader who’s new to Docker, we highly recommend reading chapter 4 carefully because it briefly introduces all the Docker concepts used throughout the book.

The rest of the book discusses several microservice patterns, such as service discovery, distributed tracing, API Gateway, and more. The approach of this book is to read the chapters in order and follow the code examples for the chapters. But in case someone wants to skip the examples, they can download the code from the GitHub repository at https://github.com/ihuaylupo/manning-smia.

## About the code

This book contains code in almost every chapter, both in numbered listings and in line with normal text. In both cases, source code is formatted in a fixed-width font like this to separate it from ordinary text. All code examples are available in my GitHub repository at https://github.com/ihuaylupo/manning-smia.

Each chapter has a different folder in the repository. Note also that all the code in this book is built to run with Java 11 using Maven as the main build tool and Docker as the container tool. In the README.md file for each chapter, you can find the following information:

- A brief introduction to the chapter
- The tools required for the initial configuration
- A “how to use” section
- The build command for the examples
- The run command for the examples
- Contact and contributing information

One of the core concepts we followed throughout the entire book is that the code examples in each chapter should be able to run completely independent of any of the

other chapters. What does this mean? You should be able to grab the code from chapter 10, for example, and run it without following the examples in the previous chapters. You'll see that for every service built in each chapter there is a corresponding Docker image. Each chapter uses Docker Compose to execute the Docker images in order to guarantee that you have a reproducible run-time environment for each chapter.

In many cases, the original source code has been reformatted; we've added line breaks and reworked indentation to accommodate the available page space in the book. In rare cases, even this was not enough, and listings include line-continuation markers ( ). Additionally, comments in the source code have often been removed from the listings when the code is described in the text. Code annotations accompany many of the listings, highlighting important concepts.

## liveBook discussion forum

Purchase of Spring Microservices in Action, Second Edition, includes free access to a private web forum run by Manning Publications, where you can make comments about the book, ask technical questions, and receive help from the authors and from other users. To access the forum, go to https://livebook.manning.com/book/spring-microservices-in-action-second-edition/discussion. You can also learn more about Manning's forums and the rules of conduct at https://livebook.manning.com/#!/discussion.

Manning's commitment to our readers is to provide a venue where a meaningful dialogue between individual readers and between readers and authors can take place. It is not a commitment to any specific amount of participation on the part of the authors, whose contribution to the forum remains voluntary (and unpaid). We suggest you try asking them some challenging questions lest their interest stray! The forum and the archives of previous discussions will be accessible from the publisher's website as long as the book is in print.

## about the authors

John Carnell is a software architect and leads the Developer Engagement team for Genesys Cloud. John spends the majority of his day teaching Genesys Cloud customers and internal developers how to deliver cloud-based contact center and telephony solutions and best practices for cloud-based development. He works hands-on building telephony-based microservices using the AWS platform. His day-to-day job is to design and build microservices across a number of technology platforms including Java, Clojure, and Go. John is a prolific speaker and writer. He regularly speaks at local user groups and was a regular speaker on “The No Fluff Just Stuff Software Symposium.” Over the last 20 years, John has authored, coauthored, and functioned as a technical reviewer for a number of Java-based technology books and industry publications. John holds a BA from Marquette University and an MBA from the University of Wisconsin in Oshkosh. John is a passionate technologist who constantly explores new technologies and programming languages. When John is not speaking, writing, or coding, he lives in Cary, North Carolina, with his wife, Janet, his three children (Christopher, Agatha, and Jack), and yes, his dog, Vader.

Illary Huaylupo Sánchez is a software engineer who graduated from Cenfotec University and holds an MBA focused on IT management from the Latin American University of Science and Technology (ULACIT) in Costa Rica. Her knowledge of software development is quite extensive. She has experience working with Java and other programming languages such as Python, C#, Node.js, and with other technologies such as various databases, frameworks, cloud services, and more. Currently, Illary works as a Senior Software Engineer at Microsoft, San Jose, Costa Rica, where she spends most of
