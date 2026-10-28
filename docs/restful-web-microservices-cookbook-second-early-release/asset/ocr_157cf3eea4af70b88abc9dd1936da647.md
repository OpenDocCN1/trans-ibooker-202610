application is talking to multiple server-side components?

# Solution

The safest place to keep transient state — the state of the application as defined by runtime requests and responses — is in the client. The client is responsible for selecting which servers are engaged in interactions and the client the one initiating HTTP requests. That means it is the client, and only the client, that has the most accurate picture of the state of the application. This is especially true when the client has enlisted multiple services (services which are unrelated to each other) in order to accomplish a task or goal.

The easiest way to do this if for the client application to keep a detailed history of each request/response interaction and for that client to select (or compute) important application state data from that history. Most programming tools support serializing HTTP streams.

Typically, the following elements should be captured and stored by the client.

+   Request Elements: * URL * Method * Headers * Query String * Request Body

+   Response Elements: * URL * Status * Content-Type * Headers * Body

Note that the URL value from the request might not be the same as the URL of the response. This might be due to an incomplete request URL (e.g. missing the protocol element: `api.example.org/users`) or as a result of server-side redirection.

# Example

Below is a snippet of code from the **hyper** CLI client application that manages the request and response elements of HTTP interactions.

```
...
// collect request info for later
requestInfo = {};
requestInfo.url = url;
```