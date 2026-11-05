
name: "Minimal.WebApi");

HttpRequestMessage request = new(
    method: HttpMethod.Get, requestUri: "api/weather");

HttpResponseMessage response = await client.SendAsync(request);

ViewData["weather"] = await response.Content
    .ReadFromJsonAsync<WeatherForecast[]>();
}
catch (Exception ex)
{
    _logger.LogWarning($"The Minimal.WebApi service is not responding.
Exception: {ex.Message}");
    ViewData["weather"] = Enumerable.Empty<WeatherForecast>().ToArray();
}
```

4. In Views/Home, in Index.cshtml, import the Northwind.Common namespace and then in the top code block get the weather forecasts from the ViewData dictionary, as shown in the following markup:

```
@{
    ViewData["Title"] = "Home Page";
    string currentItem = "";
    WeatherForecast[]? weather = ViewData["weather"] as WeatherForecast[];
}
```

5. In the first <div>, after rendering the current time, add markup to enumerate the weather forecasts unless there aren't any, and render them in a table, as shown in the following markup:

```
<p>
    <h4>Five-Day Weather Forecast</h4>
    @if ((weather is null) || (!weather.Any()))
    {
        <p>No weather forecasts found.</p>
    }
    else
    {
    <table class="table table-info">
        <tr>
        @foreach (WeatherForecast w in weather)
        {
            <td>@w.Date.ToString("ddd d MMM") will be @w.Summary</td>
        }
        </tr>
    </table>
    }
</p>
```

6. Start the Minimal.WebApi service.

7. Start the Northwind.Mvc website.

8. Navigate to https://localhost:5001/, and note the weather forecast, as shown in Figure 16.20:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_746_0.png)

Figure 16.20: A five-day weather forecast on the home page of the Northwind website

9. View the command prompt or terminal for the MVC website and note the info messages that indicate a request was sent to the minimal API web service api/weather endpoint in about 83ms, as shown in the following output:

```
info: System.Net.Http.HttpClient.Minimal.WebApi.LogicalHandler[100]
  Start processing HTTP request GET https://localhost:5003/api/weather
info: System.Net.Http.HttpClient.Minimal.WebApi.ClientHandler[100]
  Sending HTTP request GET https://localhost:5003/api/weather
info: System.Net.Http.HttpClient.Minimal.WebApi.ClientHandler[101]
  Received HTTP response headers after 76.8963ms - 200
info: System.Net.Http.HttpClient.Minimal.WebApi.LogicalHandler[101]
  End processing HTTP request after 82.951ms - 200
```

10. Stop the Minimal.WebApi service, refresh the browser, and note that after a few seconds the MVC website home page appears without weather forecasts.

11. Close Chrome and shut down the web server.

## Practicing and exploring

Test your knowledge and understanding by answering some questions, get some hands-on practice, and explore this chapter's topics with deeper research.

## Exercise 16.1 – Test your knowledge

Answer the following questions:

- 1. Which class should you inherit from to create a controller class for an ASP.NET Core Web API service?
- 2. If you decorate your controller class with the [ApiController] attribute to get default behavior like automatic 400 responses for invalid models, what else must you do?
- 3. What must you do to specify which controller action method will be executed in response to an HTTP request?

- 4. What must you do to specify what responses should be expected when calling an action method?
- 5. List three methods that can be called to return responses with different status codes.
- 6. List four ways that you can test a web service.
- 7. Why should you not wrap your use of HttpClient in a using statement to dispose of it when you are finished even though it implements the IDisposable interface, and what should you use instead?
- 8. What does the acronym CORS stand for and why is it important to enable it in a web service?
- 9. How can you enable clients to detect if your web service is healthy with ASP.NET Core 2.2 and later?
- 10. What benefits does endpoint routing provide?

## Exercise 16.2 – Practice creating and deleting customers with HttpClient

Extend the Northwind.Mvc website project to have pages where a visitor can fill in a form to create a new customer, or search for a customer and then delete them. The MVC controller should make calls to the Northwind web service to create and delete customers.

## Exercise 16.3 – Explore topics

Use the links on the following page to learn more detail about the topics covered in this chapter:

```
https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-16---building-and-consuming-web-services
```

## Summary

In this chapter, you learned how to build an ASP.NET Core Web API service that can be called by any app on any platform that can make an HTTP request and process an HTTP response.

You also learned how to test and document web service APIs with Swagger, as well as how to consume services efficiently.

In the next chapter, you will learn to build user interfaces using Blazor, Microsoft's cool new component technology that enables developers to build client-side, single-page applications (SPAs) for websites using C# instead of JavaScript, hybrid apps for desktop, and potentially mobile apps.

## 17

## Building User Interfaces
Using Blazor

This chapter is about using Blazor to build user interfaces. I will describe the different flavors of Blazor and their pros and cons.

You will learn how to build Blazor components that can execute their code on the web server or in the web browser. When hosted with Blazor Server, it uses SignalR to communicate needed updates to the user interface in the browser. When hosted with Blazor WebAssembly, the components execute their code in the client and must make HTTP calls to interact with the server.

In this chapter, we will cover the following topics:

- Understanding Blazor
- Comparing Blazor project templates
- Building components using Blazor Server
- Abstracting a service for a Blazor component
- Building components using Blazor WebAssembly
- Improving Blazor WebAssembly apps

## Understanding Blazor

Blazor lets you build shared components and interactive web user interfaces using C# instead of JavaScript. In April 2019, Microsoft announced that Blazor "is no longer experimental and we are committing to ship it as a supported web UI framework, including support for running client side in the browser on WebAssembly." Blazor is supported on all modern browsers.

## JavaScript and friends

Traditionally, any code that needs to execute in a web browser is written using the JavaScript programming language or a higher-level technology that **transpiles** (transforms or compiles) into JavaScript. This is because all browsers have supported JavaScript for about two decades, so it has become the lowest common denominator for implementing business logic on the client side.

JavaScript does have some issues, however. Although it has superficial similarities to C-style languages like C# and Java, it is actually very different once you dig beneath the surface. It is a dynamically typed pseudo-functional language that uses prototypes instead of class inheritance for object reuse. It might look human, but you will get a surprise when it's revealed to actually be a Skrull.

Wouldn't it be great if we could use the same language and libraries in a web browser as we do on the server side?

## Silverlight – C# and .NET using a plugin

Microsoft made a previous attempt at achieving this goal with a technology named Silverlight. When Silverlight 2.0 was released in 2008, a C# and .NET developer could use their skills to build libraries and visual components that were executed in the web browser by the Silverlight plugin.

By 2011 and Silverlight 5.0, Apple's success with the iPhone and Steve Jobs' hatred of browser plugins like Flash eventually led to Microsoft abandoning Silverlight since, like Flash, Silverlight is banned from iPhones and iPads.

## WebAssembly – a target for Blazor

A recent development in browsers has given Microsoft the opportunity to make another attempt. In 2017, the **WebAssembly Consensus** was completed, and all major browsers now support it: Chromium (Chrome, Edge, Opera, Brave), Firefox, and WebKit (Safari). Blazor is not supported by Microsoft's Internet Explorer because it is a legacy web browser.

**WebAssembly (Wasm)** is a binary instruction format for a virtual machine that provides a way to run code written in multiple languages on the web at near-native speed. Wasm is designed as a portable target for the compilation of high-level languages like C#.

## Understanding Blazor hosting models

Blazor is a single programming or app model with multiple hosting models:

- **Blazor Server** runs on the server side, so the C# code that you write has full access to all resources that your business logic might need without needing to authenticate. It then uses SignalR to communicate user interface updates to the client side.
