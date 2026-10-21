```
"credits_remaining": 990,
}
}
}
```

The extensions field is used to return some metadata to the client. This metadata is often related to query tracing, query cost calculation, or other debugging information. In this example, credits_total is the total number of available credits, and credits_remaining is the current number of credits left.

You may be asking yourself why a GraphQL server would share this information with the client in the first place. Clients can use it to determine when queries may start getting throttled by the server and potentially fail. This helps clients build better error-handling logic.

Of course, the availability of cost information is also valuable to hackers. If we have a way to know when our queries will be accepted by the server (as in the case of hourly credits), we have a way to determine when to launch a new attack the next time credits become available to us, instead of repeatedly sending requests that will get blocked.

# Query Depth Limits

Earlier in this chapter, we discussed circular queries and how recursive queries in GraphQL could starve a server for resources. To protect a GraphQL server from recursive queries, applications can set query depth limits. For example, setting a max_depth configuration to a value of 10 would allow up to only 10 levels of depth. Any query that exceeds the allowed depth would get rejected.

Some of the more mature GraphQL implementations support depth analysis out of the box or by leveraging external libraries written specifically for this purpose. Let’s take a look at how to implement query depth limits in graphql-ruby (Ruby) and Graphene (Python).

In graphql-ruby, it is possible to set a maximum depth limit within the MySchema class:

```
class MySchema < GraphQL::Schema
  --snip--
  max_depth 10
end
```

In Graphene, a maximum depth limit can be set in the following way:

```
schema = Schema(query=MyQuery)

validation_errors = validate(
    schema=schema.graphql_schema,
    document_ast=parse('THE QUERY'),
    rules=(
        depth_limit_validator(
            max_depth=20
        ),
    )
)
```