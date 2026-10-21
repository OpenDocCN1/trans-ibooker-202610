

The stack trace returns a wealth of information that we can use to uncover vulnerabilities, such as dependencies, software versions, software frameworks, and source code snippets. This stack trace also provides us with information such as user account, filesystem, and operating system details.

In DVGA, stack tracing is enabled only on the /graphql endpoint that GraphiQL Explorer uses to send queries to. This is to show you that GraphQL endpoints could have different configurations, so you want to test both if there is more than one.

## Leaking Data by Using GET-Based Queries

As we mentioned in Chapter 1, some GraphQL implementations allow clients to execute queries using the GET method, while others allow only POST requests. Mutation operations in particular should be sent using only POST methods. However, some implementations, like Scala-based Sangria, may allow GET requests for mutation operations as well.

Because GET requests transmit data as query parameters in the URL, they risk exposing sensitive information. For example, the following URL sends a GET request to DVGA. We pass a phone number in the variables GET parameter:

```
http://localhost:5013/graphql?query=query($phone: String)
{ paste(title: $phone) { id title } }&variables={"phone":"555-555-1337"}
```

The same query can also be sent in the following manner, by omitting the variables parameter and inserting the phone number directly into the query:

```
http://localhost:5013/graphql?query=query{ paste(title: "555-555-1337") { id title } }
```

In real applications, phone numbers are considered PII. These URLs will show up in the web server access logs of GraphQL servers (such as Apache or Nginx). Any sensitive information they contain may be logged in various locations, such as in referrer headers and any forward or reverse proxies between the requesting client and the server.

While this condition doesn’t directly give us information we don’t already have, it’s important to highlight such cases to clients in your penetration tests as something to be wary of.

## Summary

In this chapter, we explored how to extract valuable information from our targets by using a variety of tools and techniques. When introspection is enabled, you can use InQL to automatically extract the schema from GraphQL targets. When introspection is disabled, you can exploit a built-in

GraphQL feature known as field suggestions and “stuff” fields by using a tool called Clairvoyance.

You learned how to identify and bypass poor attempts at disabling introspection by using unblocked introspection meta-field queries. You also learned to uncover system details by using verbose GraphQL error and debug messages.

With all these GraphQL information disclosure tools and techniques, you should feel confident about your ability to extract application secrets, user details, PII, and system information that will propel your future GraphQL attacks.

## 7

## AUTHENTICATION AND AUTHORIZATION BYPASSES

![](img/2f244b00cd9f8f37cfc22f928aaa471a_188_0.png)

Out of the box, GraphQL has no authentication or authorization controls. As a result, the ecosystem has created its own or adopted those seen in traditional systems. In this chapter, we’ll cover the common GraphQL authentication and authorization implementations. Then we’ll discuss attacks that target some of their weaknesses.

*Authentication* is the mechanism by which a client proves their identity to a server. It answers the question: Is the user really who they say they are? Authentication attacks target a client’s identity, attempting to either steal credentials or spoof them to authenticate with a server, take certain actions on their behalf, or steal data to which they have access.

*Authorization* controls are responsible for granting access to data and ensuring that the actions an entity takes, whether they’re a human or a machine, match their assigned roles, groups, and permissions. Authorization attacks attempt to either bypass a security control entirely or poke holes in

it, allowing an attacker to take actions that wouldn’t otherwise be possible. For example, they might gain unauthorized access to system data or perform privileged actions, such as setting another user’s password.

Authentication and authorization controls can be challenging to implement. This is especially true when an application creates its own mechanisms from scratch instead of using the many battle-tested frameworks available for specific programming languages. Performing security testing of such controls is also a nontrivial task; security tools (such as API application scanners) struggle to identify authorization and authentication issues. One of the primary reasons is that scanners have no contextual understanding of the application’s business logic.

For years, hackers have defeated both authentication and authorization defenses by taking advantage of weak passwords, default credentials, forged tokens, flawed account recovery processes, replay attacks, and poor rate-limit controls. Not only is exploiting these weaknesses possible in GraphQL implementations, but, in many cases, GraphQL’s client-empowering features actually enable hackers to optimize their attacks, as you’ll soon learn.

## The State of Authentication and Authorization in GraphQL

The GraphQL spec has left implementers to fend for themselves when it comes to authentication and authorization. This lack of a detailed standard has led developers to select and deploy their own GraphQL authentication and authorization controls from a variety of libraries, tools, and configurations, often leading to vulnerabilities and implementation gaps.

In this section, we’ll sink our teeth into the ecosystem-driven authentication and authorization services, libraries, and plug-ins available for GraphQL. Broadly, these approaches follow two distinct architectural deployment models: in-band and out-of-band.

## In-Band vs. Out-of-Band

In an in-band authentication and authorization architecture, developers implement client login, signup, role-based access controls, and other permission controls directly in the GraphQL API. The same GraphQL instance that provides clients with their application data also controls the logic that authenticates clients and grants them permissions to view data. In-band GraphQL architectures typically host query or mutation operations that enable clients to send credentials to the API. The API is responsible for verifying these credentials and then issuing tokens to the clients.

Out-of-band authentication and authorization architectures implement the access control and permissions logic on either a separate internal web application service or an external system. In such an architecture, the GraphQL API isn’t responsible for managing client login, signup, or even access control. Instead, it offloads authorization decisions to another component, such as an API gateway, a container sidecar, or another server on the network. This allows developers to decouple the authorization logic from the GraphQL application.

Of the two architectural styles, in-band architectures tend to be more vulnerable to authentication and authorization attacks. Their added complexity increases an API's attack surface drastically. These APIs often duplicate permission logic for each entry point into the service, and as you'll see later in this chapter, we, as hackers, can take advantage of even the slightest misaligned control.

Thus, some contributors to the GraphQL ecosystem advocate for keeping authentication and authorization logic outside of GraphQL. The current industry best practice is to delegate authorization logic to the *business logic layer* of an application, which serves as the single source of truth for all business domain rules. It should sit between the GraphQL layer and the *persistence layer* (also known as the *database* or *datastore* layer), as shown in Figure 7-1.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_190_0.png)

Figure 7-1: The gateway, API, business, and persistence layers

By contrast, authentication for the entire GraphQL API should occur in an external or third-party *gateway layer*, which passes authenticated user contexts along to the API.

## Common Approaches

There is no way to know what kinds of controls you'll come across during your GraphQL hacking adventures. However, this section lists some of the common approaches we've seen in our research and testing. By understanding these techniques, you'll be better equipped to detect them, as well as evaluate the vulnerabilities to which they may be susceptible.

## HTTP Basic Authentication

One of the most rudimentary GraphQL authentication methods is *HTTP basic authentication*. Defined in RFC 7617, this scheme involves the inclusion of a Base64-encoded username and password in the header of a client request. The header looks as follows:

```
Authorization: Basic <base64_encoded_credential>
```
