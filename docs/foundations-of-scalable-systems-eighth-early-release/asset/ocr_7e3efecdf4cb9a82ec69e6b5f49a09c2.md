# no-cache

Specifies that a cached resource must be revalidated with an origin server before use. I discuss revalidation in the Etag subsection below.

# Private

Specifies a resource can be cached only by a user-specific device such as a Web browser

# Public

Specifies a resource can be cached by any proxy server

# max-age

Defines the length of time in seconds a cached copy of a resource should be retained. After expiration, a cache must refresh the resource by sending a request to the origin server.

# Expires and Last-Modified

The Expires and Last-Modified HTTP headers interact with the max-age directive to control how long cached data is retained.

Caches have limited storage resources and hence must periodically evict items from memory to create space. To influence cache eviction, services can specify how long resources in the cache should remain valid, or fresh. When a request arrives for a fresh resource, the cache serves the locally stored results without contacting the origin server. Once any specified retention period for a cached resource expires, it becomes stale and becomes a candidate for eviction.

Freshness is calculated using a combination of header values. The "Cache-Control: max-age=N" header is the primary directive, and this value specifies the freshness period in seconds.