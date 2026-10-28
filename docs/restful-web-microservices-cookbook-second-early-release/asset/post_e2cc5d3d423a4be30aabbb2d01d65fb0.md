
Health and status monitoring of services running on the web is a common challenge. The good news is that there has been lots of good work in the area of standardizing this process over the last few years. While I write this I see in my email inbox that the IETF is in the process of reviewing a draft document called “Health Check Response Format for HTTP APIs”.⁷² This specification was first published in 2018 and I’ve been using it as a guide for implementing my own health and status checking for quite some time.

The Health Check specification defines several properties. Below are the basics along with some notes:

- **status**: indicates the service status (`pass`, `fail`, `warn`).
- **version**: public version of the service.
- **releaseId**: service release/version.
- **notes**: array of notes relevant to current state of health.
- **output**: raw error output, in case of `fail` or `warn` states.
- **checks**: an object that provides detailed health statuses of additional downstream systems.
- **links**: an object containing link relations and URIs for external links that MAY contain more information about the health of the service.
- **serviceId**: a unique identifier of the service.
- **description**: a human-friendly description of the service.

> **NOTE**
>
> The `checks` object represents a collection of related “downstream” services. It is a bit of a “look ahead” to the health of other dependent services. This object has several more properties defined and you can review the specification for details.

Here are some additional things to keep in mind when implementing the Health Check specification.

## Health Resource

## Health Check Response Format for HTTP APIs

![](img/b73a1e885955ee1243f0097b04d814ff_231_0.png)

> **NOTE**
>
> For details on the health check resource, check out the online collection of ALPS documents⁷³ associated with this book.

### Example

A typical application/health+json response looks like this:

```json
HTTP/1.1 200 OK
Content-Type: application/health+json
Cache-Control: max-age=3600
ETag: W/"i8u7y6t5r4e3w2"
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
      {
        "componentId": "dfd6cf2b-1b6e-4412-a0b8-f6f7797a60d2",
        "componentType": "datastore",
        "observedValue": 250,
        "observedUnit": "ms",
        "status": "pass",
        "affectedEndpoints": [
          "/users/{userId}",
          "/customers/{customerId}/status",
          "/shopping/{anything}"
        ],
        "time": "2018-01-17T03:36:48Z",
        "output": ""
      },
      ...
    ]
  }
}
```

Note the use of `cache-control` and `etag` headers to instruct client applications to retain local copies of the document to reduce traffic loads to the service.

### Health Check Info in OPTIONS Responses

You can “advertise” support for the health check pattern by including a `link` relation value in responses. I do this for all my OPTIONS responses.

```http
**** REQUEST ****
OPTIONS / HTTP/1.1
Accept: application/vnd.collection+json, application/html, application/json
...

**** RESPONSE ****
HTTP/1.1 200 OK
Content-Type: application/vnd.collection+json
Link: <http://api.example.org/health>; rel="https://api.example.org/rels/health"
...
```

### Health Check Info in Service-Meta Responses

I also include support for health checks in my service metadata document (see [Recipe 4.10](#)).

```http
HTTP/1.1 200 OK
Content-Type: application/apis+json
...

{
  "name": "Example API",
  "type": "Index",
  "description": "This is an example APIs.json file.",
  "image": "https://api.example.org/logo.jpg",
  "tags": ["Application Programming Interface", "API"],
  "created": "2014-04-07",
  "modified": "2020-09-03",
  "url": "http://example.com/apis.json",
  "specificationVersion": "0.14",
  "apis": [
    {
      "name": "Example API",
      "description": "This provides details about a specific API.",
      "humanURL": "http://example.com",
      "baseURL": "http://api.example.com",
      "tags": ["API", "Application Programming Interface"],
      "properties": [
        {"type": "Documentation", "url": "https://example.com/documentation"},
        {"type": "OpenAPI", "url": "http://example.com/openapi.json"},
        {"type": "Health", "url": "http://example.com/health.json"}
      ],
      "contact": [{"FN": "APIs.json", "email": "info@apisjson.org"}]
    }
  ],
  "..."
}
```

Note that the “health” type in **APIs.json** is an extension value (not included in the **APIs.json** specification). See [Recipe 4.10](#) for details.

### Discussion

In most cases, health checks are helpful when they stick to the basics. Reporting `pass`, `fail` and `warn` is usually more than enough for service consumers. It is not a good idea to try to use health checks as a debugging or diagnostic tool. Save that for some other resource requests.

> **WARNING**
>
> This recipe is based on the draft-06 version of the “Health Check Response Format for HTTP APIs” document. By the time you read this book, there may be new drafts and/or the final approved RFC may be published. Be sure to keep an eye on this specification document to stay informed on changes.

Some readers might want to set up a “callback” health endpoint that allows others to subscribe to regular responses from your service. This is not recommended. As the number of users of your service grows, it is possible that you’ll get hundreds, possibly thousands of callback requests to handle. This can easily overwhelm your service. For that reason, stick to offering an HTTP GET endpoint that has a `cache-control` value that reduces the impact of lots of health requests. It would be the ultimate irony that it is the health checks that cause the service to be unable to meet your service level agreements!

The specification points out that responses for health checks are “dynamic”; the contents of the response may be customized based on the calling context. For example, anonymous requests might return only the `status` value (`pass`, `fail`, `warn`) but authorized requests (e.g. admin users) might see detailed information on the status of the service.

The specification does not provide lots of details on extending the `application/health+json` responses. If you want to include additional information, not covered by the defined properties, be sure to do it in a backward-compatible manner and provide links to human-readable documentations explaining your extension.

### Related

- Chapter 2
- Chapter 3
- Chapter 4
- [Link to Come]

The key to success is to treat errors as an “alternate response” instead of a failed request. In other words, always include error reporting as a feature of your service interface, not a failure condition.

### Example

The Problem Details specification (RFC7807) defines a small set of elements:

- **type**: A URI reference [RFC3986] that identifies the problem type.
- **title**: A short, human-readable summary of the problem type.
- **status**: The HTTP status code generated by the service (this is a number, not a string).
- **detail**: A human-readable explanation specific to this occurrence of the problem.
- **instance**: A URI reference that identifies the specific occurrence of the problem.

The specification defines a default value for **type**: "about:blank". It also assumes any URI found in **type** will be a pointer to a human-readable document that describes the problem.

Here’s a typical Problem Details message:

```
HTTP/1.1 403 Forbidden
Content-Type: application/problem+json
Content-Language: en

{
  "type": "https://example.com/probs/out-of-credit",
  "title": "You do not have enough credit.",
  "detail": "Your current balance is 30, but that costs 50.",
  "instance": "/account/12345/msgs/abc",
  "status": 403
}
```

It is also possible to extend problem details records using your own properties:
