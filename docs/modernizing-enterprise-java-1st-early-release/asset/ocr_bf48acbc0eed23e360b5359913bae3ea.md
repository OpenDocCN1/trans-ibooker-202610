Another approach is to use an HTTP proxy. It is essential to have the proxy up in production before you even try to extract parts of the monolith. Ensure it does not break the existing monolith and take some time to push the new service into production regularly, even without it being used by end-users. Gradually switch over by redirecting traffic if everything looks good.

For more simple service to monolith interactions, you can even think about implementing a simple JAX-RS direct communication. This approach is only suitable when you work with very few services, though. Make sure to treat the extracted service as an integration system from the perspective of the monolith.

All three approaches (API, Gateway, HTTP Proxy, and JAX-RS interface) are a pathway to your first successful microservice. They all implement the strangler pattern (refer to Chapter 3) and help you to refactor the monolith into separate systems as a first step.

Interception is a potentially dangerous path: If you start building a custom protocol translation layer that is shared by multiple services, you risk adding too much intelligence to the shared proxy. This design approach leads away from independent microservices and becomes a more service-oriented architecture with too much intelligence in the routing layer. A better alternative is the so-called Sidecar pattern, which basically describes an additional container in the pod. Rather than placing custom proxy logic in a shared layer, it becomes part of the new service. As Kubernetes sidecar, it becomes a runtime binding and can serve Legacy clients and new clients.

> NOTE
> A sidecar is just a container that runs on the same Pod as the application container. It shares the same volume and network as the application container and can “help” or enhance application behaviour with this. Typical examples are logging, or more generally agent functionality.

# Database to Databases