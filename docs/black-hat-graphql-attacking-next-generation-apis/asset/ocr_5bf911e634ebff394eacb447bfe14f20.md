You can use online regex testing websites such as https://regex101.com to see how a particular expression behaves in practice, as shown in Figure 10-1.

![](bbox=[0.23, 0.13, 0.88, 0.32])

Figure 10-1: The online regular expression tester at https://regex101.com

In October 2020, an ethical hacker who goes by the handle of mvm reported a ReDoS vulnerability in a GraphQL API to CS Money’s bug bounty program (https://hackerone.com/reports/1000567). The hacker found that the GraphQL search object takes a q (query) argument. In their testing, they inserted a Unicode null value (\u0000) as its value:

```
query {
  search(q: "\u0000", lang: "en") {
    --snip--
  }
}
```

In response to this query, the GraphQL API server returned an interesting error that revealed some information critical to identifying the existence of the ReDoS vulnerability:

```
"errors": [
  {
    "message": "value (?.*\u0000) must not contain null bytes"
    --snip--
  }
]
```

As you can see, the string supplied through the q argument was inserted into regex-matching logic on the server, indicated by the preceding (?=.* string in the response. The server might use this argument to search for relevant data in a database.

Conveniently, the server had query tracing enabled through its extensions. Query tracing allows GraphQL servers to return response metadata useful for debugging and provides information about the query’s performance. The tracing information in the response disclosed three