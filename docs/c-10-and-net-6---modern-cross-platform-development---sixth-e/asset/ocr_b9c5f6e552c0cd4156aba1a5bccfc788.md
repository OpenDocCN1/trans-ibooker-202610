![](bbox=[0.175, 0.062, 0.933, 0.368])

Figure 16.1: A request and response from a weather forecast web service

+   9. Close Developer tools.
+   10. Navigate to https://localhost:5001/weatherforecast/14 and note the response when requesting a two-week weather forecast, as shown in Figure 16.2:
+   11. Close Chrome and shut down the web server.

![](bbox=[0.175, 0.48, 0.933, 0.67])

Figure 16.2: A two-week weather forecast as a JSON document

# Creating a web service for the Northwind database

Unlike MVC controllers, Web API controllers do not call Razor views to return HTML responses for website visitors to see in browsers. Instead, they use content negotiation with the client application that made the HTTP request to return data in formats such as XML, JSON, or X-WWW-FORM-URLENCODED in their HTTP response.