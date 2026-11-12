When multiple domains are involved in composite services, careful attention must be paid to consistency, integrity, and security. It’s important to register each underlying service in a central repository so domains know what dependencies they might have with other domains. Speaking of that, I don’t encourage building composite services on top of other composite services because the orchestration might end up in a chaotic web of calls from one API to another. With respect to security, keep the rule structure and decision logic as close as possible to the originating domain. Other domains shouldn’t be able to overrule it or give a higher priority to local rules.

# ## API Contracts

When multiple teams are working on the same API, it’s crucial for all of them to agree on the specifications and design. This is where API or service contracts come in.13

An API or service contract is a document that captures how the API is designed (structure, protocol, version, methods, etc.) and can be used for setting up agreements between service providers and service consumers.14 A common standard for creating API contracts is the OpenAPI Specification (formerly known as the Swagger Specification). The benefit of documenting these contracts in code is that both humans and systems can interpret the specifications. Service providers can also receive test procedures from the service consumers, so any changes can be tested to validate whether the service breaks.

# ## API Discoverability

Just like a catalog for data products, I recommend keeping track of services by using a service registry or developer portal. A service registry is a tool that stores all critical API information in a central repository. It promotes reuse of services and provides essential information about ownership, versioning, changes, usage, and so on.

# ## Microservices

Now that we’ve broken up our complex and tightly integrated service-oriented architecture by promoting the decoupling of services from domains, we’re ready for a new trend: microservices. We touched on this in Chapter 2, but it’s a useful pattern for breaking down applications into smaller, more manageable parts. Some people like to use the term loosely coupled services, which immediately shows you the overlap with SOA.

13 The Azure API Management DevOps Resource Kit is a great repository with examples of setting up API DevOps with Azure API Management.

14 Martin Fowler goes in depth about what advantages consumer-driven contracts offer.