the Etag represents the first version of the report for the Blackstone resort on the 26th March, 2019. Other common strategies are to generate the Etag using a hash algorithm such as MD5.

# Example 6-3. Figure 6-5 HTTP Etag Example

```
Request:GET /skico.com/weather/Blackstone

Response:
HTTP/1.1 200 OK Content-Length: ...
Content-Type: application/json
Date: Fri, 26 Mar 2019 09:33:49 GMT
Cache-Control: public, max-age=3600
ETag: "blackstone-weather-03/26/19-v1"
<!-- Content omitted -->
```

For the next hour, the Web cache simply serves this cached weather report to all clients who issue a GET request. This means the origin servers are freed from processing these requests – the outcome that we want from effective caching. After an hour though, the resource becomes stale. Now, when a request arrives for a stale resource, the cache forwards it to the origin server with a If-None-Match directive along with the Etag to enquire if the resource, in our case the weather report, is still valid. This is known as revalidation.

There are two possible responses to this request.

+   - If the Etag in the request matches the value associated with the resource in the service, the cached value is still valid. The origin server can therefore return a 304 (Not Modified) response, as shown in Figure 6-6. No response body is needed as the cached value is still current, thus saving bandwidth, especially for large resources. The response may also include new cache directives to update the freshness of the cached resource.

+   - The origin server may ignore the revalidation request and respond with a 200 OK response code, a response body and Etag representing the latest version of the weather report.

# Example 6-4. Figure 6-6 Validating an Etag