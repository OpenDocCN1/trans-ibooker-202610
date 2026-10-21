Because these three fields sit at the root level of the shorthand query, they are also known as *top-level fields*. Fields may also contain their own selection set, allowing for the representation of complex data relationships. In the following example, the top-level owner field has its own selection set with one nested field:

```
{
  id
  title
  content
  owner {
    name
  }
}
```

So, selection sets are made up of fields, and fields can have their own selection sets with their own fields. Do any security issues jump out at you? In Chapter 5, we’ll explore how circular field relationships may result in recursive and expensive requests that can degrade performance and potentially crash a GraphQL server.

Fields are very important when it comes to interacting with GraphQL services. Not knowing what fields are available can be pretty limiting. Luckily, implementations have deployed a handy tool for us, known as *field suggestions*. When a client misspells a field, the error message returned by a server that implements field suggestions will reference the field it believes the client was trying to call. For example, if we sent a query for a paste with the field name of titl in DVGA (notice the typo), the server will respond with suggested alternatives:

```
"Cannot query field \"titl\" on type \"PasteObject\". Did you mean \"title\"?"
```

This field suggestion feature makes GraphQL a convenient, friendly, and simple tool not only for API consumers but also for hackers. We can exploit this feature to find fields we may not have known about otherwise. We’ll discuss this information disclosure technique in Chapter 6.

# Arguments

Like REST APIs, GraphQL allows clients to send *arguments* for various fields in their queries to tailor the results they return. If you take another look at Figure 3-1, you’ll notice that arguments can be implemented at various levels—namely, in fields and directives.

In the following query, the users field has an id argument with a value of 1. Without the id argument, this query would return the entire list of users in DVGA. The argument filters this list to those with the same identifier:

```
query {
  users(id: 1) {
    id
```