# INDEX

+   containers
+     container sprawl 63
+     virtual machines vs. 92–94
+     continuous delivery (CD) 343, 380
+     continuous integration and continuous delivery (CI/CD) 344, 380–381
+   @Controller annotation 65
+   Controller class 66
+   Conway's law 7
+   core development pattern 21–22
+   coreThreadPoolSize 197
+   correlation ID
+     adding to HTTP response with Spring Cloud Gateway 325–326
+     building post-filter receiving 237–238
+     ensuring propagation of 235–236
+     Spring Cloud Sleuth and 309–311
+       adding to licensing and organization 309–310
+       anatomy of trace 310–311
+     using in services 231–236
+       custom RestTemplate and UserContextInterceptor 235–236
+     UserContext 234–235
+     UserContextFilter 233–234
+   correlationId element 290
+   createLicense() method 71, 77
+   credential management and propagation 25
+   cross-cutting concerns 222
+   CRUD (Create, Read, Update, Delete) 61, 134, 301
+   CrudRepository class 135
+   CustomChannels interface 304–305

# D

+   data transactions 63
+   decomposing 4
+   /decrypt endpoint 146
+   @DeleteMapping annotation 71
+   dependencies 40
+     setting up licensing service dependencies 128–129
+     setting up Spring Cloud Sleuth and Zipkin dependencies 328
+     twelve-factor app best practices 40
+   dependency injection framework 7
+   dependency:tree goal 51
+   deploying microservices 376
+     build/deployment pipeline 376
+       architecture of 342–346
+       creating 366–374
+       in action 364–366
+     deploying O-stock 353–364
+       creating EC2 instances with ELK 353–356

+   creating EKS clusters 357–364
+   deploying ELK stack in EC2 instance 356–357
+   setting up O-stock core infrastructure in cloud 346–352
+   creating PostgreSQL database using Amazon RDS 348–350
+   creating Redis cluster in Amazon 351–352
+   development/production parity 44–45
+   DevOps (development operations) 38
+   DevOps engineer's role 81–89
+     communicating microservice health 86–89
+     service assembly 83–84
+     service bootstrapping 84–85
+     service registration and discovery 85–86
+   DiscoveryClient class 172
+   disposability 44
+   distributed caching 297–306
+   distributed tracing 339, 382
+     Spring Cloud Sleuth
+       correlation ID and 309–311
+       log aggregation and 311–326
+     Zipkin 327–338
+       adding custom spans 336–338
+       capturing messaging traces 334–335
+       configuring server 328–329
+       configuring services to point to 328
+       setting tracing levels 329–330
+       setting up dependencies 328
+       tracing transactions 330–333
+       visualizing more complex transactions 333
+   Docker 109
+     adding Keycloak to 245
+     configuring Apache Kafka and Redis in 285
+     containers vs. virtual machines 92–94
+     defined 94–96
+     defining and running ELK stack applications in 318–320
+   Docker Compose 97–98
+   Dockerfiles 96
+   integrating with microservices 99–108
+     creating Docker images 99–104
+       creating Docker images with Spring Boot 104–107
+     launching services with Docker Compose 107–108
+   docker build command 96
+   Docker client 95
+   docker commit command 108
+   Docker Compose 97–98, 107–108
+   Docker containers 95
+   Docker daemon 95
+   Docker Images
+     creating 99–104
+     basic Dockerfiles 100
+     multistage build Dockerfiles 101–104