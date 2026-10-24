Figure 12.5 An API Gateway is the single entry point for applications. Any request from an application goes to the API Gateway, which delegates to the rest of the microservice system to fulfill the request.

# ADVANTAGES

The main benefit of the API Gateway pattern is that it decouples applications nicely from the way the system is decomposed into microservices and hides that completely from applications. In cases where several applications have overlapping functionality or where some applications are built by third parties, using the API Gateway pattern facilitates the following:

+   - Maintaining a low barrier to entry for building applications
+   - Keeping the public API stable
+   - Keeping the public API backward compatible