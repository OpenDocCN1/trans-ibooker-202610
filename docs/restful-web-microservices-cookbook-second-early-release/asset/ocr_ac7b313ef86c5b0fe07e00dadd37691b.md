Client applications that depend on services that provide responses using pre-determined semantic profiles need a way to make sure those services will be “speaking” in the required vocabulary.

# Problem

You’ve coded your client to work with any service that complies with, for example, the Tᵒ–Dᵒ semantic profile. How can you confirm — at runtime — that the service you are about to use supports the Tᵒ–Dᵒ vocabulary?

# Solution

A dependable for a client application to check for semantic profile support in a service is to use the “profile negotiation” pattern. Clients can use the `accept-profile` request header to indicate the expected semantic profile and services can use the `content-profile` response header to indicate the supporting semantic profile.

Like content negotiation (see TK), profile negotiation allows clients and servers to share metadata details about the resource representation and make decisions on how whether the server’s response is acceptable for the client. When services return resources profiles that do not match the client’s request, the client can reject the response w/ an error, request more information from the server, or continue anyway.

# Example

There are a number of ways clients can determine the supported semantic profiles for a service resource. Profile negotiation is the most direct way for clients to indicate and validation a service’s profile support.

A simple example of profile negotiation is shown below:

```
```http
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept-Profile: <http://profiles.example.org/to-do>
```
```