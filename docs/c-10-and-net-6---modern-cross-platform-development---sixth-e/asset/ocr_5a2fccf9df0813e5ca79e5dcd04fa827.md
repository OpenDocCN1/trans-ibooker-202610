+   - **Port number**: For a production website or service, `80` for http, `443` for https. These port numbers are usually inferred from the scheme. During development, other port numbers are commonly used, such as `5000`, `5001`, and so on, to differentiate between websites and services that all use the shared domain `localhost`.
+   - **Path**: A relative path to a resource, for example, `/customers/germany`.
+   - **Query string**: A way to pass parameter values, for example, `?country=Germany&searchtext=shoes`.
+   - **Fragment**: A reference to an element on a web page using its id, for example, `#toc`.

# Assigning port numbers for projects in this book

In this book, we will use the domain localhost for all websites and services, so we will use port numbers to differentiate projects when multiple need to execute at the same time, as shown in the following table:

| Project | Description | Port numbers |
|---|---|---|
| Northwind.Web | ASP.NET Core Razor Pages website | `5000` HTTP, `5001` HTTPS |
| Northwind.Mvc | ASP.NET Core MVC website | `5000` HTTP, `5001` HTTPS |
| Northwind.WebApi | ASP.NET Core Web API service | `5002` HTTPS, `5008` HTTP |
| Minimal.WebApi | ASP.NET Core Web API (minimal) | `5003` HTTPS |
| Northwind.OData | ASP.NET Core OData service | `5004` HTTPS |
| Northwind.GraphQL | ASP.NET Core GraphQL service | `5005` HTTPS |
| Northwind.gRPC | ASP.NET Core gRPC service | `5006` HTTPS |
| Northwind.AzureFuncs | Azure Functions nanoservice | `7071` HTTP |

# Using Google Chrome to make HTTP requests

Let's explore how to use Google Chrome to make HTTP requests:

+   1. Start Google Chrome.
+   2. Navigate to **More tools** | **Developer tools**.