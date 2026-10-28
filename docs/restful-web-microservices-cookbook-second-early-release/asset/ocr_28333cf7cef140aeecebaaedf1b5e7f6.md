the provided schema URI/URL as an *identifier* in order to confirm the service has returned a response with the expected semantic metadata.

# WARNING

Using schema documents to validate *incoming* messages runs counter to Postel’s Law ¹⁸ which states: “be conservative in what you do, be liberal in what you accept from others”. Strong-typing incoming messages (instead of “be[ing] liberal in what you accept”) is likely to reject responses that might otherwise work well for both client and server. The specification document RFC112 ¹⁹, authored by Robert Braden, goes into great detail on how to design and implement features that can safely allow for minor variations in message and protocol details. See TK for the flip-side of this challenge — “be[ing] conservative in what you do” when it comes to applying schemas to *outgoing* messages.

# Negotiating for Schemas

It is possible for client applications to engage in schema negotiation with the service. This is useful when client and server both already know ahead of time that there are multiple editions of the same general schema (user-v1, user-v2, etc.) and you want to confirm both are working with the same edition. An expired proposal ²⁰ from the W3C suggested the use of `accept-schema` and `schema` HTTP headers:

```
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept-Schema: <urn:example:schema:e-commerce-payment>

*** RESPONSE
HTTP/1.1 200 OK
Schema: <urn:example:schema:e-commerce-payment
...
```

I’ve not encountered this negotiation approach “in the wild.” If it is offered by a service, it is worth looking into as a client developer — any semantic metadata is better then none.

# Schema Identifier in the Content-Type Header