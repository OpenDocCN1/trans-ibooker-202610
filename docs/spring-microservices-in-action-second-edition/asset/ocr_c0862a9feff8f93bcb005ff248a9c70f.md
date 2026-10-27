While Spring Data “auto-magically” injects the configuration data for the database into a database connection object, all other custom properties can be injected using the @ConfigurationProperties annotation. With the previous example,

```
@ConfigurationProperties(prefix= "example")
```

pulls all the example properties from the Spring Cloud Configuration Server and injects these into the property attribute on the ServiceConfig class.

> TIP While it’s possible to directly inject configuration values into properties in individual classes, we’ve found it useful to centralize all of the configuration information into a single configuration class and then inject the configuration class into where it’s needed.

# 5.3.5 Refreshing your properties using Spring Cloud Config Server

One of the first questions that comes up from development teams when they want to use the Spring Cloud Configuration Server is how can they dynamically refresh their applications when a property changes. Rest assured. The Config Server always serves the latest version of a property. Changes made to a property via its underlying repository will be up to date!

Spring Boot applications, however, only read their properties at startup, so property changes made in the Config Server won’t be automatically picked up by the Spring Boot application. But Spring Boot Actuator offers a @RefreshScope annotation that allows a development team to access a /refresh endpoint that will force the Spring Boot application to reread its application configuration. The following listing shows this annotation in action.

# Listing 5.12 The @RefreshScope annotation

```
package com.optimagrowth.license;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.context.config.annotation.RefreshScope;

@SpringBootApplication
@RefreshScope
public class LicenseServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(LicenseServiceApplication.class, args);
    }

}
```

Note a couple of things about the @RefreshScope annotation. This annotation only reloads the custom Spring properties you have in your application configuration.