# Listing 3.2 Exploring the license model

```
package com.optimagrowth.license.model;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter @Setter @ToString
public class License {
    private int id;
    private String licenseId;
    private String description;
    private String organizationId;
    private String productName;
    private String licenseType;
}
```

A Plain Old Java Object (POJO) that contains the license info

# Lombok

Lombok is a small library that allows us to reduce the amount of boilerplate Java code written in the Java classes of our project. Lombok generates code such as getters and setters for string methods, constructors, and so forth.

In this book, we will use Lombok throughout the code examples in order to keep the code more readable, but we will not get into the details of how to use it. If you are interested in knowing more about Lombok, we highly recommend you look at the following Baeldung.com articles:

+   - https://www.baeldung.com/intro-to-project-lombok
+   - https://www.baeldung.com/lombok-ide

In case you want to install Lombok on the Spring Tools Suite 4, you must download and execute Lombok and link it to the IDE.

The following listing shows the service class that we'll use to develop the logic of the different services we are going to create on the controller class.

# Listing 3.3 Exploring the LicenseService class

```
package com.optimagrowth.license.service;

import java.util.Random;

import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;

import com.optimagrowth.license.model.License;
```