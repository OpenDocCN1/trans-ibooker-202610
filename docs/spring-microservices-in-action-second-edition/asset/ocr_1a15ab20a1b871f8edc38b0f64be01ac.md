# INDEX

Spring Cloud Security 35
Spring Cloud Service Discovery 33
Spring Cloud Sleuth 34
  correlation ID and 309–311
    adding Spring Cloud Sleuth to licensing and organization 309–310
    anatomy of trace 310–311
  log aggregation and 311–326
    adding correlation ID to HTTP response with Spring Cloud Gateway 325–326
    adding Logstash encoder 314–315
    configuring Kibana 320–323
    creating Logstash TCP Appender 315–318
    defining and running ELK stack applications in Docker 318–320
    searching for trace IDs in Kibana 323
  Spring Cloud Sleuth/ELK stack implementation 313–314
  setting up dependencies 328
Spring Cloud Stream 34, 282–306
  configuring Apache Kafka and Redis in Docker 285
  defining custom channels 304–306
  using Redis to cache lookups 298–304
    configuring licensing service 298
    constructing database connection to Redis server 298–300
    defining Spring data Redis repositories 300–301
    using Redis and licensing service to store and read organization data 302–304
  writing message consumer in licensing service 293–295
  writing message producer in organization service 286–291
Spring Discovery Client, looking up service instances with 171–172
Spring Eureka
  building service 158–162
  registering services with 162–167
    dashboard 167
    REST API 166
Spring HATEOAS (Hypermedia as the Engine of Application State) 78–80, 380
Spring REST template, invoking services with 173–174
SPRING_PROFILES_ACTIVE variable 131
SPRING_CLOUD_CONFIG_URI variable 131
SPRING_PROFILES_ACTIVE variable 108
spring-boot-starter-aop dependency 186
spring-boot-starter-data-jpa dependency 129
spring-cloud-sleuth-zipkin dependency 328
spring-cloud-starter-config dependency 129
spring-cloud-starter-netflix-eureka-client artifact 163

spring-cloud-starter-netflix-eureka-client dependency 36
spring-cloud-starter-sleuth dependency 325
spring-data-redis dependency 298
Spring, microservices with 7–8
spring.application.name property 130, 163, 215
spring.cloud.config.server.git.property 140
spring.cloud.config.server.git.searchPaths property 140
spring.cloud.config.server.git.uri property 140
spring.cloud.config.uri property 130
spring.datasource.password property 146
spring.profiles.active property 130, 140
spring.sleuth.sampler.percentage property 330
spring.zipkin.baseUrl property 328
@SpringBootApplication annotation 53
SSO (single sign-on) 242, 248
STORAGE_TYPE variable 329
@StreamListener annotation 294
SubscribableChannel class 304
symmetric encryption key
  encrypting and decrypting property 146–147
  setting up 144–145
synchronous request-response approach 277–279
  brittleness between services 278
  inflexibility in adding new consumers to changes in organization service 278–279
  tight coupling between services 278

# T

@Table annotation 134
target service 228
Thread-Local variable 228
ThreadLocal 202–207, 288
timeoutDuration parameter 201
tmx-correlation-ID header 230
tmx-correlation-id header 205, 228, 238, 326, 334
token_type attribute 257
TraceId field 316
Tracer class 326
tracer.currentSpan().context().traceIdString() method 326
tracing patterns 26
tracking filter 228
TrackingFilter 228, 230, 232
twelve-factor app 39
type—NodePort attribute 362

# U

UDDI (Universal Description, Discovery, and Integration) 148
unbundling 4