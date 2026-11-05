
Roy Fielding stated in his doctoral dissertation, describing the **Representational State Transfer (REST)** architectural style, that the HTTP standard would be good for building services because it defines the following:

- URIs to uniquely identify resources, like `https://localhost:5001/api/products/23`.
- Methods to perform common tasks on those resources, like GET, POST, PUT, and DELETE.
- The ability to negotiate the media type of content exchanged in requests and responses, such as XML and JSON. Content negotiation happens when the client specifies a request header like Accept: `application/xml,*/*;q=0.8`. The default response format used by the ASP.NET Core Web API is JSON, which means one of the response headers would be Content-Type: `application/json; charset=utf-8`.

**Web services** use the HTTP communication standard, so they are sometimes called HTTP or RESTful services. HTTP or RESTful services are what this chapter is about.

Web services can also mean **Simple Object Access Protocol (SOAP)** services that implement some of the **WS-* standards**. These standards enable clients and services implemented on different systems to communicate with each other. The WS-* standards were originally defined by IBM with input from other companies like Microsoft.

## Understanding Windows Communication Foundation (WCF)

.NET Framework 3.0 and later includes a **remote procedure call (RPC)** technology named **Windows Communication Foundation (WCF)**. RPC technologies enable code on one system to execute code on another over a network.

WCF makes it easy for developers to create services, including SOAP services that implement WS-* standards. It later also supported building Web/HTTP/REST-style services, but it was rather over-engineered if that was all you needed.

If you have existing WCF services and you would like to port them to modern .NET, then there is an open-source project that had its first **General Availability (GA)** release in February 2021. You can read about it at the following link:

```
https://corewcf.github.io/blog/2021/02/19/corewcf-ga-release
```

## An alternative to WCF

The Microsoft recommended alternative to WCF is **gRPC**. gRPC is a modern cross-platform open-source RPC framework created by Google (unofficially the "g" in gRPC). You will learn more about gRPC in *Chapter 18, Building and Consuming Specialized Services*.

## Understanding HTTP requests and responses for Web APIs

HTTP defines standard types of requests and standard codes to indicate a type of response. Most of them can be used to implement Web API services.

The most common type of request is GET, to retrieve a resource identified by a unique path, with additional options like what media type is acceptable, set as request headers, as shown in the following example:

```
GET /path/to/resource
Accept: application/json
```

Common responses include success and multiple types of failure, as shown in the following table:

| Status code | Description |
|---|---|
| 200 OK | The path was correctly formed, the resource was successfully found, serialized into an acceptable media type, and then returned in the response body. The response headers specify the Content-Type, Content-Length, and Content-Encoding, for example, GZIP. |
| 301 Moved Permanently | Over time a web service may change its resource model including the path used to identify an existing resource. The web service can indicate the new path by returning this status code and a response header named Location that has the new path. |
| 302 Found | Similar to 301. |
| 304 Not Modified | If the request included the If-Modified-Since header, then the web service can respond with this status code. The response body is empty because the client should use its cached copy of the resource. |
| 400 Bad Request | The request was invalid, for example, it used a path for a product using an integer ID where the ID value is missing. |
| 401 Unauthorized | The request was valid, the resource was found, but the client did not supply credentials or is not authorized to access that resource. Re-authenticating may enable access, for example, by adding or changing the Authorization request header. |
| 403 Forbidden | The request was valid, the resource was found, but the client is not authorized to access that resource. Re-authenticating will not fix the issue. |
| 404 Not Found | The request was valid, but the resource was not found. The resource may be found if the request is repeated later. To indicate that a resource will never be found, return 410 Gone. |
| 406 Not Acceptable | If the request has an Accept header that only lists media types that the web service does not support. For example, if the client requests JSON but the web service can only return XML. |
| 451 Unavailable for Legal Reasons | A website hosted in the USA might return this for requests coming from Europe to avoid having to comply with the General Data Protection Regulation (GDPR). The number was chosen as a reference to the novel Fahrenheit 451 in which books are banned and burned. |
| 500 Server Error | The request was valid, but something went wrong on the server side while processing the request. Retrying again later might work. |
| 503 Service Unavailable | The web service is busy and cannot handle the request. Trying again later might work. |

Other common types of HTTP requests include POST, PUT, PATCH, or DELETE that create, modify, or delete resources.

To create a new resource, you might make a POST request with a body that contains the new resource, as shown in the following code:

```
POST /path/to/resource
Content-Length: 123
Content-Type: application/json
```

To create a new resource or update an existing resource, you might make a PUT request with a body that contains a whole new version of the existing resource, and if the resource does not exist, it is created, or if it does exist, it is replaced (sometimes called an upsert operation), as shown in the following code:

```
PUT /path/to/resource
Content-Length: 123
Content-Type: application/json
```

To update an existing resource more efficiently, you might make a PATCH request with a body that contains an object with only the properties that need changing, as shown in the following code:

```
PATCH /path/to/resource
Content-Length: 123
Content-Type: application/json
```

To delete an existing resource, you might make a DELETE request, as shown in the following code:

```
DELETE /path/to/resource
```

As well as the responses shown in the table above for a GET request, all the types of requests that create, modify, or delete a resource have additional possible common responses, as shown in the following table:

| Status code | Description |
|---|---|
| 201 Created | The new resource was created successfully, the response header named Location contains its path, and the response body contains the newly created resource. Immediately GET-ing the resource should return 200. |
| 202 Accepted | The new resource cannot be created immediately so the request is queued for later processing and immediately GET-ing the resource might return 404. The body can contain a resource that points to some form of status checker or an estimate of when the resource will become available. |
| 204 No Content | Commonly used in response to a DELETE request since returning the resource in the body after deleting it does not usually make sense! Sometimes used in response to POST, PUT, or PATCH requests if the client does not need to confirm that the request was processed correctly. |
| 405 Method Not Allowed | Returned when the request used a method that is not supported. For example, a web service designed to be read-only may explicitly disallow PUT, DELETE, and so on. |
| 415 Unsupported Media Type | Returned when the resource in the request body uses a media type that the web service cannot handle. For example, if the body contains a resource in XML format but the web service can only process JSON. |

## Creating an ASP.NET Core Web API project

We will build a web service that provides a way to work with data in the Northwind database using ASP.NET Core so that the data can be used by any client application on any platform that can make HTTP requests and receive HTTP responses:

1. Use your preferred code editor to add a new project, as defined in the following list:
   1. Project template: ASP.NET Core Web API / webapi
   2. Workspace/solution file and folder: PracticalApps
   3. Project file and folder: Northwind.WebApi
   4. Other Visual Studio options: Authentication Type: None, Configure for HTTPS: selected, Enable Docker: cleared, Enable OpenAPI support: selected.
2. In Visual Studio Code, select Northwind.WebApi as the active OmniSharp project.
3. Build the Northwind.WebApi project.
4. In the Controllers folder, open and review WeatherForecastController.cs, as shown in the following code:

```
using Microsoft.AspNetCore.Mvc;

namespace Northwind.WebApi.Controllers;

[ApiController]
[Route("[controller]")]
```
