# Health Check Response Format for HTTP APIs

![](bbox=[0.27, 0.12, 0.58, 0.37])

# NOTE

For details on the health Health Checks resource, check out the online collection of ALPS documents 73 associated with this book.

# Example

A typical application/health+json response looks like this:

```
HTTP/1.1 200 OK
Content-Type: application/health+json
Cache-Control: max-age=3600
ETag: "w\i8u7y6t5r4e3w2"
...
{
  "status": "pass",
  "version": "1",
  "releaseId": "1.2.2",
  "notes": [""],
  "output": "",
  "serviceId": "f03e522f-1f44-4062-9b55-9587f91c9c41",
  "description": "health of authz service",
  "checks": {
    "cassandra:responseTime": [
```