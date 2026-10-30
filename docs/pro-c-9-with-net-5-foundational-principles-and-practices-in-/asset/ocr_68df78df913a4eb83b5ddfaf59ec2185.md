```
public string State { get; set; }
}
```

> Note  The class to be configured must have a public parameterless constructor and be nonabstract. Default values can be set on the class properties.

The Configure() method of the IServiceCollection maps a section of the configuration files to a specific type. That type can then be injected into classes and views using the options pattern. Open the Startup.cs file and add the following using statement:

```
using AutoLot.Mvc.Models;
```

Next, navigate to the ConfigureServices() method, and add the following line of code:

```
services.Configure<DealerInfo>(Configuration.GetSection(nameof(DealerInfo)));
```

Open HomeController and add the following using statement:

```
using Microsoft.Extensions.Options;
```

Next update the Index() method to the following:

```
[Route("/")]
[Route("/[controller]")]
[Route("/[controller]/[action]")]
[HttpGet]
public IActionResult Index([FromServices] IOptionsMonitor<DealerInfo> dealerMonitor)
{
    var vm = dealerMonitor.CurrentValue;
    return View(vm);
}
```

When a class is configured from the services collection and added into the DI container, it can be retrieved using the options pattern. In this example, OptionsMonitor will read the configuration file to create an instance of the DealerInfo class. The CurrentValue property gets the instance of DealerInfo created from the current settings file (even if the file has changed after the app was started). The DealerInfo instance is then passed to the Index.cshtml view.

Now update the Index.cshtml view located in the Views\Home directory to be strongly typed to the DealerInfo class and to display the properties of the model.

```
@model AutoLot.Mvc.Models.DealerInfo
@{
    ViewData["Title"] = "Home Page";
}
```

```
<div class="text-center">
    <h1 class="display-4">Welcome to @Model.DealerName</h1>
    <p class="lead">Located in @Model.City, @Model.State</p>
</div>
```