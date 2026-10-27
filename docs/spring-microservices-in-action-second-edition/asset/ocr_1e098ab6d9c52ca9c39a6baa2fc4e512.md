+   J
+   JAR_FILE variable 102
+   jarmode system property 106
+   JARs
+     adding Spring Security and Keycloak JARs to individual service 259
+     layered 106–107
+   Java Persistence API (JPA) 129
+   Java, building microservices with 47–53, 63–80
+     applying internationalization into licensing service 74–77
+     booting application 52–53
+     building doorway into microservice 64–74
+     implementing Spring HATEOAS to display related links 78–80
+     setting up environment 47
+     skeleton project 47–51
+   Jenkins 365, 367–372
+   JPA (Java Persistence API) 129
+   JSON (JavaScript Object Notation) 5
+   JWTs (JSON Web Tokens) 25, 35, 242, 270–272
+   K
+   KeepAliveDuration 197
+   Keycloak
+     overview 242–244
+     protecting organization service 258–272
+       adding Spring Security and Keycloak JARs to individual service 259
+       configuring service to point to Keycloak server 260
+       defining who and what can access service 260–265
+       parsing custom field in JWT 270–272
+       propagating access token 265–270
+     protecting single endpoint 244–258
+       adding Keycloak to Docker 245
+       authenticating O-stock users 255–258
+       configuring O-stock users 253–255
+       registering client application 248–252
+       setting up Keycloak 246–247
+   KeycloakRestTemplate class 268–269
+   KeycloakWebSecurityConfigurerAdapter class 260
+   Kibana
+     configuring 320–323
+     searching for trace IDs in 323
+   Kibana Query Language (KQL) 323
+   kompose up command 362
+   KQL (Kibana Query Language) 323
+   kubeconfig (Kubernetes Configuration) 369
+   Kubectl 347
+   kubectl get nodes -o wide command 363
+   kubectl get pods command 362
+   kubectl get svc command 358
+   Kubernetes
+     creating pipeline scripts 374
+     provisioning cluster 358
+   kubernetesDeploy command 375
+   L
+   lastName parameter 13
+   layerttools 106
+   least privilege 273
+   license .withComment() value 137
+   License class 68, 70, 79–80, 133–134
+   License object 70, 194
+   License-Repository interface 136
+   LicenseController class 64, 79, 169, 206
+   LicenseController.java class 205
+   licenseId parameter 70
+   LicenseRepository class 135
+   LicenseService class 137, 170, 206
+   LicenseService-Application class 53
+   LicenseServiceApplication class 173, 236, 294
+   LicenseServiceApplication.java class 75
+   licensing-service 164
+   licensingGroup 295
+   licensingService class 136
+   limitForPeriod parameter 201
+   limitRefreshPeriod 200–201
+   linkTo method 80
+   load balanced service discovery 152
+   load balancing pattern 179–180
+   @LoadBalanced annotation 173
+   LoadBalancer service 362
+   Locale 75, 77
+   LocaleResolver 75
+   log aggregation 311
+     adding correlation ID to HTTP response with Spring Cloud Gateway 325–326
+     adding Logstash encoder 314–315
+     configuring Kibana 320–323
+     creating Logstash TCP Appender 315–318
+     defining and running ELK stack applications in Docker 318–320
+     searching for trace IDs in Kibana 323
+     Spring Cloud Sleuth/ELK stack implementation 313–314
+   log correlation 26
+   loggerSink() method 294
+   logging 45, 382
+   logging patterns 26
+   Logging-EventCompositeJsonEncoder 317
+   LoggingEventCompositeJsonEncoder class 315
+   Logstash
+     adding encoder 314–315
+     creating Logstash TCP Appender 315–318