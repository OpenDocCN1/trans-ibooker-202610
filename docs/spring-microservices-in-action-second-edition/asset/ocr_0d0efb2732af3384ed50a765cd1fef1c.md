A client makes one HTTP request, using the GET HTTP verb, and sends the name via pathVariable and the last name via requestParam.

HTTP GET request to the microservice http://localhost:8080/hello/illary?lastname=huaylupo

HTTP status: 200
HTTP response

```
{
"message" : "Hello illary huaylupo"
}
```

The client receives the response from your service as JSON. The success or failure of the call is returned as an HTTP status code.

Once Spring Boot has identified the route, it will map any parameters defined inside the route to a Java method that will carry out the work.

Once all of the data has been mapped, Spring Boot will execute the business logic.

Spring Boot will parse the HTTP request and map the route based on the HTTP verb, the URL, and potential parameters defined for the URL. A route maps to a method in a Spring RestController class.

For an HTTP PUT or POST, a JSON passed in the HTTP body is mapped to a Java class.

Once the business logic is executed, Spring Boot will convert a Java object to JSON.

Route mapping
Parameter destructing
JSON -> JAVA object mapping
Business logic execution
Java -> JSON mapping
Flow of Spring Boot microservice

Figure 1.5 Spring Boot abstracts away the common REST microservice tasks (routing to business logic, parsing HTTP parameters from the URL, mapping JSON to and from Java objects) and lets the developer focus on the business logic for the service. This figure shows three different ways to pass parameters to our controller.

will use this class to expose a REST endpoint called /hello. The following listing shows the code for the Application class.

# Listing 1.1 Hello World with Spring Boot: a (very) simple Spring microservice

Tells Spring Boot that this class is the entry point for the Spring Boot service

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
```

Tells Spring Boot to expose the code in this class as a Spring RestController

```
@SpringBootApplication
@RestController
@RequestMapping(value="hello")
public class Application {

public static void main(String[] args) {
SpringApplication.run(Application.class, args);
}

@GetMapping(value="/{firstName}")
```

Prefaces all URLs exposed in this application with a /hello prefix

Exposes an endpoint as a GET-based REST that takes two parameters in its firstName (via @PathVariable) and lastName (via @RequestParam)