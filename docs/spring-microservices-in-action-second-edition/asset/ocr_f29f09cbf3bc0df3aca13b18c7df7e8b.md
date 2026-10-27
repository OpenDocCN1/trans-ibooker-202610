# 1.9 Microservice client resiliency 23

# 1.10 Microservice security patterns 24

# 1.11 Microservice logging and tracing patterns 26

# 1.12 Application metrics pattern 27

# 1.13 Microservice build/deployment patterns 28

# 2 Exploring the microservices world with Spring Cloud 31

2.1 What is Spring Cloud? 32

Spring Cloud Config 33 • Spring Cloud Service Discovery 33

Spring Cloud LoadBalancer and Resilience4j 33 • Spring Cloud API Gateway 34 • Spring Cloud Stream 34 • Spring Cloud Sleuth 34 • Spring Cloud Security 35

2.2 Spring Cloud by example 35

2.3 How to build a cloud-native microservice 37

Codebase 40 • Dependencies 40 • Config 41 • Backing services 42 • Build, release, run 42 • Processes 43 • Port binding 43 • Concurrency 44 • Disposability 44 • Dev/prod parity 44 • Logs 45 • Admin processes 45

2.4 Making sure our examples are relevant 46

2.5 Building a microservice with Spring Boot and Java 47

Setting up the environment 47 • Getting started with the skeleton project 47 • Booting your Spring Boot application: Writing the bootstrap class 52

# 3 Building microservices with Spring Boot 55

3.1 The architect’s story: Designing the microservice architecture 56

Decomposing the business problem 56 • Establishing service granularity 59 • Defining the service interfaces 61

3.2 When not to use microservices 62

Complexity when building distributed systems 62 • Server or container sprawl 63 • Application type 63 • Data transactions and consistency 63

3.3 The developer’s tale: Building a microservice with Spring Boot and Java 63

Building the doorway into the microservice: The Spring Boot controller 64 • Adding internationalization into the licensing service 74 • Implementing Spring HATEOAS to display related links 78