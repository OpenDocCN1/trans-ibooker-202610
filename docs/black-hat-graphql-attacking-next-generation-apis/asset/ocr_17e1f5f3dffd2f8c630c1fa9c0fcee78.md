By default, GraphQL servers won’t limit the number of aliases that can be used in a single request. The GraphQL application maintainer could implement custom protections, such as counting the aliases and restricting them in some middleware, but since aliases are part of the specification, it’s uncommon to remove support for them or limit their functionality.

# Abusing Aliases for Denial of Service

When performing a penetration test, you may run into queries that seem to take the server longer to process than others. If you identify such a query, you can hog system resources by calling the same query over and over again. If the server struggles to quickly return a response, flooding the server with the same query could result in system overload.

In DVGA, one specific query is slower than others: systemUpdate. This query is designed to simulate long-running commands, such as those that perform system updates. Unauthorized clients should never be allowed to perform queries that change system state, but nothing is impossible in real-life penetration testing scenarios! Let’s run the systemUpdate query in Altair to see how long this command takes to fully process. It takes no arguments, as shown here:

```
query {
  systemUpdate
}
```

Send this query to the server and observe the time it takes for the server to return a response (Figure 5-5).

![](bbox=[0.23, 0.54, 0.64, 0.68])

Figure 5-5: The systemUpdate query response time

The systemUpdate took 50,361 milliseconds to complete. That’s around 50 seconds, a significant amount of time by today’s web standards. This is an example of a query that we might be able to leverage for DoS purposes.

> NOTE
> In your lab, the response time to systemUpdate could be faster. DVGA randomizes its behavior when this query is used to simulate system load behavior.

Using GraphQL aliases, we can attempt to run systemUpdate a few times to see how the server behaves. Listing 5-7 shows how you can run systemUpdate more than once, using aliases.