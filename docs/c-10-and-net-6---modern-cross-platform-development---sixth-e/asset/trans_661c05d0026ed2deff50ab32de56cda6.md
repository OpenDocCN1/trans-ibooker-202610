
![](img/c63b00dca7192cc58cfb9e1f1e64f483_623_0.png)

图 14.11：从字符串数组加载的供应商表

## 使用 ASP.NET Core 中的 Entity Framework Core

Entity Framework Core 是将真实数据引入网站的自然方式。在第 13 章《介绍 C# 和 .NET 的实用应用》中，你创建了两对类库：一组用于实体模型，一组用于 Northwind 数据库上下文，可用于 SQL Server 或 SQLite 或两者。你现在将在网站项目中使用它们。

## 将 Entity Framework Core 配置为服务

ASP.NET Core 所需的功能（如 Entity Framework Core 数据库上下文）必须在网站启动期间注册为服务。GitHub 仓库解决方案中的代码以及下面的代码使用 SQLite，但如果你愿意，也很容易使用 SQL Server。

我们来看看如何操作：

- 1. 在 Northwind.Web 项目中，为 SQLite 或 SQL Server 添加对 Northwind.Common.DataContext 项目的项目引用，如以下标记所示：

```
<!-- change Sqlite to SqlServer if you prefer -->
<ItemGroup>
  <ProjectReference Include="..\Northwind.Common.DataContext.Sqlite\Northwind.Common.DataContext.Sqlite.csproj" />
</ItemGroup>
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_624_0.png)

项目引用必须全部在一行上，不能换行。

- 2. 构建 Northwind.Web 项目。

- 3. 在 Startup.cs 中，导入命名空间以使用你的实体模型类型，如以下代码所示：

```
using Packt.Shared; // AddNorthwindContext extension method
```

- 4. 在 ConfigureServices 方法中添加一条语句来注册 Northwind 数据库上下文类，如以下代码所示：

```
services.AddNorthwindContext();
```

- 5. 在 Northwind.Web 项目的 Pages 文件夹中，打开 Suppliers.cshtml.cs，并导入我们数据库上下文的命名空间，如以下代码所示：

```
using Packt.Shared; // NorthwindContext
```

- 6. 在 SuppliersModel 类中，添加一个私有字段来存储 Northwind 数据库上下文，并添加一个构造函数来设置它，如以下代码所示：

```
private NorthwindContext db;

public SuppliersModel(NorthwindContext injectedContext)
{
    db = injectedContext;
}
```

- 7. 将 Suppliers 属性更改为包含 Supplier 对象而不是字符串值。

- 8. 在 OnGet 方法中，修改语句以从数据库上下文的 Suppliers 属性设置 Suppliers 属性，按国家/地区然后按公司名称排序，如以下代码中突出显示的那样：

```
public void OnGet()
{
    ViewData["Title"] = "Northwind B2B - Suppliers";

    Suppliers = db.Suppliers
        .OrderBy(c => c.Country).ThenBy(c => c.CompanyName);
}
```

- 9. 修改 Suppliers.cshtml 的内容以导入 Packt.Shared 命名空间并为每个供应商渲染多个列，如以下标记中突出显示的那样：

```
@page
@using Packt.Shared
```

```
@model Northwind.Web.Pages.SuppliersModel
<div class="row">
    <h1 class="display-2">Suppliers</h1>
    <table class="table">
        <thead class="thead-inverse">
            <tr>
                <th>Company Name</th>
                <th>Country</th>
                <th>Phone</th>
            </tr>
        </thead>
        <tbody>
            @if (Model.Suppliers is not null)
            {
                @foreach(Supplier s in Model.Suppliers)
                {
                    <tr>
                        <td>@s.CompanyName</td>
                        <td>@s.Country</td>
                        <td>@s.Phone</td>
                    </tr>
                }
            }
        </tbody>
    </table>
</div>
```

- 10. 启动网站。
- 11. 在 Chrome 中输入 https://localhost:5001/。
- 12. 点击“了解更多关于我们的供应商”，并注意供应商表现在从数据库加载，如图 14.12 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_625_0.png)

图 14.12：从 Northwind 数据库加载的供应商表

## 使用 Razor Pages 操作数据

你现在将添加插入新供应商的功能。

## 启用模型插入实体

首先，你将修改供应商模型，使其在访客提交表单以插入新供应商时响应 HTTP POST 请求：

- 1. 在 Northwind.Web 项目的 Pages 文件夹中，打开 Suppliers.cshtml.cs 并导入以下命名空间：
- 2. 在 SuppliersModel 类中，添加一个属性来存储单个供应商，并添加一个名为 OnPost 的方法，如果其模型有效，则将供应商添加到 Northwind 数据库的 Suppliers 表中，如以下代码所示：

```
using Microsoft.AspNetCore.Mvc; // [BindProperty], IActionResult
```

```
[BindProperty]
public Supplier? Supplier { get; set; }

public IActionResult OnPost()
{
    if ((Supplier is not null) && ModelState.IsValid)
    {
        db.Suppliers.Add(Supplier);
        db.SaveChanges();
        return RedirectToPage("/suppliers");
    }
    else
    {
        return Page(); // return to original page
    }
}
```

在回顾前面的代码时，请注意以下几点：

- 我们添加了一个名为 Supplier 的属性，该属性用 [BindProperty] 属性装饰，以便我们可以轻松地将网页上的 HTML 元素连接到 Supplier 类中的属性。
- 我们添加了一个响应 HTTP POST 请求的方法。它检查所有属性值是否符合 Supplier 类实体模型上的验证规则（如 [Required] 和 [StringLength]），然后将供应商添加到现有表中并将更改保存到数据库上下文中。这将生成一条 SQL 语句以在数据库中执行插入。然后它重定向到 Suppliers 页面，以便访客看到新添加的供应商。

## 定义用于插入新供应商的表单

接下来，你将修改 Razor Page 以定义一个访客可以填写并提交以插入新供应商的表单：

- 1. 在 Suppliers.cshtml 中，在 @model 声明之后添加标记助手，以便我们可以在此 Razor Page 上使用 asp-for 等标记助手，如以下标记所示：

```
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```

- 2. 在文件底部添加一个用于插入新供应商的表单，并使用 asp-for 标记助手将 Supplier 类的 CompanyName、Country 和 Phone 属性绑定到输入框，如以下标记所示：

```
<div class="row">
  <p>Enter details for a new supplier:</p>
  <form method="POST">
    <div><input asp-for="Supplier.CompanyName" placeholder="Company Name" /></div>
    <div><input asp-for="Supplier.Country" placeholder="Country" /></div>
    <div><input asp-for="Supplier.Phone" placeholder="Phone" /></div>
    <input type="submit" />
  </form>
</div>
```

在回顾前面的标记时，请注意以下几点：

- 带有 POST 方法的 <form> 元素是普通的 HTML，因此其中的 <input type="submit" /> 元素将使用表单内任何其他元素的值向当前页面发出 HTTP POST 请求。
- 带有名为 asp-for 的标记助手的 <input> 元素可启用与 Razor Page 后面模型的数据绑定。

- 3. 启动网站。

- 4. 点击“了解更多关于我们的供应商”，滚动到页面底部，输入 Bob's Burgers、USA 和 (603) 555-4567，然后点击提交。

- 5. 注意你会看到一个已刷新的供应商表，其中包含新添加的供应商。

- 6. 关闭 Chrome 并关闭 Web 服务器。

## 向 Razor Page 注入依赖服务

如果你有一个没有代码隐藏文件的 .cshtml Razor Page，那么你可以使用 @inject 指令注入依赖服务，而不是使用构造函数参数注入，然后直接使用 Razor 语法在标记中间引用注入的数据库上下文。

我们来创建一个简单的示例：

- 1. 在 Pages 文件夹中添加一个名为 Orders.cshtml 的新文件。（Visual Studio 项目模板名为 **Razor Page - Empty**，它会创建两个文件。删除 .cs 文件。）
- 2. 在 Orders.cshtml 中编写代码以输出 Northwind 数据库中的订单数量，如以下标记所示：
- 3. 启动网站。
- 4. 导航到 /orders 并注意你会看到 Northwind 数据库中有 830 个订单。
- 5. 关闭 Chrome 并关闭 Web 服务器。

```
@page
@using Packt.Shared
@inject NorthwindContext db
@{
    string title = "Orders";
    ViewData["Title"] = $"Northwind B2B - {title}";
}
<div class="row">
    <h1 class="display-2">@title</h1>
    <p>
        There are @db.Orders.Count() orders in the Northwind database.
    </p>
</div>
```

## 使用 Razor 类库

与 Razor Page 相关的所有内容都可以编译成类库，以便在多个项目中更轻松地重用。从 ASP.NET Core 3.0 及更高版本开始，这可以包括 HTML、CSS、JavaScript 库以及图像文件等媒体资源等静态文件。网站可以使用类库中定义的 Razor Page 的视图，也可覆盖它。

## 创建 Razor 类库

我们来创建一个新的 Razor 类库：

使用你喜欢的代码编辑器添加一个新项目，如以下列表中定义：

- 1. 项目模板：**Razor Class Library** / razorclasslib
- 2. 复选框/开关：**Support pages and views** / -s
- 3. 工作区/解决方案文件和文件夹：PracticalApps
- 4. 项目文件和文件夹：Northwind.Razor.Employees
