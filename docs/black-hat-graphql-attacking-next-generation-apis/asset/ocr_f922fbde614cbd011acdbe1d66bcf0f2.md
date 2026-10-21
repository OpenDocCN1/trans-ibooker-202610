You may have noticed that we’re passing a specific port to Nmap (-p)—namely, port 5013. Like any web server, GraphQL servers could run on any port, but a few are quite common, such as 80–89, 443, 4000–4005, 8443, 8000, and 8080. We recommend scanning both common and uncommon port ranges when possible.

# The __typename Field

So far, we’ve known exactly which fields to ask for in our queries, such as pastes with a selection set of id, as we requested earlier. You might be wondering, what if we don’t know what fields exist on the GraphQL API? How can we go about identifying GraphQL without this information? Luckily, there is a quick way to query GraphQL and return a valid response without knowing anything about the application’s schema.

Meta-fields are built-in fields that GraphQL APIs expose to clients. One example is __schema (part of introspection in GraphQL). Another example of a meta-field is __typename. When used, it returns the name of the object type being queried. Listing 4-4 shows a query that uses this meta-field.

```
query {
  pastes {
    __typename
  }
}
```

Listing 4-4: A GraphQL query with the __typename meta-field

When you run this query with Altair, the response will be the name of the pastes object type:

```
"data": {
  "pastes": [
    {
      "__typename": "PasteObject"
    }
  ]
}
```

As you can see, GraphQL tells us that the pastes object’s type name is PasteObject. The real hack here is that the __typename meta-field can be used against the query root type as well, as shown in Listing 4-5.

```
query {
  __typename
}
```

Listing 4-5: A GraphQL meta-field used with the query root type

This query uses __typename to describe the query root type and will work against pretty much any GraphQL implementation, since __typename is part of the official specification.