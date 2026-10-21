
As you can see, GraphQL queries are uniquely structured, and it is important to understand the various parts. Table 3-1 provides a description of each component.

## Table 3-1: The Components of a GraphQL Query

| # | Component | Description |
|---|---|---|
| 1 | Operation type | Type that defines the method of interaction with the server (query, mutation, or subscription) |
| 2 | Operation name | Arbitrary client-created label used to provide a unique name to an operation |
| 3 | Top-level field | Function that returns a single unit of information or object requested within an operation (may contain nested fields) |
| 4 | Argument (of a top-level field) | Parameter name used to send information to a field to tailor the behavior and results of that field |
| 5 | Value | Data related to an argument sent to a field |
| 6 | Field | Nested function that returns a single unit of information or object requested within an operation |
| 7 | Directive | Feature used to decorate fields to change their validation or execution behavior, altering a value returned by a GraphQL server |
| 8 | Argument (of a directive) | Parameter name used to send information to a field or object to tailor its behavior and results |
| 9 | Argument (of a field) | Parameter name used to send information to a field to tailor the behavior and results of the field |

The following sections explore these components, as well as a few additional GraphQL features, with a focus on how they contribute to GraphQL’s attack surface.

## Queries, Mutations, and Subscriptions

We discussed the root operation types query, mutation, and subscription in Chapter 1 and showed an example of using the query type to retrieve data. (For that reason, we won’t revisit the query type here.) As hackers, the real fun often happens when we can modify data. Creating, updating, and deleting data within a target platform empowers us to expose business logic flaws.

> NOTE We recommend using the GraphQL security lab we deployed in Chapter 2 to follow along with this chapter’s example requests. Use Altair to send the requests to DVGA (http://localhost:5013/graphql).

### Mutations

In GraphQL, we can unlock data modification powers by using *mutations*. Here is an example of a mutation query:

```
mutation {
  editPaste(id: 1, content: "My first mutation!") {
    paste {
      id
      title
      content
    }
  }
}
```

We define the mutation operation by using the *mutation* keyword. Then we call the top-level editPaste field, which accepts the arguments id and content. (We will discuss arguments later in this chapter.) This mutation essentially takes the paste with the id of 1 and updates its content. We then request the updated paste. This is an example of a mutation that changes and reads data simultaneously.

> NOTE
> As in REST, where an HTTP GET request is conventionally used to read information but could also be used to modify data, it is possible for a GraphQL implementation to ignore the spec and implement query operations in a way that allows data writes. In Chapter 9, we will explore why writing data using a GraphQL query can be a bad idea.

### Subscriptions

The *subscription* operation works bidirectionally: it allows clients to retrieve real-time data from a server, and allows servers to send updates to clients. Subscriptions are not as common as queries and mutations, but many servers do use them, so it is important to know how they work.

Subscriptions are carried over a transport protocol, most commonly *WebSocket*, a real-time communication protocol that allows clients and servers to exchange messages at any given time over a long-lived connection. However, because the GraphQL specification doesn’t define which transport protocol to use for subscriptions, you might see consumers use other ones.

When a client and server want to communicate over WebSocket, they perform a handshake that upgrades the existing HTTP connection to a WebSocket one. WebSocket internals are outside the scope of this book, but you can learn more about this technology by reading PortSwigger’s technical blog post on the topic at https://portswigger.net/web-security/websockets/what-are-websockets.

Because DVGA supports subscriptions over WebSocket, we can observe the handshake between DVGA’s frontend interface and the GraphQL server. Clients can use subscriptions to fetch information from the DVGA server, such as newly created pastes. For example, when you browse to the Public Pastes page on http://localhost:5013, you should see an outgoing HTTP request that looks like the following in the browser’s developer tools Network tab:

```
GET /subscriptions HTTP/1.1
Host: 0.0.0.0:5013
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
Upgrade: websocket
Origin: http://localhost:5013
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: MV5U8J3GH1UG8A1Eb181HiA==
```

The GraphQL server response to this handshake request looks like this:

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: aRn1pG8XwzRHPVxYmGVdqJv3D7U=
```

As you can see, the handshake caused the client and server to switch from HTTP to WebSocket, as indicated by the response code of 101 Switching Protocols. The Sec-WebSocket-Accept response header informs the client that the server has accepted the protocol switch.

After the handshake completes, DVGA will send a subscription request over the newly established WebSocket connection:

```
subscription {
  paste {
    id
    title
    content
  }
}
```

We define the subscription operation by using the subscription keyword, then request the paste top-level field and select the id, title, and content fields. This subscription allows clients to subscribe to the paste field; whenever a new paste is created in DVGA, the GraphQL server will notify all subscribers of the event. This removes the need for the client to constantly ask the server for updates, which is especially useful because the server may not have anything new to return at that exact moment.

If you want to try sending this subscription request to DVGA by using Altair, you’ll need to assign a subscription URL. You can do so in Altair by clicking the two-arrow icon on the left sidebar and entering the WebSocket URL ws://localhost:5013/subscriptions. Next, to receive data from the DVGA subscription, you’ll need to create a paste. You can either use the DVGA user interface to create it via the Public Pastes page or send a mutation, like the following one, from another Altair tab:

```
mutation {
  createPaste(title: "New paste", content: "Test", public: false) {
    paste {
      id
      title
      content
    }
  }
}
```

WebSocket connections are prone to cross-site WebSocket hijacking (CSWSH) vulnerabilities, which happen when the server does not validate the origin of a client in the handshake process. WebSocket connections can also be vulnerable to man-in-the-middle (MITM) attacks when the transport of messages isn’t carried over an encrypted channel such as Transport Layer Security (TLS). The existence of such vulnerabilities could have a security impact on actions carried over GraphQL subscriptions. In Chapter 9, we’ll cover WebSocket-based attacks in more detail.

## Operation Names

GraphQL operation names are labels used to uniquely identify an operation in certain contexts. They appear in the executable documents that clients send to a GraphQL service. These documents can contain a list of one or more operations. For example, the document in Listing 3-1 shows a single query operation requesting a pastes top-level field with a nested title field.

```
query {
  pastes {
    title
  }
}
```

Listing 3-1: An executable query document

If a document contains only one operation, and that operation is a query defining no variables and containing no directives, then that operation may be represented in its shorthand form, without the query keyword, as shown in Listing 3-2.

```
{
  pastes {
    title
  }
}
```

Listing 3-2: A shorthand query document
