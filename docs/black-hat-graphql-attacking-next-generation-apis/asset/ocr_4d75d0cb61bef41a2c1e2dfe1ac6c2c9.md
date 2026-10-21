# GraphQL Threat Matrix

| Implementation | Validations | Field Suggestions | Query Depth limit | Query Cost Analysis | Automatic Persisted Queries | Introspection | Debug Mode | Batch Requests |
|---|---|---|---|---|---|---|---|---|
| wp-graphql | 38 | ☑ | ⚠ | ✖ | ✖ | ⚠ | ⚠ | ☑ |
| graphql-php | 37 | ☑ | ⚠ | ⚠ | ✖ | ☑ | ⚠ | ⚠ |
| Apollo | 34 | ☑ | ⚠ | ⚠ | ✖ | ☑ | ⚠ | ☑ |
| graphql-yoga | 34 | ☑ | ⚠ | ✖ | ✖ | ⚠ | ⚠ | ⚠ |
| graphene | 34 | ☑ | ✖ | ✖ | ✖ | ☑ | ✖ | ⚠ |
| Ariadne | 34 | ☑ | ⚠ | ⚠ | ✖ | ☑ | ⚠ | ✖ |
| Strawberry | 34 | ☑ | ⚠ | ✖ | ✖ | ☑ | ✖ | ✖ |
| graphql-ruby | 28 | ☑ | ✖ | ⚠ | ⚠ | ☑ | ✖ | ☑ |
| Sangria | 27 | ☑ | ⚠ | ⚠ | ✖ | ☑ | ✖ | ⚠ |
| Tartiflette | 26 | ✖ | ✖ | ✖ | ✖ | ☑ | ✖ | ✖ |
| graphql-java | 26 | ☑ | ⚠ | ⚠ | ✖ | ☑ | ✖ | ⚠ |
| gqlgen | 25 | ☑ | ✖ | ⚠ | ⚠ | ☑ | ⚠ | ⚠ |
| Dgraph | 25 | ☑ | ✖ | ✖ | ⚠ | ☑ | ✖ | ✖ |
| graphql-go | 24 | ☑ | ✖ | ✖ | ✖ | ☑ | ⚠ | ✖ |
| juniper | 24 | ✖ | ✖ | ✖ | ✖ | ☑ | ✖ | ⚠ |
| Diana.jl | 10 | ☑ | ✖ | ✖ | ✖ | ☑ | ✖ | ✖ |
| gql-dart/gql | 9 | ☑ | ✖ | ✖ | ✖ | ☑ | ✖ | ✖ |
| Agoo | 0 | ✖ | ✖ | ✖ | ✖ | ☑ | ⚠ | ✖ |

Figure 3-4: The GraphQL Threat Matrix

Once successfully validated, a GraphQL request is executed by the server. Resolver functions, which we covered in Chapter 1, are responsible for returning a response for a requested field.

# Common Weaknesses

In this section, we will provide a high-level overview of the common weaknesses found in GraphQL. In later chapters, we will perform penetration testing against each vulnerability class, as well as review related exploit code.

# Specification Rule and Implementation Weaknesses

GraphQL's specification defines rules, design principles, and standard practices. If you ever want to develop your own GraphQL implementation, this is the document your implementation should conform to, including the way it formats its responses, validates arguments, and so on.

Here are two examples of rules taken from the GraphQL specification:

+   - Arguments may be provided in any syntactic order and maintain identical semantic meaning.
+   - The data entry in the response will be the result of the execution of the requested operation.