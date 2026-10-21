Inter-Domain Routing (CIDR) notation. For example, the IPv4 address 192.168.0.1 will become 192.168.0.1/32.

If introspection is enabled, GraphQL IDE tools such as GraphiQL Explorer or GraphQL Playground will auto-complete directive names and their arguments as soon as you start typing the at symbol (@) on a field. The same is true for fields and their arguments.

Query directive arguments can be prone to injections too. Attackers can use them to influence the way the server returns the response to the specific field. For instance, a query directive might use the argument where, which then gets translated to a SQL-matching pattern (for example, the LIKE operator).

You can use the introspection query shown in Listing 8-2 to get only the available directives by using the __schema meta-field with the directives field.

```
query GetDirectives {
  __schema {
    directives {
      name
      description
      locations
    }
  }
}
```

Listing 8-2: A GraphQL introspection query used to list directives

# Operation Names

Operation names are strings we can add to GraphQL operations such as queries, mutations, or subscriptions. They are often used to uniquely name each query when multiple queries are sent together. GraphQL graphical IDEs such as GraphiQL Explorer and GraphQL Playground use the operation name as a way to allow clients to choose which operation to run when more than one query exists in a document through a drop-down menu, as shown in Figure 8-2.

![](bbox=[0.22, 0.68, 0.88, 0.9])

Figure 8-2: Executing a selected query based on its operation name in GraphiQL Explorer