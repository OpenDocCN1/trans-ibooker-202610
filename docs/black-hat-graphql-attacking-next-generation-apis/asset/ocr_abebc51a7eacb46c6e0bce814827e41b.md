These are some of the most popular implementations in use today, as well as more niche implementations, such as Sangria for Scala, Juniper for Rust, and HyperGraphQL for Java. Later in this chapter, we will discuss how to distinguish between them during a penetration test.

Detection of GraphQL APIs can be done in several ways: either manually, which is typically harder to scale if you have more than a few hosts on a network, or automatically, using various web scanners. The advantage of using web-scanning tools is that they are scalable. They are threaded, and often have the ability to read external files as program input, such as text files with a list of hostnames to scan. These tools already have the logic to detect web interfaces built into them, and using scripting languages (such as Bash or Python), you can programmatically run them against hundreds of IP addresses or subdomains. In this chapter, we will use popular scanners such as Nmap, as well as GraphQL-oriented scanning tools, such as Graphw00f, for reconnaissance.

# Common Endpoints

In Chapter 1, we highlighted some of the differences between REST and GraphQL APIs. One of these differences, relevant to the reconnaissance phase, is that a GraphQL API endpoint is typically static, and most commonly /graphql.

However, although /graphql is often the default GraphQL endpoint, the GraphQL implementation can be reconfigured to use a completely different path. In those cases, what can we do to detect it? One way is to manually attempt a few common alternative paths to the GraphQL API, such as versioned endpoints:

+   /v1/graphql
+   /v2/graphql
+   /v3/graphql

You’ll typically see these versioned API endpoints when the application needs to support multiple versions of its API, either for backward compatibility or for the introduction of a new feature in a way that doesn’t conflict with the stable API version that customers might still be using.

Another way to find a GraphQL implementation is through IDEs, such as GraphQL Playground or GraphiQL Explorer, which we used in Chapter 1 to experiment with GraphQL queries. When either of these interfaces is enabled, it often uses an additional, dedicated endpoint. This means GraphQL can potentially exist under the following endpoints as well:

+   /graphiql
+   /playground

If these endpoints happen to also be versioned, they may have a version number prepended to their path, such as /v1/graphiql, /v2/graphiql, /v1/playground, /v2/playground, and so on.