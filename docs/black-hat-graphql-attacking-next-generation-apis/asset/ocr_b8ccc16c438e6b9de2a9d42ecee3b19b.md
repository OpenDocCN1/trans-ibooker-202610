```
directive @cost(
  complexity: Int = 1
) on FIELD_DEFINITION | OBJECT

type PasteObject {
  title: String @cost(complexity: 1)
  content: String @cost(complexity: 1)
  userAgent: String @cost(complexity: 5)
  ipAddr: String @cost(complexity: 5)
}
```

Here, a special cost directive accepts a complexity argument, and the complexity argument accepts an integer value. If no value is provided to the complexity argument, it defaults to 1. In the schema, the fields in PasteObject have some cost values assigned to them based on how resource heavy they are to resolve. (Imagine a field that requires the server to perform upstream checks against multiple third-party services, as opposed to a field that can be resolved by reading directly from a local database.)

Based on this schema definition, we can add directives to our query as follows:

```
query {
  pastes {
    title    # cost: 1
    content  # cost: 1
    userAgent # cost: 5
    ipAddr   # cost: 5
  }
}
```

The total cost of this query is 12. Knowing the total cost allows the GraphQL server to decide whether it should accept the query or reject it because it’s deemed too expensive.

NOTE
*Some GraphQL implementations automatically assign a field a value of 1 if no cost value is explicitly set.*

Many static cost assignment libraries don’t persist the cost information to any database or cache. Therefore, in practice, each query is evaluated per query. To illustrate the dangers of failing to track cost usage, consider the diagram in Figure 5-6.

![](bbox=[0.22, 0.75, 0.9, 0.92])

Figure 5-6: The dangers of stateless cost analysis