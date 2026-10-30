

.NET Framework 提供了多种选择，包括 WebForms、MVC、Web API、Windows Communication Foundation (WCF) 和 WebMatrix。它们各有利弊；有些关系密切，有些则截然不同。所有可用的选择都意味着开发人员必须了解它们中的每一个，以便为手头的任务选择合适的选项，或者只是选择一个然后希望一切顺利。

使用 ASP.NET Core，你可以构建使用 Razor Pages、Model-View-Controller 模式、RESTful 服务以及使用 Angular 和 React 等 JavaScript 框架的 SPA 应用的应用程序。虽然 UI 渲染会因 MVC、Razor Pages 和 JavaScript 框架之间的选择而有所不同，但底层开发框架在所有选择中都是相同的。两个未被带入 ASP.NET Core 的先前选择是 WebForms 和 WCF。

> 注意 将所有独立的框架统一在同一屋檐下后，原 ASP.NET MVC 和 ASP.NET Web API 的名称已被正式退役。在本书中，我仍然将使用 Model-View-Controller 模式的 ASP.NET Core Web 应用程序称为 MVC，并将 ASP.NET RESTful 服务称为 Web API，以求简便。

## 来自 MVC/Web API 的 ASP.NET Core 功能

许多促使开发人员使用 ASP.NET MVC 和 ASP.NET Web API 的设计目标和功能在 ASP.NET Core 中仍然得到支持（并且有所改进）。其中一些（但不是全部）列于此处：

- 约定优于配置
- 控制器和动作
- 模型绑定
- 模型验证
- 路由
- 过滤器
- 布局和 Razor 视图

这些将在下一节中介绍，除了布局和 Razor 视图将在第 31 章中介绍。

## 约定优于配置

ASP.NET MVC 和 ASP.NET Web API 通过引入某些约定减少了必要的配置量。遵循这些约定可减少手动（或模板化）配置量，但也要求开发人员了解这些约定才能利用它们。其中两个主要约定包括命名约定和目录结构。

## 命名约定

ASP.NET Core 中存在多个命名约定，既适用于 MVC 也适用于 API 应用。例如，控制器通常以 Controller 后缀命名（例如 HomeController），并且派生自 Controller（或 ControllerBase）。通过路由访问时，会去掉 Controller 后缀。在查找控制器的视图时，去掉后缀的控制器名称是开始搜索的位置。

去掉后缀的约定在 ASP.NET Core 中反复出现。以下章节中将有许多示例。

另一个命名约定用于视图位置和选择。默认情况下，动作方法（在 MVC 应用中）将渲染与方法同名的视图。编辑器和显示模板以它们在视图中渲染的类命名。这些默认值如果应用程序需要可以更改。所有这些将在构建 AutoLot.Mvc 应用程序时进一步探讨。

## 目录结构

你必须理解几个文件夹约定才能成功构建 ASP.NET Core Web 应用程序和服务。

## Controllers 文件夹

按照约定，Controllers 文件夹是 ASP.NET Core MVC 和 API 实现（以及路由引擎）期望放置应用程序控制器的位置。

## Views 文件夹

Views 文件夹是应用程序视图的存储位置。每个控制器在主 Views 文件夹下都有自己的文件夹，以控制器名称（去掉 Controller 后缀）命名。动作方法默认在其控制器的文件夹中渲染视图。例如，Views/Home 文件夹保存 HomeController 控制器类的所有视图。

## Shared 文件夹

Views 下的一个特殊文件夹名为 Shared。该文件夹可供所有控制器及其动作方法访问。在按控制器命名的文件夹中搜索后，如果找不到视图，则会在 Shared 文件夹中搜索视图。

## wwwroot 文件夹（ASP.NET Core 中的新功能）

ASP.NET MVC 的改进之一是为 ASP.NET Core Web 应用程序创建了一个名为 wwwroot 的特殊文件夹。在 ASP.NET MVC 中，JavaScript 文件、图像、CSS 和其他客户端内容与所有其他文件夹混杂在一起。在 ASP.NET Core 中，客户端内容全部包含在 wwwroot 文件夹下。这种将编译文件与客户端文件分离的做法显著清理了使用 ASP.NET Core 时的项目结构。

## 控制器和动作

就像 ASP.NET MVC 和 ASP.NET Web API 一样，控制器和动作方法是 ASP.NET Core MVC 或 API 应用程序的支柱。

## Controller 类

如前所述，ASP.NET Core 统一了 ASP.NET MVC5 和 ASP.NET Web API。这种统一还将 MVC5 和 Web API 2.2 中的 Controller、ApiController 和 AsyncController 基类合并为一个新类 Controller，其自身基类名为 ControllerBase。ASP.NET Core Web 应用程序控制器继承自 Controller 类，而 ASP.NET Core 服务控制器继承自 ControllerBase 类（稍后介绍）。

Controller 类为 Web 应用程序提供了一系列帮助方法。表 29-1 列出了最常用的方法。

## 表 29-1 Controller 类提供的一些帮助方法

| Helper Method | Meaning in Life |
|---|---|
| ViewData TempData ViewBag | 通过 ViewDataDictionary、TempDataDictionary 和动态 ViewBag 传输向视图提供数据。 |
| View | 返回 ViewResult（派生自 ActionResult）作为 HTTP 响应。默认为与动作方法同名的视图，可选择指定特定视图。所有选项都允许指定一个强类型并发送到 View 的视图模型。视图在第 31 章中介绍。 |
| PartialView | 向响应管道返回 PartialViewResult。局部视图在第 31 章中介绍。 |
| ViewComponent | 向响应管道返回 ViewComponentResult。ViewComponents 在第 31 章中介绍。 |
| Json | 返回包含作为响应序列化为 JSON 的对象的 JsonResult。 |
| OnActionExecuting | 在动作方法执行前执行。 |
| OnActionExecutionAsync | OnActionExecuting 的异步版本。 |
| OnActionExecuted | 在动作方法执行后执行。 |

## ControllerBase 类

ControllerBase 类为 ASP.NET Core Web 应用程序和服务提供核心功能，以及返回 HTTP 状态码的帮助方法。表 29-2 列出了 ControllerBase 中的一些核心功能，表 29-3 涵盖了返回 HTTP 状态码的一些帮助方法。

## 表 29-2 ControllerBase 类提供的一些帮助方法

| Helper Method | Meaning in Life |
|---|---|
| HttpContext | 返回当前执行动作的 HttpContext。 |
| Request | 返回当前执行动作的 HttpRequest。 |
| Response | 返回当前执行动作的 HttpResponse。 |
| RouteData | 返回当前执行动作的 RouteData（路由将在本章后面介绍）。 |
| ModelState | 返回模型关于模型绑定和验证的状态（两者都将在本章后面介绍）。 |
| Url | 返回 IUrlHelper 的实例，提供构建 ASP.NET Core MVC 应用程序和服务的 URL 的访问权限。 |

## 表 29-2.（续）

| Helper Method | Meaning in Life |
|---|---|
| User | 返回 ClaimsPrincipal 用户。 |
| Content | 向响应返回 ContentResult。重载允许添加内容类型和编码定义。 |
| File | 向响应返回 FileContentResult。 |
| Redirect | 一系列方法通过返回 RedirectResult 将用户重定向到另一个 URL。 |
| LocalRedirect | 一系列方法仅在 URL 为本地时将用户重定向到另一个 URL。比通用 Redirect 方法更安全。 |
| RedirectToAction
RedirectToPage
RedirectToRoute | 一系列方法重定向到另一个动作方法、Razor 页面或命名路由。路由将在本章后面介绍。 |
| TryUpdateModel | 显式模型绑定（将在本章后面介绍）。 |
| TryValidateModel | 显式模型验证（将在本章后面介绍）。 |

## 表 29-3 ControllerBase 类提供的一些 HTTP 状态码帮助方法

| Helper Method | HTTP Status Code Action Result | Status Code |
|---|---|---|
| NoContent | NoContentResult | 204 |
| Ok | OkResult | 200 |
| NotFound | NotFoundResult | 404 |
| BadRequest | BadRequestResult | 400 |
| Created
CreatedAtAction
CreatedAtRoute | CreatedResult
CreatedAtActionResult
CreateAtRouteResult | 201 |
| Accepted
AcceptedAtAction
AcceptedAtRoute | AcceptedResult
AcceptedAtActionResult
AcceptedAtRouteResult | 202 |

## 动作

动作是控制器上的方法，返回 IActionResult（或异步操作的 Task<IActionResult>）或实现 IActionResult 的类，例如 ActionResult 或 ViewResult。动作将在以下章节中更详细地介绍。

## 模型绑定

模型绑定是 ASP.NET Core 使用 HTTP Post 调用中提交的名称-值对为模型分配值的过程。要绑定到引用类型，名称-值对来自表单值或请求体，引用类型必须具有公共默认构造函数，并且要绑定的属性必须是公共且可写的。在分配值时，隐式类型转换（例如设置字符串

