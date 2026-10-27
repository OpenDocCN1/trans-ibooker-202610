different technology implementations. While we’ve chosen to use Spring Boot and Spring Cloud to implement the patterns we’re going to use in this book, nothing will keep you from taking the concepts presented here and using these with other technology platforms. Specifically, we’ll cover the following microservice patterns:

+   - Core development pattern
+   - Client resiliency patterns
+   - Logging and tracing patterns
+   - Build and deployment pattern
+   - Routing patterns
+   - Security patterns
+   - Application metrics patterns

It’s important to understand that there isn’t a formal definition of how to create a microservice. In the next section, you’ll see a list of common aspects you need to take into consideration while building a microservice.

# 1.7 Core microservice development pattern

The core microservice development pattern addresses the basics of building a microservice. Figure 1.11 highlights the topics we’ll cover around basic service design.

![](bbox=[0.18, 0.44, 0.88, 0.86])

Figure 1.11 When designing your microservice, you need to think about how the service will be consumed and communicated with.