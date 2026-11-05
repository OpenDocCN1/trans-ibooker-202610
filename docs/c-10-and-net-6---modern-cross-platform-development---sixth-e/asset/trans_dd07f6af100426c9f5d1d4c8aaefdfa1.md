
```
.GenerateEmailConfirmationTokenAsync(user);
IdentityResult result = await userManager
    .ConfirmEmailAsync(user, token);

if (result.Succeeded)
{
    WriteLine($"User {user.UserName} email confirmed successfully.");
}
else
{
    foreach (IdentityError error in result.Errors)
    {
        WriteLine(error.Description);
    }
}

if (!(await userManager.IsInRoleAsync(user, AdminRole)))
{
    IdentityResult result = await userManager
        .AddToRoleAsync(user, AdminRole);

    if (result.Succeeded)
    {
        WriteLine($"User {user.UserName} added to {AdminRole} successfully.");
    }
    else
    {
        foreach (IdentityError error in result.Errors)
        {
            WriteLine(error.Description);
        }
    }
}

return Redirect("/");
```

请注意以下事项：
- 角色名称和用户电子邮件的两个字段。
- 构造函数获取并存储已注册的用户和角色管理器依赖服务。
- 如果 Administrators 角色不存在，我们使用角色管理器创建它。

- 我们尝试通过电子邮件查找测试用户，如果不存在则创建它，然后将用户分配到 `Administrators` 角色。
- 由于网站使用 DOI，我们必须生成电子邮件确认令牌并使用它来确认新用户的电子邮件地址。
- 成功消息和任何错误都会写入控制台。
- 您将被自动重定向到主页。

- 3. 启动网站。
- 4. 点击 **Privacy** 并注意您将被重定向到登录页面。
- 5. 输入您的电子邮件和密码。（我使用了 `mark@example.com`。）
- 6. 点击 **Log in** 并注意您仍被拒绝访问。
- 7. 点击 **Home**。
- 8. 在地址栏中，手动输入 roles 作为相对 URL 路径，如以下链接所示：https://localhost:5001/roles。
- 9. 查看写入控制台的成功消息，如以下输出所示：
- 10. 点击 **Logout**，因为您必须注销并重新登录，以便在您已登录后创建的角色成员资格能够加载。
- 11. 再次尝试访问 **Privacy** 页面，输入以编程方式创建的新用户的电子邮件，例如 `test@example.com`，以及其密码，然后点击 **Log in**，您现在应该可以访问。
- 12. 关闭 Chrome 并关闭 Web 服务器。

```
User test@example.com created successfully.
User test@example.com email confirmed successfully.
User test@example.com added to Administrators successfully.
```

## 使用过滤器缓存响应

为了提高响应时间和可扩展性，您可能希望通过使用 [ResponseCache] 属性装饰方法来缓存由操作方法生成的 HTTP 响应。

您可以通过设置参数来控制响应的缓存位置和缓存时长，如以下列表所示：
- **Duration**：以秒为单位。这设置以秒为单位测量的 max-age HTTP 响应头。常见选择是一小时（3600 秒）和一天（86400 秒）。
- **Location**：ResponseCacheLocation 值之一，Any、Client 或 None。这设置 cache-control HTTP 响应头。
- **NoStore**：如果为 true，则忽略 Duration 和 Location 并将 cache-control HTTP 响应头设置为 no-store。

我们来看一个示例：
- 1. 在 HomeController.cs 中，为 Index 方法添加一个属性，以在浏览器或服务器和浏览器之间的任何代理上缓存响应 10 秒，如以下代码中突出显示所示：
- 2. 在 Views 中，在 Home 中，打开 Index.cshtml，并添加一个段落以输出当前时间长格式以包含秒，如以下标记所示：
- 3. 启动网站。
- 4. 注意主页上的时间。
- 5. 点击 Register。
- 6. 点击 Home 并注意主页上的时间相同，因为使用了页面的缓存版本。
- 7. 点击 Register。等待至少十秒。
- 8. 点击 Home 并注意时间现在已更新。
- 9. 点击 Log in，输入您的电子邮件和密码，然后点击 Log in。
- 10. 注意主页上的时间。
- 11. 点击 Privacy。
- 12. 点击 Home 并注意页面未被缓存。
- 13. 查看控制台并注意警告消息，解释您的缓存已被覆盖，因为访问者已登录，在此场景中，ASP.NET Core 使用防伪令牌，它们不应被缓存，如以下输出所示：
- 14. 关闭 Chrome 并关闭 Web 服务器。

```
[ResponseCache(Duration = 10, Location = ResponseCacheLocation.Any)]
public IActionResult Index()
```

```
<p class="alert alert-primary">@DateTime.Now.ToLongTimeString()</p>
```

```
warn: Microsoft.AspNetCore.Antiforgery.DefaultAntiforgery[8]
The 'Cache-Control' and 'Pragma' headers have been overridden and set to 'no-cache, no-store' and 'no-cache' respectively to prevent caching of this response. Any response that uses antiforgery should not be cached.
```

## 使用过滤器定义自定义路由

您可能希望为操作方法定义一个简化的路由，而不是使用默认路由。

例如，要显示隐私页面当前需要以下 URL 路径，该路径指定了控制器和操作：

```
https://localhost:5001/home/privacy
```

我们可以使路由更简单，如以下链接所示：

```
https://localhost:5001/private
```

我们来看如何操作：
- 1. 在 HomeController.cs 中，为 Privacy 方法添加一个属性以定义简化的路由，如以下代码中突出显示所示：
- 2. 启动网站。
- 3. 在地址栏中，输入以下 URL 路径：
- 4. 输入您的电子邮件和密码，点击 Log in，并注意简化路径显示 Privacy 页面。
- 5. 关闭 Chrome 并关闭 Web 服务器。

```
[Route("private")]
[Authorize(Roles = "Administrators")]
public IActionResult Privacy()
```

```
https://localhost:5001/private
```

## 理解实体和视图模型

在 MVC 中，M 代表模型。模型表示响应请求所需的数据。常用的有两种类型的模型：实体模型和视图模型。

实体模型表示 SQL Server 或 SQLite 等数据库中的实体。根据请求，可能需要从数据存储中检索一个或多个实体。实体模型使用类定义，因为它们可能需要更改然后用于更新底层数据存储。

我们希望在响应请求时显示的所有数据都是 MVC 模型，有时称为视图模型，因为它是一个传递到视图中以渲染为 HTML 或 JSON 等响应格式的模型。视图模型应该是不可变的，因此通常使用记录定义。

例如，以下 HTTP GET 请求可能意味着浏览器正在请求产品编号 3 的产品详细信息页面：

```
http://www.example.com/products/details/3
```

控制器需要使用 ID 路由值 3 来检索该产品的实体并将其传递给视图，然后视图可以将模型转换为 HTML 以在浏览器中显示。

想象一下，当用户访问我们的网站时，我们希望向他们展示类别轮播、产品列表以及本月访问者数量的计数。

我们将引用您在 *第 13 章，C# 和 .NET 的实用应用介绍* 中创建的 Northwind 数据库的 Entity Framework Core 实体数据模型：
- 1. 在 `Northwind.Mvc` 项目中，为 SQLite 或 SQL Server 添加对 `Northwind.Common.DataContext` 的项目引用，如以下标记所示：
- 2. 构建 `Northwind.Mvc` 项目以编译其依赖项。
- 3. 如果您使用的是 SQL Server，或者可能需要在 SQL Server 和 SQLite 之间切换，则在 `appsettings.json` 中添加使用 SQL Server 的 Northwind 数据库连接字符串，如以下标记中突出显示所示：
- 4. 在 `Program.cs` 中，导入命名空间以使用您的实体模型类型，如以下代码所示：
- 5. 在 `builder.Build` 方法调用之前，添加语句以加载适当的连接字符串，然后注册 Northwind 数据库上下文，如以下代码所示：
- 6. 向 `Models` 文件夹添加一个类文件并将其命名为 `HomeIndexViewModel.cs`。

```
<ItemGroup>
    <!-- change Sqlite to SqlServer if you prefer -->
    <ProjectReference Include=
"..\Northwind.Common.DataContext.Sqlite\Northwind.Common.DataContext.Sqlite.csproj" />
</ItemGroup>
```

```
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=aspnet-Northwind.Mvc-DC9C4FAF-DD84-4FC9-B925-69A61240EDA7;Trusted_Connection=True;MultipleActiveResultSets=true",
    "NorthwindConnection": "Server=.;Database=Northwind;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
}
```

```
using Packt.Shared; // AddNorthwindContext extension method
```

```
// if you are using SQL Server
string sqlServerConnection = builder.Configuration
    .GetConnectionString("NorthwindConnection");
builder.Services.AddNorthwindContext(sqlServerConnection);

// if you are using SQLite default is ..\Northwind.db
builder.Services.AddNorthwindContext();
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_668_0.png)

> **良好实践：** 尽管 MVC 项目模板创建的 `ErrorViewModel` 类不遵循此约定，但我建议您对视图模型类使用命名约定 `{Controller}{Action}ViewModel`。

