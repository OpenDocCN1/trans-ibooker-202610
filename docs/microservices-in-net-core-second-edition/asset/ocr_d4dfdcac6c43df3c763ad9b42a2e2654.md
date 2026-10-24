has only a single API endpoint. Typically, a microservice has more than one endpoint, but one is enough for this example. The endpoint responds with the current UTC date and time. This is illustrated in figure 1.9.

![](bbox=[0.192, 0.175, 0.634, 0.244])

**Figure 1.9**  A “Hello World”-style microservice that responds with the current date and time

> **NOTE**  When I talk about an *API endpoint*, an *HTTP endpoint*, or just an *endpoint*, I mean a URL where one of your microservices reacts to HTTP requests.

To implement this example, you’ll follow these three steps:

+   1. Create an empty ASP.NET application.
+   2. Add ASP.NET MVC to the application.
+   3. Add an MVC controller with an implementation of the endpoint.

The following sections will go through each step in detail.

# 1.9.1  Creating an empty ASP.NET application

The first thing you need to do is create an empty ASP.NET application called `Hello-Microservices` using the .NET command-line tool. If you prefer, you can create the project using your IDE—the result is the same. To use the .NET command-line tool, open a shell and issue the command `dotnet new web --name HelloMicroservices`.

Once you’ve created your empty ASP.NET application and named it Hello-Microservices, you should have a folder called HelloMicroservices that contains these files:

+   - appsettings.Development.json
+   - appsettings.json
+   - HelloMicroservices.csproj
+   - Program.cs
+   - Startup.cs

There are other files in the project, but these are the one you’ll be concerned with right now.

This is a complete application, ready to run. It will respond to HTTP GET requests to the path “/” with the string Hello World. You can start the application from the command line by going to the HelloMicroservices folder and typing the command `dotnet run`.

The application runs on localhost port 5000. (Note that if you choose to run it from inside Visual Studio, you may get another port.) If you go to http://localhost:5000 in a browser, you’ll get the “Hello World” response.