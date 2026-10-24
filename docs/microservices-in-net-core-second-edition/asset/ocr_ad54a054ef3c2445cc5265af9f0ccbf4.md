deploy our microservices to the Kubernetes cluster. Then we will continue to deploy microservices to Kubernetes.

# 1.8.3 Setting up a development environment

Before you can start coding your first microservice, you need to have the right tools. To follow along with the examples in this book, you'll need four primary tools: an IDE, the .NET command line, an HTTP tool, and Docker.

First, you need a development environment for creating ASP.NET applications. The three most common options are as follows:

+   - Visual Studio—Visual Studio is a fully fledged IDE with great support for writing and debugging ASP.NET application. It is Windows-only and is probably the most widely used IDE for ASP.NET development.
+   - Visual Studio Code—Visual Studio Code is a glorified editor that comes with a C# plug-in that makes a very capable editor for writing ASP.NET applications. It is lightweight and works across Windows, Linux, and Mac.
+   - JetBrains Rider—Rider is another fully fledged IDE with strong support for writing and debugging ASP.NET applications. Rider works on Windows, Linux, and Mac alike.

All these support developing, running, and debugging ASP.NET applications. They all give you a nice C# editor with IntelliSense and refactoring support. They're all aware of ASP.NET, and they can all launch and debug ASP.NET applications.

Second, you need to have a version of the .NET command-line tool. If you did not get it bundled with the IDE, follow the instructions for installing .NET at http://dot.net. This gives you a command-line tool called dotnet that you'll use to perform a number of different tasks involved with microservices, including restoring, building, and creating NuGet packages and running microservices. Throughout the book I will use the dotnet command line for these tasks, but if you prefer, you can also do most of them in your IDE.

Third, you'll need a tool for making HTTP requests. I recommend the REST client plug-in for Visual Studio Code, but Fiddler, Postman, and cURL are also good and popular tools. You can use any of these to follow along with the examples in this book.

Fourth, you need Docker installed, and you need to have Kubernetes turned on in your Docker settings.

NOTE In appendix A, you'll find download, installation, and quick usage information for Visual Studio, Visual Studio Code, JetBrains Rider, and Docker. Now is the time to set up the tools of your choice—you'll need them throughout the rest of the book.

# 1.9 A simple microservices example

Once you have a development environment up and running, it's time for a “Hello World”-style microservices example. You'll use ASP.NET to create a microservice that