
- The Error action method passes a view model into its view with a request ID used for tracing. The error response will not be cached:

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

If the visitor navigates to a path of / or /Home, then it is the equivalent of /Home/Index because those were the default names for controller and action in the default route.

## Understanding the view search path convention

The Index and Privacy methods are identical in implementation, yet they return different web pages. This is because of conventions. The call to the View method looks in different paths for the Razor file to generate the web page.

Let's deliberately break one of the page names so that we can see the paths searched by default:

1. In the Northwind.Mvc project, expand the Views folder and then the Home folder.
2. Rename the Privacy.cshtml file to Privacy2.cshtml.
3. Start the website.
4. Start Chrome, navigate to https://localhost:5001/, click **Privacy**, and note the paths that are searched for a view to render the web page (including in Shared folders for MVC views and Razor Pages), as shown in Figure 15.3:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_660_0.png)

Figure 15.3: An exception showing the default search path for views

5. Close Chrome and shut down the web server.
6. Rename the Privacy2.cshtml file back to Privacy.cshtml.

You have now seen the view search path convention, as shown in the following list:

- Specific Razor view: /Views/{controller}/{action}.cshtml
- Shared Razor view: /Views/Shared/{action}.cshtml
- Shared Razor Page: /Pages/Shared/{action}.cshtml

## Understanding logging

You have just seen that some errors are caught and written to the console. You can write messages to the console in the same way by using the logger.

1. In the Controllers folder, in HomeController.cs, in the Index method, add statements to use the logger to write some messages of various levels to the console, as shown in the following code:

```
_logger.LogError("This is a serious error (not really!)");
_logger.LogWarning("This is your first warning!");
```

```
_logger.LogWarning("Second warning!");
_logger.LogInformation("I am in the Index method of the HomeController.");
```

2. Start the Northwind.Mvc website project.
3. Start a web browser and navigate to the home page for the website.
4. At the command prompt or terminal, note the messages, as shown in the following output:
5. Close Chrome and shut down the web server.

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

## Understanding filters

When you need to add some functionality to multiple controllers and actions, you can use or define your own filters that are implemented as an attribute class.

Filters can be applied at the following levels:

- At the action level by decorating an action method with the attribute. This will only affect the one action method.
- At the controller level by decorating the controller class with the attribute. This will affect all methods of the controller.
- At the global level by adding the attribute type to the Filters collection of the MvcOptions instance that can be used to configure MVC when calling the AddControllersWithViews method, as shown in the following code:

```
builder.Services.AddControllersWithViews(options =>
{
    options.Filters.Add(typeof(MyCustomFilter));
});
```

## Using a filter to secure an action method

You might want to ensure that one particular action method of a controller class can only be called by members of certain security roles. You do this by decorating the method with the [Authorize] attribute, as described in the following list:

- [Authorize]: Only allow authenticated (non-anonymous, logged-in) visitors to access this action method.
- [Authorize(Roles = "Sales,Marketing")]: Only allow visitors who are members of the specified role(s) to access this action method.

Let's see an example:

1. In HomeController.cs, import the Microsoft.AspNetCore.Authorization namespace.
2. Add an attribute to the Privacy method to only allow access to logged-in users who are members of a group/role named Administrators, as shown highlighted in the following code:
3. Start the website.
4. Click Privacy and note that you are redirected to the log in page.
5. Enter your email and password.
6. Click Log in and note that you are denied access.
7. Close Chrome and shut down the web server.

```
[Authorize(Roles = "Administrators")]
public IActionResult Privacy()
```

## Enabling role management and creating a role programmatically

By default, role management is not enabled in an ASP.NET Core MVC project, so we must first enable it before creating roles, and then we will create a controller that will programmatically create an Administrators role (if it does not already exist) and assign a test user to that role:

1. In Program.cs, in the setup of ASP.NET Core Identity and its database, add a call to AddRoles to enable role management, as shown highlighted in the following code:
2. In Controllers, add an empty controller class named RolesController.cs and modify its contents, as shown in the following code:

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
