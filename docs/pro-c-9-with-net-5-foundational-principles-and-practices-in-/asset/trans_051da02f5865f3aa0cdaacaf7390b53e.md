
```
{
    return View();
}
}
```

> 注意 以控制器命名的视图文件夹会先被搜索。如果找不到视图，则会搜索 Shared 文件夹。如果仍然找不到，则会抛出异常。

要渲染一个与操作方法名称不同的视图，请传入文件名（不带 cshtml 扩展名）。以下代码将渲染 CustomViewName.cshtml 视图：

```
public ActionResult SampleAction()
{
    return View("CustomViewName");
}
```

最后两个重载用于传入一个成为视图模型的数据对象。第一个示例使用默认视图名称，第二个示例指定了不同的视图名称。

```
public ActionResult SampleAction()
{
    var sampleModel = new SampleActionViewModel();
    return View(sampleModel);
}
public ActionResult SampleAction()
{
    var sampleModel = new SampleActionViewModel();
    return View("CustomViewName",sampleModel);
}
```

下一节将使用从 HomeController 上的 RazorSyntax() 操作方法渲染的视图，详细探讨 Razor 视图引擎。操作方法将从注入到该方法中的 CarRepo 类实例获取一条 Car 记录，并将 Car 实例作为模型传递给视图。

打开 AutoLot.Mvc 应用的 Controllers 目录中的 HomeController 并添加以下 using 语句：

```
using AutoLot.Dal.Repos.Interfaces;
```

接下来，将 RazorSyntax() 方法添加到控制器。

```
[HttpGet]
public IActionResult RazorSyntax([FromServices] ICarRepo carRepo)
{
    var car = carRepo.Find(1);
    return View(car);
}
```

操作方法使用 HTTPGet 属性装饰，将此方法设置为 /Home/RazorSyntax 的应用程序端点，只要请求是 HTTP Get。ICarRepo 参数上的 FromServices 属性告知 ASP.NET Core 参数不应使用任何传入数据进行数据绑定，而是方法从依赖注入容器接收 ICarRepo 的实例。该方法获取一个 Car 实例并使用 View 方法返回一个 ViewResult。由于没有命名视图，ASP.NET Core 将在 Views\Home 目录或 Views\Shared 目录中查找名为 RazorSyntax.cshtml 的视图。如果在任一位置都找不到视图，则会向客户端（浏览器）返回异常。

运行应用程序并在浏览器中导航到 https://localhost:5001/Home/RazorSyntax（如果您使用 Visual Studio 和 IIS，则需要更新端口）。由于项目中没有视图可以满足请求，因此会向浏览器返回异常。回想一下第 29 章，Startup 类的 Configure() 方法如果环境为 Development，则会在 HTTP 管道中添加 UseDeveloperExceptionPage() 方法。图 31-1 显示了此方法的效果。

![](img/587021f4d5ba039be70a11107130377b_1294_0.png)

图 31-1. 来自开发人员异常页面的错误消息

开发人员异常页面提供了充足的信息来调试您的应用程序，包括完整的堆栈跟踪的原始异常详细信息。现在，在 Configure() 方法中注释掉该行，用“标准”错误处理程序替换它，如下所示：

```
if (env.IsDevelopment())
{
    //app.UseDeveloperExceptionPage();
    app.UseExceptionHandler("/Home/Error");
    ...
}
```

再次运行应用程序并导航到 http://localhost:5001/Home/RazorSyntax，您将看到标准错误页面，如图 31-2 所示。

![](img/587021f4d5ba039be70a11107130377b_1295_0.png)

图 31-2. 来自标准错误页面的错误消息

注意 本章中的示例 URL 全部使用 Kestrel 和端口 5001。如果您使用 Visual Studio 配合 IIS Express，请使用 launchsettings.json 配置文件中的 URL。

标准错误处理程序将错误重定向到 HomeController 的 Error 操作方法。请记住将 Configure() 方法改回使用开发人员异常页面：

```
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    ...
}
```

有关自定义错误处理和更多可用选项的更多信息，请参阅文档：https://docs.microsoft.com/en-us/aspnet/core/fundamentals/error-handling?view=aspnetcore-5.0。

## Razor 视图引擎和 Razor 语法

Razor 视图引擎是作为对 Web Forms 视图引擎的改进而设计的，并使用 Razor 作为核心语言。Razor 是嵌入到视图中的服务器端代码，基于 C#，并纠正了 Web Forms 视图引擎的许多烦恼。将 Razor 整合到 HTML 和 CSS 中会产生比使用 Web Forms 视图引擎语法更干净、更易读的代码。

开始使用时，在 AutoLot.Mvc 项目中右键单击 Views\Home 文件夹并选择 Add ► New Item。从 Add New Item - AutoLot.Mvc 对话框中选择 Razor View - Empty，并将视图命名为 RazorSyntax.cshtml。

注意 当您右键单击 Views\Home 文件夹时，还有一个菜单选项：Add ► View。但是，该对话框会直接带您返回 Add New Item 对话框。

Razor 视图通常通过使用 @model 指令进行强类型（注意 m 为小写）。通过在视图顶部添加以下内容，将新视图的类型更新为 Car 实体：

```
@model AutoLot.Models.Entities.Car
```

在页面顶部添加一个 <h1> 标签。这与 Razor 无关；它只是为页面添加一个标题。

```
<h1>Razor Syntax</h1>
```

Razor 语句块以 @ 符号开头，要么是自包含的语句（如 foreach），要么像以下示例那样用大括号括起来：

```
@for (var i = 0; i < 15; i++)
{
    //do something
}
```

```
@{
    //Code Block
    var foo = "Foo";
    var bar = "Bar";
    var htmlString = "<ul><li>one</li><li>two</li></ul>";
}
```

要将变量的值输出到视图，只需使用 @ 符号加上变量名。这相当于 Response.Write()。请注意，直接输出到浏览器时语句后没有分号。

```
@foo
<br />
@htmlString
<br />
@foo.@bar
<br />
```

前面的示例（@foo.@bar）使用句点（.）在两个变量之间进行组合。这不是通常的 C#“点”符号，即导航属性链。它只是将两个变量的输出与物理句点一起写入响应流。如果您需要“点”方式向下遍历变量，请在变量上使用 @，然后像平常一样编写代码。

```
@foo.ToUpper()
```

如果您想输出原始 HTML，则使用所谓的 HTML 助手。这些是内置于 Razor 视图引擎的助手。以下是输出原始 HTML 的行：

```
@Html.Raw(htmlString)
<hr />
```

代码块可以混合标记和代码。以标记开头的行被解释为 HTML，而所有其他行被解释为代码。如果一行以非代码的文本开头，您必须使用内容指示符（@:）或 <text></text> 内容块指示符。请注意，行可以在两者之间来回转换。以下是示例：

```
@{
    @:Straight Text
    <div>Value:@Model.Id</div>
    <text>
        Lines without HTML tag
    </text>
    <br />
}
```

如果您想转义 @ 符号，请使用双 @。Razor 也足够智能，可以处理电子邮件地址，因此无需转义。如果您需要 Razor 将 @ 符号视为 Razor 标记，请添加括号。

```
foo@foo.com
<br />
@@foo
<br />
test@foo
<br/>
test@(foo)
<br />
```

前面的代码分别输出 foo@foo.com、@foo、test@foo 和 testFoo。
Razor 注释以 @* 开头，以 *@ 关闭。

```
@*
    Multiline Comments
    Hi.
*@
```

Razor 也支持内联函数。以下示例函数对字符串列表进行排序：

```
@functions {
    public static IList<string> SortList(IList<string> strings) {
        var list = from s in strings orderby s select s;
        return list.ToList();
    }
}
```

以下代码创建一个字符串列表，使用 SortList() 函数对其进行排序，然后将排序后的列表输出到浏览器：

```
@{
    var myList = new List<string> {"C", "A", "Z", "F"};
    var sortedList = SortList(myList);
}
```
