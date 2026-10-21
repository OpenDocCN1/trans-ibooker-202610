Operation names are also used for other purposes, such as debugging and logging. In fact, they are interesting potential injection vectors because applications can use them in many ways. For instance, some applications use operation names for analytics, to determine which queries clients use the most. The operation name string could end up in different systems, such as logging systems, relational databases, cache databases, and so on. It is important to check whether the GraphQL API allows special characters as part of the operation name, as this could turn out to be an injectable interface.

Operation names are typically alphanumeric, but some GraphQL server implementations are more permissive than others when it comes to the type of characters they permit.

# Input Entry Points

When attempting to perform injection testing against GraphQL APIs, we need to find a way to discover input entry points. If we are lucky and introspection is left enabled, we can often quickly access the various queries, mutations, and subscriptions that the API supports, along with information about its types, fields, arguments, and so on, using a GraphQL IDE tool like Altair, GraphiQL Explorer, or GraphQL Playground.

To view this information about DVGA in Altair, set the URL to http://localhost:5013/graphql and click the Save button (the diskette icon) located at the top right. Click the Refresh button located next to the Save button, and then click Docs. You should see a section for queries, mutations, and subscriptions. Click any of them to see the types of arguments that exist within each, as shown in the screenshot in Figure 8-3.

![](bbox=[0.23, 0.54, 0.73, 0.825])

Figure 8-3: Schema documentation in the Altair client