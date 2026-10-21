Building bug-free software is hard (and likely impossible to avoid completely). The more you know about an application and the deeper you dig into it, the higher your chances of finding a vulnerability.

# Denial of Service

One of the most prevalent vulnerability classes in GraphQL is DoS-based vulnerabilities. These vulnerabilities can degrade a targeted system’s performance or exhaust its resources completely, making it unable to fulfill client queries or even crash. In this chapter, we hinted at how field and object relationships, aliases, directives, and fragments could all potentially be used as attack vectors against a GraphQL service, because these capabilities provide API clients with an enormous amount of control over the query structure and execution behavior.

In Chapter 5, you’ll learn how this power can also enable clients to construct very complex queries that effectively degrade a GraphQL server’s performance if the right security countermeasures are not put in place. We will review four ways that a client can create expensive queries. These may overwhelm the GraphQL server and lead to DoS conditions.

# Information Disclosure

A common weakness in many web applications is the unintended disclosure of data to the public, or to a group of users that isn’t authorized to access it. Information leakages have many causes, and systems entrusted with protecting sensitive information such as PII should deploy numerous layers of detection and prevention controls to protect information from being exposed.

When it comes to GraphQL, hackers can fingerprint and collect data from its API in several ways. In Chapter 6, we’ll teach you how to leverage introspection queries to hunt for fields that may contain sensitive information. We’ll equip you with tools that take advantage of how field suggestions and error messages work to help uncover hidden data models and maneuver around GraphQL environments where introspection may be disabled.

# Authentication and Authorization Flaws

Authentication and authorization are complex security controls in any API system architecture. The fact that the GraphQL spec refrains from providing authentication and authorization guidance for implementations doesn’t help. This void often leads engineers to implement their own authentication and authorization mechanisms based on open source, in-house, or third-party solutions.

Most of the authentication and authorization vulnerabilities you’ll find in GraphQL stem from the same issues you’d find in traditional APIs, such as failure to adequately protect against brute-force attacks, logic flaws, or poor coding that allows controls to be entirely bypassed. In Chapter 7, we’ll review several common GraphQL authentication and authorization strategies and teach you how to defeat them with aliases, batch queries, and good, old-fashioned logic flaws.