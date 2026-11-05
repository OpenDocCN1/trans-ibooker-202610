
- Error 动作方法将包含用于追踪的请求 ID 的视图模型传递给其视图。错误响应不会被缓存：

```
using Microsoft.AspNetCore.Mvc; // Controller, IActionResult
using Northwind.Mvc.Models; // ErrorViewModel
using System.Diagnostics; // Activity

namespace Northwind.Mvc.Controllers;
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;

    public HomeController(ILogger<HomeController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        return View();
    }

    public IActionResult Privacy()
    {
        return View();
    }

    [ResponseCache(Duration = 0,
        Location = ResponseCacheLocation.None, NoStore = true)]
    public IActionResult Error()
    {
        return View(new ErrorViewModel { RequestId =
            Activity.Current?.Id ?? HttpContext.TraceIdentifier });
    }
}
```

如果访问者导航到 / 或 /Home 路径，则等同于 /Home/Index，因为它们是默认路由中控制器和动作的默认名称。

## 了解视图搜索路径约定

Index 和 Privacy 方法在实现上是相同的，但它们返回不同的网页。这是因为约定。View 方法的调用会在不同的路径中查找用于生成网页的 Razor 文件。

让我们故意破坏其中一个页面名称，以便可以看到默认搜索的路径：

1. 在 Northwind.Mvc 项目中，展开 Views 文件夹，然后展开 Home 文件夹。
2. 将 Privacy.cshtml 文件重命名为 Privacy2.cshtml。
3. 启动网站。
4. 启动 Chrome，导航到 https://localhost:5001/，点击 **Privacy**，并注意为渲染网页而搜索的路径（包括 MVC 视图和 Razor Pages 的 Shared 文件夹），如 图 15.3 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_660_0.png)

图 15.3：显示视图默认搜索路径的异常

5. 关闭 Chrome 并关闭 Web 服务器。
6. 将 Privacy2.cshtml 文件重新命名为 Privacy.cshtml。

你现在已经看到了视图搜索路径约定，如下列表所示：

- 特定 Razor 视图：/Views/{controller}/{action}.cshtml
- 共享 Razor 视图：/Views/Shared/{action}.cshtml
- 共享 Razor 页面：/Pages/Shared/{action}.cshtml

## 了解日志记录

你刚才看到一些错误被捕获并写入控制台。你可以使用记录器以相同的方式向控制台写入消息。

1. 在 Controllers 文件夹中，在 HomeController.cs 的 Index 方法中，添加语句使用记录器以各种级别向控制台写入一些消息，如以下代码所示：

```
_logger.LogError("This is a serious error (not really!)");
_logger.LogWarning("This is your first warning!");
```

```
_logger.LogWarning("Second warning!");
_logger.LogInformation("I am in the Index method of the HomeController.");
```

2. 启动 Northwind.Mvc 网站项目。
3. 启动 Web 浏览器并导航到网站的首页。
4. 在命令提示符或终端中，注意消息，如以下输出所示：
5. 关闭 Chrome 并关闭 Web 服务器。

```
fail: Northwind.Mvc.Controllers.HomeController[0]
    This is a serious error (not really!)
warn: Northwind.Mvc.Controllers.HomeController[0]
    This is your first warning!
warn: Northwind.Mvc.Controllers.HomeController[0]
    Second warning!
info: Northwind.Mvc.Controllers.HomeController[0]
    I am in the Index method of the HomeController.
```

## 了解筛选器

当你需要在多个控制器和动作中添加一些功能时，可以使用或定义自己的筛选器，这些筛选器实现为属性类。

筛选器可以在以下级别应用：

- 在动作级别通过为动作方法添加属性装饰。这只会影响一个动作方法。
- 在控制器级别通过为控制器类添加属性装饰。这将影响控制器的所有方法。
- 在全局级别通过将属性类型添加到 MvcOptions 实例的 Filters 集合中，该实例可在调用 AddControllersWithViews 方法时用于配置 MVC，如以下代码所示：

```
builder.Services.AddControllersWithViews(options =>
{
    options.Filters.Add(typeof(MyCustomFilter));
});
```

## 使用筛选器保护动作方法

你可能希望确保控制器类的某个特定动作方法只能由某些安全角色的成员调用。你可以通过用 [Authorize] 属性装饰该方法来实现，如下列表所述：

- [Authorize]：仅允许已通过身份验证（非匿名、已登录）的访问者访问此动作方法。
- [Authorize(Roles = "Sales,Marketing")]：仅允许属于指定角色的访问者访问此动作方法。

让我们看一个示例：

1. 在 HomeController.cs 中，导入 Microsoft.AspNetCore.Authorization 命名空间。
2. 向 Privacy 方法添加一个属性，仅允许属于名为 Administrators 的组/角色的已登录用户访问，如以下代码中突出显示所示：
3. 启动网站。
4. 点击 Privacy 并注意你被重定向到登录页面。
5. 输入你的电子邮件和密码。
6. 点击登录并注意你被拒绝访问。
7. 关闭 Chrome 并关闭 Web 服务器。

```
[Authorize(Roles = "Administrators")]
public IActionResult Privacy()
```

## 启用角色管理并以编程方式创建角色

默认情况下，ASP.NET Core MVC 项目中未启用角色管理，因此我们必须先启用它，然后再创建角色，然后我们将创建一个控制器，该控制器将以编程方式创建 Administrators 角色（如果尚不存在）并将测试用户分配给该角色：

1. 在 Program.cs 中，在 ASP.NET Core Identity 及其数据库的设置中，添加对 AddRoles 的调用以启用角色管理，如以下代码中突出显示所示：
2. 在 Controllers 中，添加一个名为 RolesController.cs 的空控制器类并修改其内容，如以下代码所示：

```
services.AddDefaultIdentity<IdentityUser>(
    options => options.SignIn.RequireConfirmedAccount = true)
    .AddRoles<IdentityRole>() // enable role management
    .AddEntityFrameworkStores<ApplicationDbContext>();
```

```
using Microsoft.AspNetCore.Identity; // RoleManager, UserManager
using Microsoft.AspNetCore.Mvc; // Controller, IActionResult

using static System.Console;

namespace Northwind.Mvc.Controllers;

public class RolesController : Controller
{
    private string AdminRole = "Administrators";
```

```
private string UserEmail = "test@example.com";

private readonly RoleManager<IdentityRole> roleManager;
private readonly UserManager<IdentityUser> userManager;

public RolesController(RoleManager<IdentityRole> roleManager,
    UserManager<IdentityUser> userManager)
{
    this.roleManager = roleManager;
    this.userManager = userManager;
}

public async Task<IActionResult> Index()
{
    if (!(await roleManager.RoleExistsAsync(AdminRole)))
    {
        await roleManager.CreateAsync(new IdentityRole(AdminRole));
    }

    IdentityUser user = await userManager.FindByEmailAsync(UserEmail);

    if (user == null)
    {
        user = new();
        user.UserName = UserEmail;
        user.Email = UserEmail;
        IdentityResult result = await userManager.CreateAsync(
            user, "Pa$$w0rd");

        if (result.Succeeded)
        {
            WriteLine($"User {user.UserName} created successfully.");
        }
        else
        {
            foreach (IdentityError error in result.Errors)
            {
                WriteLine(error.Description);
            }
        }
    }

    if (!user.EmailConfirmed)
    {
        string token = await userManager
```
