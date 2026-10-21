![](bbox=[0.24, 0.06, 0.9, 0.29])

Figure 5-3: The Altair response time indicator

Next, copy the query from https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch05/field-duplication.graphql, paste it into Altair, and run it. This query contains approximately 1,000 content fields. Figure 5-4 shows that processing this query took 958 milliseconds, which is 36 times slower!

![](bbox=[0.24, 0.42, 0.9, 0.7])

Figure 5-4: A slower response time to a query with repeating fields

Some fields will require more resources to resolve, so the performance impact might vary depending on the chosen field.

This attack requires the client to continuously send large payloads. Attempting to manually exploit field duplication can be cumbersome. As an alternative method, you can use a special Python exploit that attempts to perform a field duplication attack at a far larger scale. Listing 5-6 shows