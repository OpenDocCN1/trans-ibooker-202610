
```
<input asp-for="PetName" class="form-control" />
<span asp-validation-for="PetName" class="text-danger"></span>
</div>
<div class="form-group">
<label asp-for="MakeId" class="col-form-label"></label>
<select asp-for="MakeId" class="form-control" asp-items="ViewBag.MakeId"></select>
</div>
<div class="form-group">
<label asp-for="Color" class="col-form-label"></label>
<input asp-for="Color" class="form-control"/>
<span asp-validation-for="Color" class="text-danger"></span>
</div>
```

编辑器模板使用了几个标签助手（asp-for、asp-items、asp-validation-for 和 asp-validation-summary）。这些将在本章后面介绍。

该模板由 EditorFor()/EditorForModel() HTML 助手调用。与显示模板一样，这些方法会查找名为 Car.cshtml 的视图，或查找方法中指定的视图。

## 布局

与 Web Forms 母版页类似，MVC 支持在视图之间共享的布局，使站点页面具有一致的外观和感觉。导航到 Views\Shared 文件夹，打开 _Layout.cshtml 文件。这是一个完整的 HTML 文件，包含 <head> 和 <body> 标签。

该文件是其他视图渲染到其中的基础。此外，由于页面的大部分内容（例如导航以及任何页眉和/或页脚标记）由布局页面处理，因此视图页面保持小而简单。在文件中向下滚动，直到看到以下 Razor 代码行：

```
@RenderBody()
```

该行指示布局页面在何处渲染视图。现在向下滚动到结束 </body> 标签之前的一行。以下行为布局创建了一个新的节点，并使其成为可选：

```
@await RenderSectionAsync("scripts", required: false)
```

节点也可以通过传入 true 作为第二个参数来标记为必需。它们也可以同步渲染，如下所示：

```
@RenderSection("Header",true)
```

视图文件中 @section 块中的任何代码和/或标记都不会通过 @RenderBody() 调用渲染，而是在布局的节点定义位置渲染。例如，假设您有一个视图，其节点实现如下：

```
@section Scripts {
<script src="~/lib/jquery-validation/dist/jquery.validate.js"></script>
}
```

视图中的代码会渲染到布局中，以代替节点定义。如果布局具有以下定义：

```
<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
<script src="~/js/site.js" asp-append-version="true"></script>
@await RenderSectionAsync("Scripts", required: false)
```

那么视图的节点会被添加，从而得到发送到浏览器的以下标记：

```
<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
<script src="~/js/site.js" asp-append-version="true"></script>
<script src="~/lib/jquery-validation/dist/jquery.validate.js"></script>
```

ASP.NET Core 中的两个新选项是 IgnoreBody 和 IgnoreSection。这些方法放置在布局中，将分别不渲染视图的主体或特定节点。这样可以根据条件逻辑（如安全级别）从布局中打开或关闭视图的功能。

### 为视图指定默认布局

如前所述，默认布局页面定义在 _ViewStart.cshtml 文件中。任何未指定布局的视图都将使用位于视图目录或上级目录中的第一个 _ViewStart.cshtml 文件中定义的布局。

### 分部视图

分部视图在概念上类似于 Web Forms 中的用户控件。分部视图有助于封装 UI，从而减少重复的代码和/或标记。分部视图不使用布局，而是注入到另一个视图中，或使用视图组件渲染（本章后面会介绍）。

### 更新布局和分部视图

有时布局文件会变得庞大而笨拙。一种管理方法是把布局拆分成一组有重点的分部视图。

### 创建分部视图

在 Shared 文件夹下创建一个名为 Partials 的新文件夹。创建三个空视图，分别命名为 _Head.cshtml、_JavaScriptFiles.cshtml 和 _Menu.cshtml。

### Head 分部视图

剪切布局中位于 <head></head> 标签之间的内容，并将其粘贴到 _Head.cshtml 文件中。

```
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>@ViewData["Title"] - AutoLot.Mvc</title>
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
<link rel="stylesheet" href="~/css/site.css" />
```

在 _Layout.cshtml 中，用渲染新分部视图的调用替换删除的标记：

```
<head>
    <partial name="Partials/_Head"/>
</head>
```

<partial> 标签是标签助手的另一个示例。name 属性是分部视图的名称，路径从视图的当前目录开始，在本例中为 Views\Shared。

### 菜单分部视图

对于菜单分部视图，剪切 <header></header> 标签之间的所有标记（不是 <head></head> 标签），然后粘贴到 _Menu.cshtml 文件中。更新 _Layout 以渲染 Menu 分部视图。

```
<header>
    <partial name="Partials/_Menu"/>
</header>
```

### JavaScript 文件分部视图

目前最后一步是剪切用于 JavaScript 文件的 <script> 标签，并将它们粘贴到 JavaScriptFiles 分部视图中。确保将 RenderSection 标签留在原处。以下是 JavaScriptFiles 分部视图：

```
<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
<script src="~/js/site.js" asp-append-version="true"></script>
```

以下是 _Layout.cshtml 文件当前的标记：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <partial name="Partials/_Head" />
</head>
<body>
    <header>
        <partial name="Partials/_Menu" />
    </header>
    <div class="container">
        <main role="main" class="pb-3">
            @RenderBody()
        </main>
    </div>

    <footer class="border-top footer text-muted">
        <div class="container">
            &copy; 2021 - Autolot.Mvc - <a asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
        </div>
    </footer>
```

```
<partial name="Partials/_JavaScriptFiles" />
@await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

## 向视图发送数据

有多种方式可以向视图发送数据。当视图是强类型时，可以在渲染视图时发送数据（无论是通过操作方法还是通过 <partial> 标签助手）。

### 强类型视图和视图模型

当模型或 ViewModel 被传入视图方法时，该值会被赋值给强类型视图的 @model 属性，如下所示（注意小写的 m）：

```
@model IEnumerable<Order>
```

@model 设置视图的类型，然后可以使用 @Model Razor 命令访问它，如下所示（注意大写的 M）：

```
@foreach (var item in Model)
{
    //Do something interesting here
}
```

RazorViewSyntax() 操作方法展示了视图从操作方法获取数据。

```
[HttpGet]
public IActionResult RazorSyntax([FromServices] ICarRepo carRepo)
{
    var car = carRepo.Find(1);
    return View(car);
}
```

可以通过 <partial> 传入模型值，如下所示：

```
<partial name="Partials/_CarListPartial" model="@Model"/>
```

### ViewBag、ViewData 和 TempData

ViewBag、ViewData 和 TempData 对象是向视图发送少量数据的机制。表 31-1 列出了从控制器向视图（除了 Model 属性）或从控制器向控制器传递数据的三种机制。

### 表 31-1. 向视图发送数据的其他方式

| 数据传输对象 | 使用说明 |
|---|---|
| TempData | 这是一个短生命周期对象，仅在当前请求和后续一次请求中有效。通常用于重定向到另一个操作方法时。 |
| ViewData | 一个字典，允许以名称-值对的形式存储值（例如，ViewData["Title"] = "My Page"）。 |
| ViewBag | ViewData 字典的动态包装器（例如，ViewBag.Title = "My Page"）。 |

ViewBag 和 ViewData 都指向同一个对象；它们只是提供了不同的数据访问方式。让我们再看一下之前创建的 _HeadPartial.cshtml 文件（重要行以粗体显示）：

```
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>@ViewData["Title"] - AutoLot.Mvc</title>
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
<link rel="stylesheet" href="~/css/site.css" />
```

您会注意到 <title> 属性使用 ViewData 来设置值。由于 ViewData 是 Razor 构造，它前面带有 @ 符号。要查看实际效果，请使用以下内容更新 RazorSyntax.cshtml 视图：

```
@model AutoLot.Models.Entities.Car
@{
    ViewData["Title"] = "RazorSyntax";
}
<h1>Razor Syntax</h1>
...
```

现在，当您运行应用程序并导航到 https://localhost:5001/Home/RazorSyntax 时，您将在浏览器选项卡中看到标题“Razor Syntax - AutoLot.Mvc”。

## 标签助手

标签助手是 ASP.NET Core 中引入的新功能。标签助手是一种标记（自定义标签或标准标签上的属性），表示服务器端代码。然后服务器端代码帮助塑造所生成的 HTML。它们极大地改善了 MVC 视图的开发体验和可读性。

与作为 Razor 方法调用的 HTML 助手不同，标签助手是添加到标准 HTML 元素或独立自定义标签中的属性。如果您使用 Visual Studio 进行开发，那么还有一个额外的好处：内置标签助手支持 IntelliSense。

例如，以下 HTML 助手为客户的 FullName 属性创建标签：

```
@Html.Label("FullName","Full Name:",new {@class="customer"})
```

