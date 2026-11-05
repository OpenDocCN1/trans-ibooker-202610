```
_logger.LogWarning("Second warning!");
_logger.LogInformation("I am in the Index method of the HomeController.");
```

+   2. Start the Northwind.Mvc website project.
+   3. Start a web browser and navigate to the home page for the website.
+   4. At the command prompt or terminal, note the messages, as shown in the following output:
+   5. Close Chrome and shut down the web server.

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

# Understanding filters

When you need to add some functionality to multiple controllers and actions, you can use or define your own filters that are implemented as an attribute class.

Filters can be applied at the following levels:

+   - At the action level by decorating an action method with the attribute. This will only affect the one action method.
+   - At the controller level by decorating the controller class with the attribute. This will affect all methods of the controller.
+   - At the global level by adding the attribute type to the Filters collection of the MvcOptions instance that can be used to configure MVC when calling the AddControllersWithViews method, as shown in the following code:

```
builder.Services.AddControllersWithViews(options =>
{
    options.Filters.Add(typeof(MyCustomFilter));
});
```

# Using a filter to secure an action method

You might want to ensure that one particular action method of a controller class can only be called by members of certain security roles. You do this by decorating the method with the [Authorize] attribute, as described in the following list:

+   - [Authorize]: Only allow authenticated (non-anonymous, logged-in) visitors to access this action method.