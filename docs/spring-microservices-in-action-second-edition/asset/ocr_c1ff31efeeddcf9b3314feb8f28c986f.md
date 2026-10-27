# appendix C  
Monitoring  
your microservices

Because microservices are distributed and fine-grained (small), they introduce a level of complexity to our applications that don’t exist in monolithic applications. Microservices architectures require a high degree of operational maturity, and monitoring becomes a critical part of their administration. If we research service monitoring, we will see that most people agree that it’s a fundamental process, but what exactly is monitoring? We define monitoring as the process of analysis, collection, and data storage such as application, platform, and system event metrics among others, that helps us visualize failure patterns within an IT environment.

Only failures? It is essential to clarify that failure is one of the most apparent reasons why monitoring is fundamental, but it isn’t the only one. The microservice performance also is considered another reason and, indeed, plays a critical role in our applications. We cannot describe the performance as a binary concept that is “up and running” or “down.” Service architectures can operate with a specific state of degradation that may affect the performance of one or more services. In the following sections, we show you how to monitor our Spring Boot microservices using several technologies such as Spring Boot Actuator, Micrometer, Prometheus, and Grafana. So, let’s start.

# C.1   Introducing monitoring with Spring Boot Actuator

Spring Boot Actuator is a library that provides monitoring and administration tools for our REST API in a reasonably simple way. It does this by organizing and exposing a series of REST endpoints that allow us to access different monitoring information to check the status of our services. In other words, Spring Boot Actuator