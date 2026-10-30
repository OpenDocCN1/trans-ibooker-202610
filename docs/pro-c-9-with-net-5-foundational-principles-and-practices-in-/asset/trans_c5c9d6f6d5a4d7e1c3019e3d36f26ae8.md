
要渲染此视图，请在 Controllers 文件夹中添加一个名为 CarsController 的新控制器。将代码更新为以下内容（本章后面会更新此代码）：

```
using Microsoft.AspNetCore.Mvc;

namespace AutoLot.Mvc.Controllers
{
    [Route("[controller]/[action]")]
    public class CarsController : Controller
    {
        public IActionResult Create()
        {
            return View();
        }
    }
}
```

现在运行应用程序并导航到 http://localhost:5001/Cars/Create。检查源代码会发现，表单的 action 属性基于 asp-controller 和 asp-action，method 被设置为 post，并且 `__RequestVerificationToken` 被添加为隐藏的表单输入。

```
<form action="/Cars/Create" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="CfDJ8Hqg5HsrvCt0kkLRHY4ukxwvix0vkQ3vOvezvtJWdIOP5IwbI5-FFWXh8KCFZo7eKxveCuK8NRJywj8Jz23pP2nV37fIGqqcITRyISGgq7tRYZD uPv8NMIY z2nCWRiDbx0vlkg61DTDW9BrJxr8H63Y">
</form>
```

Create 视图将在本章中持续更新。

## 表单操作标签助手

表单操作标签助手用于按钮和图像，用于更改包含它们的表单的操作。例如，添加到编辑表单中的以下按钮将导致 POST 请求发送到 Create 端点：

```
<button type="submit" asp-action="Create">Index</button>
```

## 锚点标签助手

锚点标签助手取代了 Html.ActionLink HTML 助手。例如，要为 RazorSyntax 视图创建链接，请使用以下代码：

```
<a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="RazorSyntax">
    Razor Syntax
</a>
```

要将 Razor 语法页面添加到菜单中，请将 _Menu.cshtml 更新为以下内容，在 Home 和 Privacy 菜单项之间添加新的菜单项（锚点标签周围的 `<li>` 标签是为 Bootstrap 菜单准备的）：

```
...
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
</li>
```

```
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="RazorSyntax">Razor Syntax</a>
</li>
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
</li>
```

## 输入标签助手

`<input>` 标签助手是最通用的标签助手之一。除了自动生成 HTML id 和 name 属性，以及任何 HTML5 data-val 验证属性之外，该标签助手还会根据目标属性的数据类型构建适当的 HTML 标记。表 31-3 列出了根据属性的 .NET Core 类型所创建的 HTML 类型。

表 31-3. 使用输入标签助手从 .NET 类型生成的 HTML 类型

| .NET 类型 | 生成的 HTML 类型 |
|---|---|
| Bool | type="checkbox" |
| String | type="text" |
| DateTime | type="datetime" |
| Byte, Int, Single, Double | type="number" |

此外，输入标签助手会根据数据注解添加 HTML5 类型属性。表 31-4 列出了一些最常见的注解以及生成的 HTML5 类型属性。

表 31-4. 从 .NET 数据注解生成的 HTML5 类型属性

| .NET 数据注解 | 生成的 HTML5 类型属性 |
|---|---|
| EmailAddress | type="email" |
| Url | type="url" |
| HiddenInput | type="hidden" |
| Phone | type="tel" |
| DataType(DataType.Password) | type="password" |
| DataType(DataType.Date) | type="date" |
| DataType(DataType.Time) | type="time" |

Car.cshtml 编辑器模板包含用于 PetName 和 Color 属性的 `<input>` 标签。提醒一下，这里只列出了这些标签：

```
<input asp-for="PetName" class="form-control" />
<input asp-for="Color" class="form-control"/>
```

输入标签助手会向渲染后的标签添加 name 和 id 属性、该属性的现有值（如果有的话）以及 HTML5 验证属性。这两个字段都是必填项，并且字符串长度限制为 50。以下是这两个属性的渲染标记：

```
<input class="form-control" type="text" data-val="true" data-val-length="The field Pet Name must be a string with a maximum length of 50." data-val-length-max="50" data-val-required="The Pet Name field is required." id="PetName" maxlength="50" name="PetName" value="Zippy">
```

```
<input class="form-control valid" type="text" data-val="true" data-val-length="The field Color must be a string with a maximum length of 50." data-val-length-max="50" data-val-required="The Color field is required." id="Color" maxlength="50" name="Color" value="Black" aria-describedby="Color-error" aria-invalid="false">
```

## 文本区域标签助手

`<textarea>` 标签助手会自动添加 id 和 name 属性，以及为该属性定义的任何 HTML5 验证标签。例如，以下代码行为 Description 属性创建了一个 textarea 标签：

```
<textarea asp-for="Description"></textarea>
```

## 选择标签助手

`<select>` 标签助手从模型属性和集合构建输入选择标签。与其他输入标签助手一样，id 和 name 会添加到标记中，以及任何 HTML5 data-val 属性。如果模型属性值与某个选择列表项的值匹配，则该选项会获得 selected 属性并添加到标记中。

例如，假设一个模型有一个名为 Country 的属性和一个名为 Countries 的 SelectList，列表定义如下：

```
public List<SelectListItem> Countries { get; } = new List<SelectListItem>
{
    new SelectListItem { Value = "MX", Text = "Mexico" },
    new SelectListItem { Value = "CA", Text = "Canada" },
    new SelectListItem { Value = "US", Text = "USA" },
};
```

以下标记将渲染带有相应选项的 select 标签：

```
<select asp-for="Country" asp-items="Model.Countries"></select>
```

如果 Country 属性的值设置为 CA，则会将以下完整标记输出到视图中：

```
<select id="Country" name="Country">
    <option value="MX">Mexico</option>
    <option selected="selected" value="CA">Canada</option>
    <option value="US">USA</option>
</select>
```

## 验证标签助手

验证消息和验证摘要标签助手与 Html.ValidationMessageFor 和 Html.ValidationSummaryFor HTML 助手非常相似。前者应用于模型中特定属性的 HTML span，后者应用于 div 标签并代表整个模型。验证摘要可以选择 All 错误、ModelOnly（不包括模型属性上的错误）或 None。
回顾一下 Car.cshtml 文件的 EditorTemplate 中的验证标签助手（此处以粗体显示）：

```
<div asp-validation-summary="All" class="text-danger"></div>
<div class="form-group">
<label asp-for="PetName" class="col-form-label"></label>
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

这些助手将显示来自绑定和验证的 ModelState 错误，如图 31-3 所示。

## 创建新汽车

- 宠物名字段是必填项。
- 颜色字段是必填项。

![](img/587021f4d5ba039be70a11107130377b_1314_0.png)

图 31-3. 验证标签助手的实际效果

## 环境标签助手

`<environment>` 标签助手通常用于根据站点运行的环境，有条件地加载 JavaScript 和 CSS 文件（或任何标记）。打开 _Head.cshtml 分部视图，并将标记更新为以下内容：

```
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>@ViewData["Title"] - AutoLot.Mvc</title>
<environment include="Development">
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
</environment>
<environment exclude="Development">
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
</environment>
<link rel="stylesheet" href="~/css/site.css" />
```

第一个 `<environment>` 标签助手使用 include="Development" 属性，在环境设置为 Development 时包含所包含的文件。在前面的代码中，加载的是 Bootstrap 的非压缩版本。第二个标签助手使用 exclude="Development" 在环境为 Development 之外的任何环境时使用所包含的文件，并加载 bootstrap.css 的压缩版本。site.css 文件在开发环境和非开发环境之间不会变化，因此它被列在 `<environment>` 标签助手之外。

另外，将 _JavaScriptFiles.cshtml 分部视图更新为以下内容（注意 Development 部分中的文件不再具有 .min 扩展名）：

```
<environment include="Development">
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.js"></script>
</environment>
<environment exclude="Development">
<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
</environment>
<script src="~/js/site.js" asp-append-version="true"></script>
```

## 链接标签助手

`<link>` 标签助手拥有用于本地和远程文件的属性。asp-append-version 属性与本地文件一起使用时，会将文件的哈希值作为查询字符串参数添加到发送到浏览器的 URL 中。当文件发生变化时，哈希值也会变化，从而更新发送到浏览器的 URL。由于链接发生了变化，浏览器会清除该文件的缓存并重新加载它。将 _Head.cshtml 文件中的 bootstrap.css 和 site.css 链接标签更新为以下内容：

```
<environment include="Development">
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" asp-append-version="true"/>
</environment>
<environment exclude="Development">
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
</environment>
<link rel="stylesheet" href="~/css/site.css" asp-append-version="true"/>
```

