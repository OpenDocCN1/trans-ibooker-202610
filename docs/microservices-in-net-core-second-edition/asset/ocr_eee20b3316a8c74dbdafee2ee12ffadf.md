becomes invalid. The microservice that owns the data is in the best position to decide when a piece of data is still valid and when it has become invalid. Therefore, endpoints responding to queries about data owned by the microservice should include cache headers in the response telling the caller how long it should cache the response data.

# USING HTTP CACHE HEADERS TO CONTROL CACHING

HTTP defines a number of headers that can be used to control how HTTP responses can be cached. The purpose of the HTTP caching mechanisms is twofold:

+   - To eliminate the need, in many cases, to request information the caller already has
+   - To eliminate the need, in some other situations, to send full HTTP responses

To eliminate the need to make requests for information the caller already has, the server can add a cache-control header to responses. The HTTP specification defines a range of controls that can be set in the cache-control header. The most common are the private/public and the max-age directives. The first indicates whether only the caller—private—or any cache the response or intermediaries—proxy servers, for instance—may cache the response, too. The max-age directive indicates the number of seconds the response may be cached. For example, the following cache-control header indicates that the caller, and only the caller, can cache the response for 3,600 seconds:

```
cache-control: private, max-age:3600
```

In other words, the caller may reuse the response any time it wants to make an HTTP request to the same URL with the same method—GET, POST, PUT, DELETE—and the same body within 3,600 seconds. In this book, I will only use caching for queries, which means only for GET requests. It’s worth noting that the query string is part of the URL, so caching takes query strings into account.

To eliminate the need to send a full response in cases where the caller has a cached but stale response, the ETag and If-None-Match headers can be used: the server can add an ETag header to responses. This is an identifier for the response. When the caller makes a later request to the same URL using the same method and the same body, it can include the ETag in a request header called If-None-Match. The server can read the ETag and know which response the caller has cached. If the server decides the cached response is still valid, it can return a response with the “304 Not Modified” status code to tell the client to use the already cached response. Furthermore, the server can add a cache-control header to the 304 response to prolong the period the response may be cached. Note that the ETag is set by the server and later read again by the same server.

Let’s consider the microservices in figure 6.3 again. The shopping cart microservice uses product information that it gets by querying the product catalog microservice. How long the product catalog information for any given product is likely to be correct is best decided by product catalog, which owns the data. Therefore, the product catalog should add cache headers to its responses, and the shopping cart should