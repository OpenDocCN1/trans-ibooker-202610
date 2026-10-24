use them to decide how long it can cache a response. Figure 6.4 shows a sequence of requests to the product catalog that the shopping cart wants to make.

![](bbox=[0.186, 0.149, 0.849, 0.684])

**Figure 6.4** The product catalog microservice can allow its collaborators to cache responses by including cache headers in its HTTP responses. In this example, it sets max-age to indicate how long responses may be cached, and it also includes an etag built from the product IDs and versions.

In figure 6.4, the cache headers on the response to the first request tell the shopping cart microservice that it can cache the response for 3,600 seconds. The second time the shopping cart wants to make the same request, the cached response is reused because fewer than 3,600 seconds have passed. The third time, the request to the product catalog microservice is made because more than 3,600 seconds have passed. That request includes the ETag from the first response in the If-None-Match header. Product catalog uses the ETag to decide that the response would still be the same, so it sends back