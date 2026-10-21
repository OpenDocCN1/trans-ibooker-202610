# Preventing SSRF

To determine whether an application might be vulnerable to SSRF, we can ask ourselves this question: Does a client have control over any of the target URLs the API flows use? SSRF mostly involves manipulating target URLs by directing them to unexpected and restricted internal or external locations. Here are some strategies for protecting against this:

+   - **Input validation.** Allows rejecting dangerous characters passed to GraphQL arguments that accept URLs as part of a query or mutation. Ensures that only authorized URLs are accepted and helps reduce the risk of SSRF.

+   - **Network segmentation.** Helps minimize the risk by ensuring that applications can communicate with only the relevant internal networks. A vulnerable GraphQL API in your staging network shouldn’t be able to reach another GraphQL API in your production network.

+   - **Threat modeling.** Allows identifying potential risks earlier in the development life cycle of GraphQL APIs and, more specifically, in queries or mutations that have the potential to be vulnerable to SSRF.

+   - **Least privileges principle.** Helps minimize the blast radius. Ensure that the instance on which GraphQL runs does not have overly permissive permissions and cannot perform privileged actions across applications.

In the next section, we’ll pivot to talking about hijacking-based vulnerabilities that impact GraphQL subscriptions.

# Cross-Site WebSocket Hijacking

If an attacker can hijack a user’s session by getting their hands on session cookies that grant special privileges on an application, they can perform actions using the victim’s privileges and access their sensitive data. CSWSH is a CSRF vulnerability that impacts the handshake part of WebSocket communications, which use cookie-based authentication. Because GraphQL APIs can use WebSocket for subscription operations, they risk being vulnerable to CSWSH.

In Chapter 3, we showed the handshake request and response sent between a GraphQL client and server when using subscriptions over WebSocket to communicate. Clients initiate these WebSocket handshakes over HTTP and may include a cookie like the following if the WebSocket server implements authentication:

```
Cookie: session=somesessionID
```

CSWSH can occur when a WebSocket connection handshake doesn’t include an anti-CSRF token to prevent attackers from performing cross-origin requests. When no such token exists, it’s easy for an attacker to