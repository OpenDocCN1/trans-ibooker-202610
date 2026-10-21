As you can see, GraphQL queries are uniquely structured, and it is important to understand the various parts. Table 3-1 provides a description of each component.

# Table 3-1: The Components of a GraphQL Query

| # | Component | Description |
|---|---|---|
| 1 | Operation type | Type that defines the method of interaction with the server (query, mutation, or subscription) |
| 2 | Operation name | Arbitrary client-created label used to provide a unique name to an operation |
| 3 | Top-level field | Function that returns a single unit of information or object requested within an operation (may contain nested fields) |
| 4 | Argument (of a top-level field) | Parameter name used to send information to a field to tailor the behavior and results of that field |
| 5 | Value | Data related to an argument sent to a field |
| 6 | Field | Nested function that returns a single unit of information or object requested within an operation |
| 7 | Directive | Feature used to decorate fields to change their validation or execution behavior, altering a value returned by a GraphQL server |
| 8 | Argument (of a directive) | Parameter name used to send information to a field or object to tailor its behavior and results |
| 9 | Argument (of a field) | Parameter name used to send information to a field to tailor the behavior and results of the field |

The following sections explore these components, as well as a few additional GraphQL features, with a focus on how they contribute to GraphQL’s attack surface.

# Queries, Mutations, and Subscriptions

We discussed the root operation types query, mutation, and subscription in Chapter 1 and showed an example of using the query type to retrieve data. (For that reason, we won’t revisit the query type here.) As hackers, the real fun often happens when we can modify data. Creating, updating, and deleting data within a target platform empowers us to expose business logic flaws.

> NOTE We recommend using the GraphQL security lab we deployed in Chapter 2 to follow along with this chapter’s example requests. Use Altair to send the requests to DVGA (http://localhost:5013/graphql).