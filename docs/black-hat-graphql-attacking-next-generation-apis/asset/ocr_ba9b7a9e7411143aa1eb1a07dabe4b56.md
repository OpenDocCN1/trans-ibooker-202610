```
# nmap --script=graphql-introspection -iL hosts.txt -sV -p 5013

PORT      STATE SERVICE VERSION
5013/tcp open  http    Ajenti http control panel
| graphql-introspection:
|   VULNERABLE:
|   GraphQL Server allows Introspection queries at endpoint:
|   Endpoint: /graphql is vulnerable to introspection queries!
|       State: VULNERABLE
|       Checks if GraphQL allows Introspection Queries.
|   
|   References:
|       https://graphql.org/learn/introspection/
```

Listing 4-19: A GraphQL introspection detection with the Nmap NSE

Using nmap to detect when introspection is enabled is just the first step. The next step is to extract all possible schema information by using a more robust query.

In the book’s GitHub repository, you can find a comprehensive introspection query that, when executed, will extract a lot of useful information about the target’s schema: https://github.com/dolevf/Black-Hat-GraphQL/blob/master/queries/introspection_query.txt. This query will return information such as queries, mutations, and subscriptions names, with the arguments they accept; names of objects and fields, along with their types; names and descriptions of GraphQL directives; and object relationships. If you run that query in Altair, the server should return a fairly large response, as shown in Figure 4-6.

![](bbox=[0.24, 0.55, 0.9, 0.88])

Figure 4-6: An introspection in Altair