```
type Location {
  latitude: Int
  longitude: Int
}

type Query {
  users: [User]
}

schema {
  query: Query
}
```

Listing 1-3: The full schema, with an entry point to querying the User type

By querying the users field in the Query type, clients can access the User object type we defined. The square brackets [] surrounding the User object type indicate that this query will return an array of User objects. We'll discuss this syntax in Chapter 3.

NOTE
Notice that, while field names (like users) are lowercase, object names (like User) begin with an uppercase letter. This is the most common naming convention in GraphQL schemas.

Listing 1-4 is an example query that a client might send to a GraphQL server implementing the schema in Listing 1-3.

```
query {
  users {
    username
    email
  }
}
```

Listing 1-4: A GraphQL query

As you can see, GraphQL queries are pretty easy to read: all this query does is get the username and email of all users of the application. We define the query by using the query root operation. Then we request users as the query's top-level field, specifying the username and email fields we want. Because this query only reads information and doesn't change any data, we perform a query operation rather than a mutation.

Notice that blank spaces are used to separate components like names and values. The number of spaces used doesn't matter; whether there's a single space or multiple spaces, the query will remain the same and return consistent results.

# The Query Parser and Resolver Functions

Now, what happens when a GraphQL server receives a query? Well, it makes use of a query parser to read and extract the information it needs to execute