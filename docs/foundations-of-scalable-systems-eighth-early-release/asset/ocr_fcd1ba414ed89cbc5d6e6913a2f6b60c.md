which utilizes a load distribution policy to choose a target service replica for each request. Stateless services scale easily and simplify failure scenarios by allowing the load balancer to simply resend requests to responsive targets. Although most load balancers will support stateful services using a feature called sticky sessions, stateful services make load balancing and handling failures more complex. Hence, they are not recommended for highly scalable services.

API design is a topic of great complexity and debate. An excellent overview of basic API design and resource modeling is https://www.thoughtworks.com/insights/blog/rest-api-design-resource-modeling.

The Java Enterprise Edition (JEE) is an established and widely deployed server-side technology. It has a wide range of abstractions for building rich and powerful services. The Oracle tutorial is an excellent starting place for appreciating this platform - https://docs.oracle.com/javaee/7/tutorial/.

Much of the knowledge and information about load balancers is buried in the documentation provided by the technology suppliers. You choose your load balancer and then dive into the manuals. For an excellent, broad perspective on the complete field of load balancing, Server Load Balancing by Tony Bourke (O’Reilly) is a good resource.

+   1 Fielding, Roy Thomas (2000). “Architectural Styles and the Design of Network-based Software Architectures”. Dissertation. University of California, Irvine
+   2 https://app.swaggerhub.com/help/tutorials/openapi-3-tutorial
+   3 https://app.swaggerhub.com
+   4 https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH
+   5 https://developer.mozilla.org/en-US/docs/Web/HTTP/Compression
+   6 Node.js is a notable exception here as it is single threaded. However, it employs an asynchronous programming model for blocking input-output that supports handling many simultaneous requests.
+   7 https://tools.ietf.org/html/rfc6265
+   8 https://www.oracle.com/java/technologies/java-ee-glance.html