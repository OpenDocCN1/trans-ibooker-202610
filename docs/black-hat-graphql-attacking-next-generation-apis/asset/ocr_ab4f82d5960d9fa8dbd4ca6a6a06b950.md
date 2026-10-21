as over-fetching (returning data to the client that isn’t used) or under-fetching (returning too little data, forcing the client to make a second request). You’ll learn more about these differences and why they matter for performance in the next section.

GraphQL has additional useful features, such as schema stitching and schema federation. Schema stitching is a way to create a single GraphQL schema from multiple underlying GraphQL services, allowing GraphQL to be used as a unified gateway. Essentially, it packages (stitches) multiple schemas into one big schema, creating a single integration point for clients. Because multiple microservices can define their own GraphQL schemas and have their own GraphQL endpoints, allowing a single GraphQL API gateway to consolidate many schemas into one can make it easier for clients to integrate with an application.

Schema federation is similar to schema stitching, except it doesn’t require you to manually stitch schemas together. Instead, schema federation lets you tell the GraphQL API gateway where to look for additional schemas. The gateway then does the stitching automatically. Federation is a lower-maintenance approach for consolidating multiple APIs into a single gateway.

Complex API applications, such as ones that require schema federation or schema stitching, may introduce security vulnerabilities, potentially allowing hackers to access data to which they shouldn’t otherwise have access. In general, the more complex an application is, the higher the chance that its internal complexities could lead to vulnerabilities.

# GraphQL APIs vs. REST APIs

In the previous sections, we discussed the challenges of traditional APIs that GraphQL attempts to solve. For example, REST APIs often provide more data than the client needs (over-fetching) or too little data (under-fetching), forcing the client to make additional API requests. In this section, we’ll walk through an example to demonstrate how an application fronted by a REST API compares to one that uses GraphQL.

Consider Table 1-1, a database table with information about an application’s user base. A simple web application might display this information as part of an admin panel that lets the systems administrator list all available accounts and get their state. We’ll call this the Users Administration page.

Table 1-1: Users Database Table

| User ID | Username | Email | First name | Last name | State |
|---|---|---|---|---|---|
| 1 | dsmith | david@example.com | David | Smith | Disabled |
| 2 | clarry | chris@example.com | Chris | Larry | Enabled |

In the following sections, we’ll describe the API requests a client would have to make to retrieve user data if the application were using a REST API, and how it might do the same in an application using GraphQL.