On the wire, this is the request:

```
GET / HTTP/1.1
Host: localhost:5001
Accept: application/json
```

HTTP method (GET), path (/), and protocol (HTTP/1.1) used in the request

Host to which the request is made

List of request headers. In this case, there's only an Accept header with the value application/json.

The response from this request is the current UTC data and time in a JSON string:

```
HTTP/1.1 200 OK
Connection: close
Date: Thu, 21 Nov 2019 12:28:01 GMT
Content-Type: application/json; charset=utf-8
Server: Kestrel
Transfer-Encoding: chunked
```

```
"2019-11-21T12:28:02.5561423Z"
```

> NOTE Remember you can create HTTP request and see the responses using tools like REST client for VS Code, Postman, or Fiddler.

You've now implemented your first HTTP endpoint with ASP.NET MVC.

# Summary

+   - Microservices is an overloaded term used both for the microservices architectural style and for individual microservices in a system of microservices.
+   - The microservices architectural style is a special form of SOA, where each service is small and provides one and only one business capability.
+   - A microservice is a service with a single narrowly focused capability.
+   - I'll refer to six characteristics of a microservice in this book:
+     - Provides a single capability.
+     - Individually deployable. You must be able to deploy every microservice on its own without touching any other part of the system.
+     - Runs in one or more processes, separate from other microservices.
+     - Owns and stores the data belonging to the capability it provides in a data store that the microservice itself has access to.
+     - Is small enough that a small team of around five people can develop and maintain a few handfuls or more of them.
+     - Replaceable. The team should be able to rewrite a microservice from scratch in a short period of time if, for instance, the codebase has become a mess.
+   - Microservices go hand-in-hand with continuous delivery:
+     - Having small, individually deployable microservices makes continuous delivery easier.
+     - Being able to deploy automatically, quickly, and reliably simplifies deploying and maintaining a system of microservices.