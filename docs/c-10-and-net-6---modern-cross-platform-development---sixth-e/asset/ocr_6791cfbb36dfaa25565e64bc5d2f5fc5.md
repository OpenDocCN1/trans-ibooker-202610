| Status code | Description |
|---|---|
| 201 Created | The new resource was created successfully, the response header named Location contains its path, and the response body contains the newly created resource. Immediately GET-ing the resource should return 200. |
| 202 Accepted | The new resource cannot be created immediately so the request is queued for later processing and immediately GET-ing the resource might return 404. The body can contain a resource that points to some form of status checker or an estimate of when the resource will become available. |
| 204 No Content | Commonly used in response to a DELETE request since returning the resource in the body after deleting it does not usually make sense! Sometimes used in response to POST, PUT, or PATCH requests if the client does not need to confirm that the request was processed correctly. |
| 405 Method Not Allowed | Returned when the request used a method that is not supported. For example, a web service designed to be read-only may explicitly disallow PUT, DELETE, and so on. |
| 415 Unsupported Media Type | Returned when the resource in the request body uses a media type that the web service cannot handle. For example, if the body contains a resource in XML format but the web service can only process JSON. |

# Creating an ASP.NET Core Web API project

We will build a web service that provides a way to work with data in the Northwind database using ASP.NET Core so that the data can be used by any client application on any platform that can make HTTP requests and receive HTTP responses:

+   1. Use your preferred code editor to add a new project, as defined in the following list:
+      1. Project template: ASP.NET Core Web API / webapi
+      2. Workspace/solution file and folder: PracticalApps
+      3. Project file and folder: Northwind.WebApi
+      4. Other Visual Studio options: Authentication Type: None, Configure for HTTPS: selected, Enable Docker: cleared, Enable OpenAPI support: selected.
+   2. In Visual Studio Code, select Northwind.WebApi as the active OmniSharp project.
+   3. Build the Northwind.WebApi project.
+   4. In the Controllers folder, open and review WeatherForecastController.cs, as shown in the following code:

```
using Microsoft.AspNetCore.Mvc;

namespace Northwind.WebApi.Controllers;

[ApiController]
[Route("[controller]")]
```