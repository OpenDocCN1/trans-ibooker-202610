
rules that either permit or deny client access. Listing 7-3 shows queries and mutations protected by GraphQL Shield, which defines the permissions and roles required to access each query.

```
const permissions = shield({
  Query: {
    frontPage: not(isAuthenticated),
    fruits: and(isAuthenticated, or(isAdmin, isEditor)),
    customers: and(isAuthenticated, isAdmin),
  },
  Mutation: {
    addFruitToBasket: isAuthenticated,
  },
  Fruit: isAuthenticated,
  Customer: isAdmin,
})
```

Listing 7-3: A GraphQL Shield code example

Clients wishing to use the frontPage query don’t have to be authenticated, as defined by the rule not(isAuthenticated), whereas to use the customers query, they have to both be authenticated and have an admin user, as indicated by and(isAuthenticated, isAdmin). The and operator mandates that both conditions must be true for permission to be granted.

A developer community actively maintains GraphQL Shield and continuously improves it. As of this writing, the last documented vulnerability in GraphQL Shield was an authorization bypass that dates back to 2020 in versions earlier than 6.0.6.

When performing a code review, look for the GraphQL Shield component called the fallbackRule. This rule can determine whether a request should be allowed or denied by default whenever a rule is not defined. By default, fallbackRule is set to allow. To read more about GraphQL Shield rules, refer to the official documentation at https://www.graphql-shield.com/docs/rules#logic-rules.

## Schema Directives

GraphQL deployments might use custom schema-level directives to apply both authentication and authorization controls on certain operations and fields. By decorating schema components, these custom directives can control what clients can and can’t do in the API. We can use them to enforce security at the query level, type level, field level, and so on.

The graphql-directive-auth library (https://github.com/graphql-community/graphql-directive-auth) provides one example of how developers could apply directives to solve authentication and authorization gaps in their APIs. In some implementations, the @auth directive accepts a requires argument, which takes a string value representing the role or group a user needs in order to query the field. Clients usually send these user groups or roles through a JWT payload. The directive logic analyzes these to either allow or deny access to protected elements of the schema.

Authorization directives can have various other names or arguments. Table 7-1 is a list of common ones you might encounter in your introspection hunts.

## Table 7-1: Common GraphQL Authorization Directives

| Directive name | Argument name | Argument type |
|---|---|---|
| @auth | requires | String |
| @protect | role | String |
| @hasRole | role | String |

Some @auth directives might also use an argument called permissions, which accepts a list of scope grants.

## IP-Based Allow Listing

Some GraphQL APIs, particularly those deployed in internal systems that aren't public facing, may choose not to authenticate individual client requests. Instead, they might opt to use an allow list of source IP addresses to authorize clients. In this technique, the server checks the client IP address included in a network request against a list of addresses or network range (such as 10.0.0.0/24).

This IP address often gets passed to the API by a public-facing network device, such as a reverse proxy or a load balancer. Applications will then attempt to discover the IP address by looking for HTTP headers set on incoming requests. A few common HTTP headers for this purpose are X-Forwarded-For, X-Real-IP, X-Originating-IP, and X-Host.

Because clients can spoof these headers, reverse proxies may blindly forward misinformation to the application. For example, here is how you might pass a custom X-Forwarded-For header to DVGA with cURL:

```
# curl -X POST http://localhost:5013/graphql -d '{"query":"{ __typename }"}'
-H "Content-Type: application/json" -H "X-Forwarded-For: 10.0.0.1"
```

If the application allows only requests to the GraphQL API that originate from the network 10.0.0.0/24, injecting such a header at a later stage could allow an attacker to bypass the IP-based allow list and communicate with the application.

## Authentication Testing

When you're testing GraphQL authentication, you'll encounter certain operations that aren't protected by any authentication layer. For example, unauthenticated users might have access to queries, while only authenticated users might be able to perform more sensitive, state-changing actions using mutations. You might find this model in use on a blog: any client can read posts, whereas only authenticated users can write comments.

It’s important to do a thorough scan of the target GraphQL server and schema for any unprotected queries. This section will outline how you can detect and defeat certain GraphQL authentication controls.

## Detecting the Authentication Layer

One of the best ways to determine whether a target GraphQL application is protected by an authentication layer is by sending it a canary query. Use either of the introspection queries from Chapter 6 or craft your own to probe the schema for a range of operations, objects, and types. Depending on the response you receive, you may be able to detect the type of authentication used, as well as the layer at which authentication controls are implemented. In particular, keep an eye out for status codes, error messages, and differences in the responses to query variations.

## HTTP Status Codes

A sure way to verify that some sort of authentication layer exists on a GraphQL target is by analyzing the HTTP response you receive after sending a canary query. Most GraphQL implementations will always return a 200 OK status code, even when the query contains typos or errors. However, if you receive a 403 Forbidden Error, it’s possible that out-of-band authentication and authorization control, like a gateway or a WAF, has blocked your request from reaching the API in the first place.

## Error Messages

Error messages can obviously reveal the presence of authentication controls, but they might also tell us exactly what type of authentication the API requires and where in the architecture these checks occur. Table 7-2 shows a list of common in-band GraphQL authentication error messages and the authentication implementation known to raise the error message by default.

## Table 7-2: Common GraphQL Authentication Errors

| Error message | Possible authentication implementation |
|---|---|
| Authentication credentials are missing. Authorization header is required and must contain a value. | OAuth 2.0 Bearer with JSON Web Token |
| Not Authorised! | GraphQL Shield |
| Not logged in
Auth required
API key is required | GraphQL Modules |
| Invalid token!
Invalid role! | graphql-directive-auth |

Error messages can be customized and might differ from those shown here. Reference Chapter 6 for additional information on how to abuse errors to extract valuable information from GraphQL. For example, a combination

of a 200 OK status code and an error message could indicate that authentication is required. Because these details may vary from one GraphQL API to another, we recommend checking all avenues.

## Authentication-Related Fields

Another great way to detect an authentication layer is to use introspection queries to identify any authentication-related query or mutation operations. By design, in-band GraphQL authentication requires authentication, session management, and identity-based operations. For example, a client will most likely need to send unauthenticated mutation requests that perform login and sign-up operations to create and access their authenticated accounts. We can use the introspection query in Listing 7-4 to analyze the schema for any mutation operations related to authentication.

```
{ __schema { mutationType { name kind fields { name description } } } }
```

Listing 7-4: An introspection query used to identify all mutations

Check whether the query returns any mutation names similar to these:

- me
- login
- logout
- signup
- register
- createUser
- createAccount

If so, you can infer that the API has an authentication layer, which means you can begin testing its resiliency against password brute-force attacks.

## Brute-Forcing Passwords by Using Query Batching

A classic authentication attack, password brute-forcing works against systems that fail to implement rate limits or other automated account-takeover prevention controls. To perform one, an attacker sends many login requests to a system in an attempt to correctly guess a password. This programmatic attack usually accepts a dictionary of possible user credentials or iterates through a sequence of characters to generate possible credential combinations.
