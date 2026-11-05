```
```csharp
_logger.LogWarning("Second warning!");
_logger.LogInformation("I am in the Index method of the HomeController.");

HomeIndexViewModel model = new
(
    VisitorCount: (new Random()).Next(1, 1001),
    Categories: db.Categories.ToList(),
    Products: db.Products.ToList()
);
return View(model); // pass model to view
}
```
```

Remember the view search convention: when the View method is called in a controller's action method, ASP.NET Core MVC looks in the Views folder for a subfolder with the same name as the current controller, that is, Home. It then looks for a file with the same name as the current action, that is, Index.cshtml. It will also search for views that match the action method name in the Shared folder and for Razor Pages in the Pages folder.

# Understanding views

In MVC, the V stands for view. The responsibility of a view is to transform a model into HTML or other formats.

There are multiple view engines that could be used to do this. The default view engine is called Razor, and it uses the @ symbol to indicate server-side code execution. The Razor Pages feature introduced with ASP.NET Core 2.0 uses the same view engine and so can use the same Razor syntax.

Let's modify the home page view to render the lists of categories and products:

+   1. Expand the Views folder, and then expand the Home folder.
+   2. Open the Index.cshtml file and note the block of C# code wrapped in @{ }. This will execute first and can be used to store data that needs to be passed into a shared layout file like the title of the web page, as shown in the following code:
+   ```csharp
+   @{
+       ViewData["Title"] = "Home Page";
+   }
+   ```
+   3. Note the static HTML content in the <div> element that uses Bootstrap for styling.

![](bbox=[0.2, 0.92, 0.24, 0.94])

> Good Practice: As well as defining your own styles, base your styles on a common library, such as Bootstrap, that implements responsive design.