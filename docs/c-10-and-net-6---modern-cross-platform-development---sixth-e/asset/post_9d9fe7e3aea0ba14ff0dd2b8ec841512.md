
## ASP.NET Core 3.0

ASP.NET Core 3.0 was released in September 2019 and focused on fully leveraging .NET Core 3.0 and .NET Standard 2.1, which meant it could not support .NET Framework, and it added useful refinements, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| Static assets in Razor class libraries | 14 | Using Razor class libraries |
| New options for MVC service registration | 15 | Understanding ASP.NET Core MVC startup |
| ASP.NET Core gRPC | 18 | Building services using ASP.NET Core gRPC |
| Blazor Server | 17 | Building components using Blazor Server |

## ASP.NET Core 3.1

ASP.NET Core 3.1 was released in December 2019 and is an LTS release, meaning it will be supported until December 13, 2022. It focused on refinements like partial class support for Razor components and a new <component> tag helper.

## Blazor WebAssembly 3.2

Blazor WebAssembly 3.2 was released in May 2020. It was a Current release, meaning that projects had to be upgraded to the .NET 5 version within three months of the .NET 5 release, that is, by February 10, 2021. Microsoft finally delivered on the promise of full-stack web development with .NET, and both Blazor Server and Blazor WebAssembly are covered in Chapter 17, Building User Interfaces Using Blazor.

## ASP.NET Core 5.0

ASP.NET Core 5.0 was released in November 2020 and focused on bug fixes, performance improvements using caching for certificate authentication, HPACK dynamic compression of HTTP/2 response headers in Kestrel, nullable annotations for ASP.NET Core assemblies, and a reduction in container image sizes, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| Extension method to allow anonymous access to an endpoint | 16 | Securing web services |
| JSON extension methods for HttpRequest and HttpResponse | 16 | Getting customers as JSON in the controller |

## ASP.NET Core 6.0

ASP.NET Core 6.0 was released in November 2021 and focused on productivity improvements like minimizing code to implement basic websites and services, .NET Hot Reload, and new hosting options for Blazor, like hybrid apps using .NET MAUI, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| New empty web project template | 14 | Understanding the empty web template |
| HTTP logging middleware | 16 | Enabling HTTP logging |
| Minimal APIs | 16 | Implementing minimal Web APIs |
| Blazor error boundaries | 17 | Defining Blazor error boundaries |
| Blazor WebAssembly AOT | 17 | Enabling Blazor WebAssembly ahead-of-time compilation |
| .NET Hot Reload | 17 | Fixing code using .NET Hot Reload |
| .NET MAUI Blazor apps | 19 | Hosting Blazor components in .NET MAUI apps |

## Building Windows-only desktop apps

Technologies for building Windows-only desktop apps include:

- **Windows Forms**, 2002.
- **Windows Presentation Foundation (WPF)**, 2006.
- **Windows Store** apps, 2012.
- **Universal Windows Platform (UWP)** apps, 2015.
- **Windows App SDK** (formerly **WinUI 3** and **Project Reunion**) apps, 2021.

## Understanding legacy Windows application platforms

With the Microsoft Windows 1.0 release in 1985, the only way to create Windows applications was to use the C language and call functions in three core DLLs named kernel, user, and GDI. Once Windows became 32-bit with Windows 95, the DLLs were suffixed with 32 and became known as **Win32 API**.

In 1991, Microsoft introduced Visual Basic, which provided developers with a visual, drag-and-drop-from-a-toolbox-of-controls way to build the user interface for Windows applications. It was immensely popular, and the Visual Basic runtime is still distributed as part of Windows 10 today.

With the first version of C# and .NET Framework released in 2002, Microsoft provided technology for building Windows desktop applications named **Windows Forms**. The equivalent at the time for web development was named **Web Forms**, hence the complimentary names. The code could be written in either Visual Basic or C# languages. Windows Forms had a similar drag-and-drop visual designer, although it generated C# or Visual Basic code to define the user interface, which can be difficult for humans to understand and edit directly.

In 2006, Microsoft released a more powerful technology for building Windows desktop applications, named **Windows Presentation Foundation (WPF)**, as a key component of .NET Framework 3.0 alongside **Windows Communication Foundation (WCF)** and **Windows Workflow (WF)**.

Although a WPF app can be created by writing only C# statements, it can also use **eXtensible Application Markup Language (XAML)** to specify its user interface, which is easy for both humans and code to understand. Visual Studio for Windows is partially built with WPF.

In 2012, Microsoft released Windows 8 with its Windows Store apps that run in a protected sandbox.

In 2015, Microsoft released Windows 10 with an updated Windows Store app concept named **Universal Windows Platform (UWP)**. UWP apps can be built using C++ and the DirectX UI, or JavaScript and HTML, or C# using a custom fork of modern .NET that is not cross-platform but provides full access to the underlying WinRT APIs.

UWP apps can only execute on the Windows 10 platform, not earlier versions of Windows, but UWP apps can run on Xbox and Windows Mixed Reality headsets with motion controllers.

Many Windows developers rejected Windows Store and UWP apps because they have limited access to the underlying system. Microsoft recently created **Project Reunion** and **WinUI 3**, which work together to allow Windows developers to bring some of the benefits of modern Windows development to their existing WPF apps and allow them to have the same benefits and system integrations that UWP apps have. This initiative is now known as **Windows App SDK**.

## Understanding modern .NET support for legacy Windows platforms

The on-disk size of the .NET SDKs for Linux and macOS is about 330 MB. The on-disk size of the .NET SDK for Windows is about 440 MB. This is because it includes the Windows Desktop Runtime, which allows the legacy Windows application platforms Windows Forms and WPF to be run on modern .NET.

There are many enterprise applications built using Windows Forms and WPF that need to be maintained or enhanced with new features, but until recently they were stuck on .NET Framework, which is now a legacy platform. With modern .NET and its Windows Desktop Pack, these apps can now use the full modern capabilities of .NET.

## Structuring projects

How should you structure your projects? So far, we have built small individual console apps to illustrate language or library features. In the rest of this book, we will build multiple projects using different technologies that work together to provide a single solution.

With large, complex solutions, it can be difficult to navigate amongst all the code. So, the primary reason to structure your projects is to make it easier to find components. It is good to have an overall name for your solution or workspace that reflects the application or solution.

We will build multiple projects for a fictional company named **Northwind**. We will name the solution or workspace **PracticalApps** and use the name **Northwind** as a prefix for all the project names.

There are many ways to structure and name projects and solutions, for example, using a folder hierarchy as well as a naming convention. If you work in a team, make sure you know how your team does it.

## Structuring projects in a solution or workspace

It is good to have a naming convention for your projects in a solution or workspace so that any developer can tell what each one does instantly. A common choice is to use the type of project, for example, class library, console app, website, and so on, as shown in the following table:

| Name | Description |
|---|---|
| Northwind.Common | A class library project for common types like interfaces, enums, classes, records, and structs, used across multiple projects. |
| Northwind.Common.EntityModels | A class library project for common EF Core entity models. Entity models are often used on both the server and client side, so it is best to separate dependencies on specific database providers. |
| Northwind.Common.DataContext | A class library project for the EF Core database context with dependencies on specific database providers. |
| Northwind.Web | An ASP.NET Core project for a simple website that uses a mixture of static HTML files and dynamic Razor Pages. |
| Northwind.Razor.Component | A class library project for Razor Pages used in multiple projects. |
| Northwind.Mvc | An ASP.NET Core project for a complex website that uses the MVC pattern and can be more easily unit tested. |
| Northwind.WebApi | An ASP.NET Core project for an HTTP API service. A good choice for integrating with websites because they can use any JavaScript library or Blazor to interact with the service. |
| Northwind.OData | An ASP.NET Core project for an HTTP API service that implements the OData standard to enable a client to control queries. |
| Northwind.GraphQL | An ASP.NET Core project for an HTTP API service that implements the GraphQL standard to enable a client to control queries. |
| Northwind.gRPC | An ASP.NET Core project for a gRPC service. A good choice for integrating with apps built with any language and platform since gRPC has wide support and is highly efficient and performant. |
| Northwind.SignalR | An ASP.NET Core project for real-time communication. |
| Northwind.AzureFuncs | An ASP.NET Core project for implementing a serverless nanoservice for hosting in Azure Functions. |
| Northwind.BlazorServer | An ASP.NET Core Blazor Server project. |
| Northwind.BlazorWasm.Client | An ASP.NET Core Blazor WebAssembly client-side project. |
| Northwind.BlazorWasm.Server | An ASP.NET Core Blazor WebAssembly server-side project. |
| Northwind.Maui | A .NET MAUI project for a cross-platform desktop/mobile app. |
| Northwind.MauiBlazor | A .NET MAUI project for hosting Blazor components with native integrations with the OS. |
