```
{
    return View();
}
}
```

> Note The view folder named after the controller is searched first. If the view cannot be found, the Shared folder is searched. If it still can’t be found, an exception is thrown.

To render a view with a name that is different than the action method name, pass in the name of the file (without the cshtml extension). The following code will render the CustomViewName.cshtml view:

```
public ActionResult SampleAction()
{
    return View("CustomViewName");
}
```

The final two overloads are for passing in a data object that becomes the model for the view. The first example uses the default view name, and the second example specifies a different view name.

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

The next section explores the Razor view engine in detail using a view that is rendered from an action method named RazorSyntax() on the HomeController. The action method will get a Car record from an instance of the CarRepo class that is injected into the method and will pass the Car instance as the model to the view.

Open HomeController in the AutoLot.Mvc application’s Controllers directory and add the following using statement:

```
using AutoLot.Dal.Repos.Interfaces;
```

Next, add the RazorSyntax() method to the controller.

```
[HttpGet]
public IActionResult RazorSyntax([FromServices] ICarRepo carRepo)
{
    var car = carRepo.Find(1);
    return View(car);
}
```