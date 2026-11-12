
This tool certainly gives an inkling of what may be possible with this technology; however, if you're not too familiar with C or Rust, you may struggle to use this beyond displaying 42 in the browser.

Fortunately, Microsoft is working on an experimental technology called Blazor that abstracts a lot of this work behind a familiar-looking project structure.

## Understanding Blazor

Blazor is, at the time of writing, around two years old (it was first demoed in 2017). The idea was to take the concept of WebAssembly and mix it in with existing concepts that web developers are familiar with, such as Razor.

> Part of the reason that this section is an appendix is that only server-side Blazor was released with .NET Core 3. While the client side is in a workable state, it is not in the official release at the time of writing.

There are two flavors of Blazor: client and server. Server-side Blazor was released with .NET Core 3. It works by running the C# code on the server and then sending screen updates to the client using SignalR. Whilst this is an interesting methodology, since it does not use WebAssembly, we won't discuss it further in this chapter.

To create a Blazor project, you'll need the Visual Studio plugin: https://marketplace.visualstudio.com/items?itemName=aspnet.blazor.

You'll also need to install the Visual Studio Templates for Blazor; from a command line, type the following:

```
dotnet new -i Microsoft.AspNetCore.Blazor.Templates
```

> By the time this book is published, this step may no longer be necessary.

We can now create a new Blazor project:

- 1. Create a new **Blazor App** project from Visual Studio 2019:

![](img/762492a8287f369cc1ac4621a212f193_506_0.png)

- 2. In the next screen, select your **Project name** and **Location**, as you would normally:

![](img/762492a8287f369cc1ac4621a212f193_507_0.png)

- 3. In the next screen, you should see several options. In this example, we're going to create a client-side Blazor application:

![](img/762492a8287f369cc1ac4621a212f193_507_1.png)

- 4. If you now select **Create**, you should be presented with a complete and working example of Blazor. Try running this: you should see a fully functional single-page application. If you've ever seen the React template, this structure should look very familiar to you.

> > While this is true at the time of writing, the template may have changed by the time you read this.

If you click around the application, it should feel a lot like a modern single-page application. Let's look a little deeper. We'll start with the project structure:

![](img/762492a8287f369cc1ac4621a212f193_508_0.png)

This structure should look quite familiar to you. It's worth noting the `.razor` extensions: in fact, the name **Blazor** comes from an amalgamation of the words **Browser** and **Razor**.

For this quick overview, we won't go too far into the details, but let's have a look at the **Counter** page; when you run the application, you should see a page that allows you to click a button and increase the value:

![](img/762492a8287f369cc1ac4621a212f193_509_0.png)

This page gives an excellent feel for exactly how the language works; let's have a look at the code that drives this. The `Counter.razor` file looks like this:

```
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="@IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

We have some HTML: the page header, the current count, and so on. Then, we have the Razor syntax—that's pretty much anywhere that you see an `at` symbol (@). Note that the button maps the `click` event handler to the method inside the `@code` block. Blazor keeps a track of the DOM, and when something changes—for example, the `currentCount`—the page is refreshed.

Let's run the application again, but this time, switch on the Developer Tools in the browser:

![](img/762492a8287f369cc1ac4621a212f193_510_0.png)

For most browsers, this can be achieved by pressing F12.

This tells us two things that are very important when dealing with Blazor. First, have a look at all of the DLLs that are being brought down to run the application. Essentially, this is downloading the Mono runtime. As you can see, the files aren't particularly big, but it is something to bear in mind when selecting the Blazor technology.

The second thing to note is that clicking the button does not make a server call. All of the code is executing on the client. This is the real draw of Blazor: you can write C# code and it runs in your browser.

We've had a quick look at WebAssembly in its raw form. We've discussed why WebAssembly is such an appealing, emergent web technology. We then had a look at the Blazor project to see how it simplifies the process of creating web applications.

## Other Books You May Enjoy

If you enjoyed this book, you may be interested in these other books by Packt:

![](img/762492a8287f369cc1ac4621a212f193_511_0.png)

## Hands-On Software Architecture with C# 8 and .NET Core 3

Gabriel Baptista, Francesco Abbruzzese

ISBN: 978-1-78980-093-7

- Overcome real-world architectural challenges and solve design consideration issues
- Apply architectural approaches like Layered Architecture, service-oriented architecture (SOA), and microservices
- Learn to use tools like containers, Docker, and Kubernetes to manage microservices
- Get up to speed with Azure Cosmos DB for delivering multi-continental solutions
- Learn how to program and maintain Azure Functions using C#
- Understand when to use test-driven development (TDD) as an approach for software development
- Write automated functional test cases for your projects

![](img/762492a8287f369cc1ac4621a212f193_512_0.png)

## Hands-On RESTful Web Services with ASP.NET Core 3

Samuele Resca

ISBN: 978-1-78953-761-1

- Gain a comprehensive working knowledge of ASP.NET Core
- Integrate third-party tools and frameworks to build maintainable and efficient services
- Implement patterns using dependency injection to reduce boilerplate code and improve flexibility
- Use ASP.NET Core's out-of-the-box tools to test your applications
- Use Docker to run your ASP.NET Core web service in an isolated and self-contained environment
- Secure your information using HTTPS and token-based authentication
- Integrate multiple web services using resiliency patterns and messaging techniques

## Leave a review - let other readers know what you think

Please share your thoughts on this book with others by leaving a review on the site that you bought it from. If you purchased the book from Amazon, please leave us an honest review on this book's Amazon page. This is vital so that other potential readers can see and use your unbiased opinion to make purchasing decisions, we can understand what our customers think about our products, and our authors can see your feedback on the title that they have worked with Packt to create. It will only take a few minutes of your time, but is valuable to other potential customers, our authors, and Packt. Thank you!

## Index

.NET Core 3
benefits 59
compilation errors, fixing 52
upgrading to 49, 50, 51, 52

### A

Access Azure Storage Client, using for Photo Storage App service, configuring 323, 324, 325
Android device, setup reference link 424
application running, on physical device 424, 425, 427, 428, 429, 431
testing, on physical device 424, 425, 427, 428, 429, 431
ASP.NET Core MVC application, connecting to Cosmos DB about 86, 87, 88
MongoDbRepository class, creating 96, 97
NuGet package, adding 89, 90, 92, 95, 96
ASP.NET Core Web Application, packages code generation design 158, 159
controller, creating 170, 171
database, creating 163, 164, 165, 167
database, seeding with test data 167, 168
EF Core SQL Server 158
EF Core tools 158
installing 157
models, creating 160, 161
services, configuring 161
ASP.NET Core Web Application, publishing options custom targets 191, 192
File System 195
FTP 193
Import Profile 196, 197
Microsoft Azure App Service 181, 183, 185, 186, 188, 190
Web Deploy 194
Web Deploy Package 195
ASP.NET Core Web Application arrange/act/assert 179
creating 154, 157
deploying 181
in-memory database 179
running 171, 173, 174
testing 174, 175, 176, 178, 179, 180
testing, with FIRST 180
authorization, Stock Checker Application API 294, 295, 296, 298, 299, 300, 301, 302, 303, 304
about 292
roles 292, 293
users 292, 293
Azure Cognitive Services configuring 403, 404, 405, 406, 407
Azure Container Registry (ACR) 385, 386, 387
Azure Cosmos DB account name 81
API 81
calculator, reference link 83
geo-redundancy/multi-region writes 82
location 82
reference link 76
setting up 76, 77, 79
subscription/resource group 79, 81
Azure Functions overview 199
Azure Kubernetes Service (AKS) about 347, 349, 384, 388, 389
Azure Container Registry (ACR) 385, 386, 387
container registry 385
Docker image, building 384
