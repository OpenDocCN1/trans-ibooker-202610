Here, a GraphQL server has set the maximum cost allowed (MAX_COST) to 200. In this example, queries that have a cost of 200 and below are accepted, which means that if a client is sending multiple parallel queries, all with a cost of 200, all will be accepted. This might introduce risks if the application’s backend is not able or ready to sustain parallel queries with such cost. Imagine an attacker using the maximum cost allowed to send thousands of requests; if the limit is too forgiving, this could choke an application.

# Dynamically Assigning Cost to the Server Response

Cost analysis can also be performed on the server response to a query after it is fully resolved. The server must first process the query in order to understand its cost. However, looking at the actual resolved query can provide a more accurate cost estimation than the static technique.

The advantage of this dynamic method over the static method is that dynamic cost assignment takes into account the response complexity as it is returned by the server. Think of a client requesting a single field that results in the server returning an array containing 1,000 elements. In this case, the response indicates a level of complexity that cannot be inferred by just looking at the query.

# Using Credits-Based Rate Limiting

GraphQL servers can be designed to keep track of the cost of queries made throughout a client session’s lifetime. Tracking this information allows servers to set hourly or daily quota limits, and reject queries after a certain limit is exceeded, as part of a credit-based system. For example, a server may set an hourly credit allowance (such as 1,000) per user session or per source IP address. If a query had a static cost of 200, a client could make only five of these queries per hour. To query again, they would have to wait until the credit allowance quota renews.

For this mechanism to work, however, the server must track and store a client’s API usage data in a database. Otherwise, query limits based on cost would have to be stateless, which is common in GraphQL APIs.

# Discovering a Query’s Cost in Responses

As you’ve learned, there are a few ways to implement cost-analysis controls in GraphQL APIs. In some implementations, you may see cost-related metadata in the response to a query. Consider the following GraphQL response example, which uses the extensions response field to provide cost-related information to the client:

```
{
  "data": {
  --snip--
  },
  "extensions": {
    "cost": {
      "credits_total": 1000,

```