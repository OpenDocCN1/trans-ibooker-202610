Next, add the following line to the ConfigureServices() method:

```
services.TryAddSingleton<IActionContextAccessor, ActionContextAccessor>();
```

# Create the String Extension Method

When referring to controller names in code, quite often ASP.NET Core needs to have the raw string value, without the Controller suffix. This prevents using the nameof() method without then calling string.Replace(). As this gets tedious over time, we’ll create a string extension method to handle this.

Add a new folder named Utilities to the AutoLot.Services project, and in that folder, add a new static class named StringExtensions.cs. Update the code to the following to add the RemoveController() extension method:

```
using System;

namespace AutoLot.Mvc.Extensions
{
    public static class StringExtensions
    {
        public static string RemoveController(this string original)
            => original.Replace("Controller", "", StringComparison.OrdinalIgnoreCase);
    }
}
```

# Create the Base Class

Create a new folder named TagHelpers in the root of the AutoLot.Mvc project. In this folder, create a new folder named Base, and in that folder, create a class named ItemLinkTagHelperBase.cs, make the class public and abstract, and inherit from TagHelper. Add the following using statements to the new file:

```
using AutoLot.Mvc.Controllers;
using AutoLot.Services.Utilities;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.Infrastructure;
using Microsoft.AspNetCore.Mvc.Routing;
using Microsoft.AspNetCore.Razor.TagHelpers;

namespace AutoLot.Mvc.TagHelpers.Base
{
    public abstract class ItemLinkTagHelperBase : TagHelper
    {
    }
}
```

Add a constructor that takes instances of IActionContextAccessor and IUrlHelperFactory. Use the UrlHelperFactory with the ActionContextAccessor to create an instance of IUrlHelper, and store that in a class-level variable. The code is shown here:

```
protected readonly IUrlHelper UrlHelper;
protected ItemLinkTagHelperBase(IActionContextAccessor contextAccessor, IUrlHelperFactory urlHelperFactory)
```