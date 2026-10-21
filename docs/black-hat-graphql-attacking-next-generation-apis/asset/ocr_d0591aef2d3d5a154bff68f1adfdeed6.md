In tools such as Altair or GraphiQL Explorer, Listing 3-18’s JSON will be defined in the Variables pane of the client.

Input types provide clients with a possible way to defeat type validations, which may or may not have broken validation logic. For example, earlier in this chapter we discussed how custom scalar types could fail to validate values sent by clients, such as IP addresses or email addresses. Validation issues related to email addresses could allow attackers to bypass registration forms and login processes or perform injections.

# Introspection

After reviewing GraphQL’s language and type system, you should have noticed stark differences in what GraphQL APIs and REST APIs can offer clients. GraphQL puts a lot of power in the hands of the client by default. But wait, there’s more!

Arguably one of GraphQL’s most powerful features is introspection, the built-in tool that empowers clients to discover actions they can take using a GraphQL API. Introspection lets clients query a GraphQL server for information about its underlying schema, which includes data like queries, mutations, subscriptions, directives, types, fields, and more. As hackers, this feature can be a gold mine in supporting our reconnaissance, profiling, data collection, and attack-vector analysis efforts. Let’s dive into how we can use it.

The GraphQL introspection system has seven introspection types that we can use to query the schema. Table 3-3 lists these introspection types.

# Table 3-3: The Introspection System Types

| Introspection type | Usage |
|---|---|
| __Schema | Provides all information about the schema of a GraphQL service |
| __Type | Provides all information about a type |
| __TypeKind | Provides the different kinds of types (scalars, objects, interface, union, enum, and so on) |
| __Field | Provides all information for each field of an object or interface type |
| __InputValue | Provides field and directive argument information |
| __EnumValue | Provides one of the possible values of an enum |
| __Directive | Provides all information on both custom and built-in directives |

Consider Listing 3-19, which uses the __Schema introspection type against DVGA.

```
query {
  __schema {
    types {

```