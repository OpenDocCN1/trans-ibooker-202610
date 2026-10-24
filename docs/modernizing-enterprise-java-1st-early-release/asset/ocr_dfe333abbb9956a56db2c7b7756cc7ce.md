② Spring Boot automatically provides an implementation for *ProductRepository* at runtime and injects it into the controller using the [@Autowired](https://...) annotation.

③ The `repository` attribute on the controller class is used to retrieve the list of products from the databases.

Everything now is ready to start our second microservice, which will listen to port 9000 to avoid conflicts with the other one.

```
```bash
mvn spring-boot:run
```
```

You should see an output like this:

```
```text
[INFO] --- spring-boot-maven-plugin:2.1.4.RELEASE-redhat-00001:run (default-cli)
@ catalog ---
[INFO] Attaching agents: []
2020-12-02 17:12:18.528  INFO 61053 --- [           main]
trationDelegate$BeanPostProcessorChecker : Bean
'org.springframework.cloud.autoconfigure.ConfigurationPropertiesR
 ebinderAutoConfiguration' of type
[org.springframework.cloud.autoconfigure.ConfigurationPropertiesR
 ebinderAutoConfiguration$$EnhancerBySpringCGLIB$$e898759c] is not
eligible for getting processed by all BeanPostProcessors (for
example: not eligible for auto-proxying)

  ____             _                 ____              _
 /\\ / ___'_ __ _ _(_)_ __  __ _    / ___|  ___   ___ | |_
( ( )\___ | '_ | '_| | '_ \/ _` |   \___ \ / _ \ / _ \| | |
 \\/  ___)| |_)| | | | | || (_| |    ___) | (_) | (_) | | |
  '  |____| .__|_| |_|_| |_\__, |   |____/ \___/ \___/|_| |_|
 =========|_|==============|___/================================
 :: Spring Boot ::        (v2.1.6.RELEASE)

StandardService : Starting service [Tomcat]
2020-12-02 17:12:20.064  INFO 61053 --- [           main]
org.apache.catalina.core.StandardEngine : Starting Servlet
Engine: Apache Tomcat/9.0.7.redhat-16
2020-12-02 17:12:20.220  INFO 61053 --- [           main]
o.a.c.c.C.[Tomcat].[localhost].[/] : Initializing Spring
embedded WebApplicationContext
```
```