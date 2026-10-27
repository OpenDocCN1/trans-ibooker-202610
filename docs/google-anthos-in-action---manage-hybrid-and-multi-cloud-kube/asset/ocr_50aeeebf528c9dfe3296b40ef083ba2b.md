efficiencies. For example, you will be able to build and test a single business domain service in isolation allowing for rapid iteration of capabilities. Standard contracts between services will create a well defined boundary to determine when you are introducing breaking changes that may affect other dependent services. You will have two options (1) incrementally extract and rewrite capabilities or (2) rewrite the entire application. Most of the time this will depend on the business’s goals and appetite for investment. We will cover option (1) briefly here as it is the most common approach we see customers adopting.

There are ways to peel off capabilities on the edge and rewrite them to be cloud native. A lot of teams start with common services like Authentication & Authorization, Logging, etc. This approach will allow your teams to take smaller bites of the apple to start moving your application architecture into a modern microservices design.

At this point you might be asking yourself, why not just use Migrate for Anthos on the legacy application and be done with it. The answer depends on your particular use case and the life of the application. That approach may be well suited for an application that is scheduled to sunset or be replaced. For applications that will be around longer you will want to eventually rewrite the application to take advantage of modern architecture patterns like Microservices and event based architecture.

There are many advantages to the aforementioned architectures highlighted below:

+   - Development Advantages
+   - Smaller teams focused on a single business problem domain.
+     - Modern techniques for development and testing of code to support 100% autonomous build, test and deployment of services. While it is possible to achieve 100% test coverage and automated deployments with legacy applications the amount of effort required is significantly greater than starting with a design that supports it up front.
+     - Decoupled services allow us to choose the technology stack that is best suited for the business capabilities the team is building.
+     - Different teams can manage them. This approach is often used to