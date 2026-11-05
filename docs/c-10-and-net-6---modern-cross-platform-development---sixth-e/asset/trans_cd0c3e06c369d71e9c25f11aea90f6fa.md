
5. 当主页出现产品列表时，点击其中一个，例如第二个产品，**Chang**。

6. 注意浏览器地址栏中的 URL 路径、浏览器标签页中显示的页面标题以及产品详情页，如图 15.5 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_680_0.png)

Figure 15.5: Chang 的产品详情页

7. 查看 **开发者工具**。

8. 编辑 Chrome 地址框中的 URL 以请求不存在的产品 ID，例如 99，并注意 404 Not Found 状态码和自定义错误响应。

## 更详细地了解模型绑定器

模型绑定器功能强大，默认的模型绑定器能为你做很多事情。在默认路由识别出要实例化的控制器类和要调用的操作方法后，如果该方法有参数，则需要为这些参数设置值。

模型绑定器通过查找 HTTP 请求中传递的参数值作为以下任一类型的参数来实现这一点：

- **路由参数**，如我们在上一节中所做的 id，如以下 URL 路径所示：`/Home/ProductDetail/2`
- **查询字符串参数**，如以下 URL 路径所示：`/Home/ProductDetail?id=2`
- **表单参数**，如下所示的标记：

```
<form method="post" action="/Home/ProductDetail">
  <input type="text" name="id" value="2" />
  <input type="submit" />
</form>
```

模型绑定器可以填充几乎任何类型：

- 简单类型，如 int、string、DateTime 和 bool。
- 由类、记录或结构体定义的复杂类型。
- 集合类型，如数组和列表。

让我们创建一个有点人为的示例来说明使用默认模型绑定器可以实现什么：

1. 在 Models 文件夹中，添加一个名为 Thing.cs 的新文件。
2. 修改内容以定义一个类，其中包含一个名为 Id 的可空整数和一个名为 Color 的字符串的两个属性，如以下代码所示：
3. 在 HomeController 中，添加两个新的操作方法，一个用于显示包含表单的页面，一个用于使用你的新模型类型并带参数显示事物，如以下代码所示：
4. 在 Views\Home 文件夹中，添加一个名为 ModelBinding.cshtml 的新文件。
5. 修改其内容，如以下标记所示：

```
namespace Northwind.Mvc.Models;

public class Thing
{
    public int? Id { get; set; }
    public string? Color { get; set; }
}
```

```
public IActionResult ModelBinding()
{
    return View(); // the page with a form to submit
}

public IActionResult ModelBinding(Thing thing)
{
    return View(thing); // show the model bound thing
}
```

```
@model Thing
@{
    ViewData["Title"] = "Model Binding Demo";
}
<h1>@ViewData["Title"]</h1>
```

```
<div>
    Enter values for your thing in the following form:
</div>
<form method="POST" action="/home/modelbinding?id=3">
    <input name="color" value="Red" />
    <input type="submit" />
</form>
@if (Model != null)
{
<h2>Submitted Thing</h2>
<hr />
<div>
    <dl class="dl-horizontal">
        <dt>Model.Id</dt>
        <dd>@Model.Id</dd>
        <dt>Model.Color</dt>
        <dd>@Model.Color</dd>
    </dl>
</div>
}
```

6. 在 Views/Home 中打开 Index.cshtml，并在第一个 <div> 中添加一个新段落，其中包含指向模型绑定页面的链接，如以下标记所示：

```
<p><a asp-action="ModelBinding" asp-controller="Home">Binding</a></p>
```

7. 启动网站。

8. 在主页上，点击 Binding。

9. 注意关于模糊匹配的未处理异常，如图 15.6 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_682_0.png)

Figure 15.6: 未处理的模糊操作 方法不匹配异常

10. 关闭 Chrome 并关闭 Web 服务器。

## 消除操作方法的歧义

虽然 C# 编译器可以通过注意到签名不同来区分这两种方法，但从 HTTP 请求路由的角度来看，这两种方法都是潜在的匹配项。我们需要一种特定于 HTTP 的方法来消除操作方法的歧义。

我们可以通过为操作创建不同的名称或指定一个方法应用于特定的 HTTP 动词（如 GET、POST 或 DELETE）来实现这一点。这就是我们将如何解决该问题的方法：

1. 在 HomeController 中，为第二个 ModelBinding 操作方法添加装饰，以指示它应用于处理 HTTP POST 请求，即在提交表单时，如以下代码中突出显示的那样：

```
[HttpPost]
public IActionResult ModelBinding(Thing thing)
```

> 另一个 ModelBinding 操作方法将隐式地用于所有其他类型的 HTTP 请求，如 GET、PUT、DELETE 等。

2. 启动网站。

3. 在主页上，点击 Binding。

4. 点击提交按钮并注意 Id 属性的值是从查询字符串参数设置的，而 color 属性的值是从表单参数设置的，如图 15.7 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_683_0.png)

Figure 15.7: 模型绑定演示页面

5. 关闭 Chrome 并关闭 Web 服务器。

## 传递路由参数

现在我们将使用路由参数设置属性：

1. 修改表单的操作以将值 2 作为路由参数传递，如以下标记中突出显示的那样：
2. 启动网站。
3. 在主页上，点击 Binding。
4. 点击提交按钮并注意 Id 属性的值是从路由参数设置的，而 Color 属性的值是从表单参数设置的。
5. 关闭 Chrome 并关闭 Web 服务器。

```
<form method="POST" action="/home/modelbinding/2?id=3">
```

## 传递表单参数

现在我们将使用表单参数设置属性：

1. 修改表单的操作以将值 1 作为表单参数传递，如以下标记中突出显示的那样：
2. 启动网站。
3. 在主页上，点击 Binding。
4. 点击提交按钮并注意 Id 和 Color 属性的值都从表单参数设置。

```
<form method="POST" action="/home/modelbinding/2?id=3">
  <input name="id" value="1" />
  <input name="color" value="Red" />
  <input type="submit" />
</form>
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_684_0.png)

> 良好实践：如果有多个同名参数，请记住表单参数在自动模型绑定中具有最高优先级，而查询字符串参数具有最低优先级。

## 验证模型

模型绑定过程可能会导致错误，例如数据类型转换或如果模型已用验证规则装饰则会产生验证错误。已绑定数据以及任何绑定或验证错误都存储在 ControllerBase.ModelState 中。

让我们通过对绑定模型应用一些验证规则，然后在视图中显示无效数据消息，来探索我们可以用模型状态做什么：

1. 在 Models 文件夹中打开 Thing.cs。
2. 导入 System.ComponentModel.DataAnnotations 命名空间。
3. 使用验证属性装饰 Id 属性以将允许的数字范围限制为 1 到 10，并使用一个属性确保访客提供颜色，并添加一个带有正则表达式用于验证的新 Email 属性，如以下代码中突出显示的那样：
4. 在 Models 文件夹中添加一个名为 HomeModelBindingViewModel.cs 的新文件。
5. 修改其内容以定义一个记录，其中包含属性用于存储绑定模型、指示是否存在错误的标志以及错误消息序列，如以下代码所示：
6. 在 HomeController 中，在处理 HTTP POST 的 ModelBinding 方法中，注释掉之前将 thing 传递给视图的语句，而是添加语句来创建视图模型实例。验证模型并存储错误消息数组，然后将视图模型传递给视图，如以下代码中突出显示的那样：

```
public class Thing
{
    [Range(1, 10)]
    public int? Id { get; set; }

    [Required]
    public string? Color { get; set; }

    [EmailAddress]
    public string? Email { get; set; }
}
```

```
namespace Northwind.Mvc.Models;

public record HomeModelBindingViewModel
(
    Thing Thing,
    bool HasErrors,
    IEnumerable<string> ValidationErrors
);
```

```
[HttpPost]
public IActionResult ModelBinding(Thing thing)
{
    HomeModelBindingViewModel model = new(
        thing,
        !ModelState.IsValid,
        ModelState.Values
            .SelectMany(state => state.Errors)
    )
```
