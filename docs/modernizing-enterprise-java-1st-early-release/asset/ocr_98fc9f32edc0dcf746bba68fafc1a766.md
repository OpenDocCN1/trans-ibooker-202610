support through Istio or various frameworks like [Cloudstate]. You want to use features such as retry rules, circuit breaker, and pool ejection.

> **NOTE**
> 
> [Istio] is an open-source service mesh that layers transparently onto existing distributed applications. It is also a platform, including APIs that integrate into any logging platform, telemetry, or policy system.

# Security

Authentication or Authorization between services is not part of Kubernetes itself. There are two ways to implement it. Using Istio, each service is provided with a strong identity representing its role and enables interoperability across clusters and clouds. It secures service-to-service communication, as well as providing a key management to automate key and certificate generation, distribution, rotation, and revocation. A more application-centric alternative can be to use a single-sign-on component like [Keycloak] or relying on [Eclipse MicroProfile JSON-Web-Token] (JWT).

# Tracing

Tracing gives you a way to follow request paths and events throughout the system across individual application parts by still allowing you to trace-back to an origin. You can find different approaches across the community today. Independent of languages, frameworks, or technologies you intend to use, Istio can enable distributed tracing. There are other commercial and open-source projects available helping with distributed tracing across your application components. [Zipkin] and [Jaeger] are two possible solutions.

# Define your target platform

It’s important to note that the nine elements mentioned above are focused on application development and do not capture all the necessities of a modern container platform. Just looking at this narrow focus leaves important areas unaddressed. A container platform needs to provide