
As the name indicates, safe-circular-query.graphql is the safer version of a circular query. In this query, we ask for the owners’ names of all pastes on the application, except we’re doing so in circles, which exponentially increases the number of objects the GraphQL server has to load. Paste the query into Altair and run it against the server to prove the concept of circular queries.

> NOTE If you feel adventurous and are curious to see the impact that a more complex circular query would have on our vulnerable target, you can find the unsafe version of the query at https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch05/unsafe-circular-query.graphql. This query could crash the DVGA instance and possibly the hypervisor, so use it with caution.

## Circular Introspection Vulnerabilities

A circular relationship exists in GraphQL’s built-in introspection system. Therefore, when introspection is enabled, you could potentially have access to a circular query right out of the gate.

The introspection system has its own schema, defined in the official GraphQL specification document. Here is an excerpt of it:

```
type __Schema {
  --snip--
  types: ① [__Type!]!
  queryType: __Type!
  mutationType: __Type
  subscriptionType: __Type
  directives: [__Directive!]!
  --snip--
}
```

```
type ② __Type {
  --snip--
  name: String
  description: String
  fields(includeDeprecated: Boolean = false): ③ [__Field!]
  --snip--
}
```

```
type ④ __Field {
  --snip--
  name: String!
  description: String
  args: [__InputValue!]!
  type: ⑤ __Type!
  isDeprecated: Boolean!
  --snip--
}
```

At ①, the types field is defined for the __Schema object type. You can see that types is set to [__Type!], which means that it’s using the __Type object defined at ②. The square brackets and exclamation point mean that the types field will return a non-nullable array of __Type objects.

The __Type object has a fields field, set at ③, of type [__Field!]. This will return a non-nullable array containing __Field objects. At ④, the __Field type is defined. This type has a field named type at ⑤ referencing the __Type object. As you can see, we have a circular relationship between __Type.fields and __Field.type.

> NOTE
> The full introspection system schema can be found in the GraphQL specification document at https://spec.graphql.org/October2021/#sec-Schema-Introspection.Schema-Introspection-Schema.

You can easily test this circular relationship by running the following query with Altair:

```
query {
  __schema {
    types {
      fields {
        type {
          fields {
            type {
              fields {
                name
              }
            }
          }
        }
      }
    }
  }
}
```

Such circular queries can be fairly easily exploited. While a single query may not be able to take down a server, a series of complex queries could have the potential to impact it.

## Circular Fragment Vulnerabilities

GraphQL operations can share logic through the use of fragments, as explained in Chapter 3. Fragments are defined by the client, and as such, clients can build any logic they desire into them. That said, the GraphQL specification documentation contains rules about how fragments should be implemented, including this one:

> The graph of fragment spreads must not form any cycles including spreading itself. Otherwise, an operation could infinitely spread or infinitely execute on cycles in the underlying data.

Let’s explore how fragments can be constructed to form a cycle and lead to a DoS. In DVGA, run the following query, which uses a fragment named Start on the object PasteObject. The pastes field utilizes this fragment using the ...Start syntax:

```
query {
  pastes {
    ...Start
  }
}

fragment Start on PasteObject {
  title
  content
}
```

When the query is executed, it returns the field and content fields of pastes:

```
"pastes": [
  {
    "title": "My Title",
    "content": "My First Paste"
  }
]
```

Now, what if we add another fragment named End that uses the Start fragment, and modify the Start fragment to use the End fragment? An interesting condition will occur here:

```
query CircularFragment {
  pastes {
    ...Start
  }
}

fragment Start on PasteObject {
  title
  content
  ...End
}

fragment End on PasteObject {
  ...Start
}
```

This condition leads to an infinite execution, just as the GraphQL specification suggests. Try experimenting with this query in the lab.

NOTE Executing this query will immediately crash DVGA with a segmentation fault error. Make sure to execute it only in the virtual lab. After you run the query, DVGA should no longer be available. To start it again, simply follow the steps from Chapter 2 to create another Docker container.

If you ran the query, you should have seen a pretty immediate crash! You might be wondering, are all GraphQL servers vulnerable to this attack? The short answer is no, if the GraphQL server is spec compliant. A GraphQL server is supposed to reject such queries before they get executed. Still, you never know when you might run into a completely custom implementation in a penetration test, so knowing about this technique is worthwhile.

## Field Duplication

Field duplication vulnerabilities concern queries that contain repeating fields. They are simple to execute, yet less effective than circular queries.

While circular queries are small queries that result in abnormally large responses, field duplications are large queries that exhaust the server because of the amount of time they take to process and resolve. To effectively abuse GraphQL APIs by using field duplications, you must send a constant stream of queries to keep the server’s resources continuously busy.

## Understanding How Field Duplication Works

To understand how field duplication works, consider the following GraphQL query:

```
query {
  pastes {
    title
    content
  }
}
```

This query returns the title and content fields of all pastes in the application. When GraphQL receives this query, it will use its query resolvers to provide each field requested.

If we “stuff” additional fields in the query, GraphQL will be forced to resolve each field separately. This behavior could introduce additional load on the server, cause performance degradation, or completely crash it.

The strategy here is fairly simple: choose a field that you think might be expensive to resolve, and stuff the query with additional copies of that field’s name. Listing 5-5 shows an example query.

```
query {
  pastes {
    title
    content
    content
    content
    content
    content
  }
}
```

Listing 5-5: A GraphQL query with repeating fields

When a query contains multiple repeating fields, such as in Listing 5-5, where content is repeated five times, you might expect to see the same five fields in the response. In reality, GraphQL will consolidate the response and display only a single content JSON field:

```
{
  "data": {
    "pastes": [
      {
        "title": "My Title",
        "content": "My First Paste"
      }
    ]
  }
}
```

From a client perspective, it might seem like GraphQL is ignoring our repeating fields. Fortunately, this is not the case. Through response time analysis, you can see the query’s impact on the server. Unless the server has implemented specific security defenses, such as query cost analysis (covered later in this chapter), you should expect to see these vulnerabilities in most GraphQL implementations.

## Testing for Field Duplication Vulnerabilities

To test field duplication attacks in our lab, we’ll write a simple query and attempt to repeat a few selected fields to see how our target responds.

Open Altair and ensure that the address bar is set to http://localhost:5013/graphql. In the left pane, enter the following query, which will serve as a baseline:

```
query {
  pastes {
    content
  }
}
```

Click Send to query GraphQL. In the response section, you’ll notice that Altair provides the total time it took for the server to respond in milliseconds, as shown in Figure 5-3.

It took 26 milliseconds for DVGA to respond to the query, which is a normal response time. The time you might see in your lab could differ but should be in the same ballpark.
