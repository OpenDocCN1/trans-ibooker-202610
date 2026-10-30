# The Item Edit Tag Helper

Create a new class named ItemEditTagHelper.cs in the TagHelpers folder. Make the class public and inherit from ItemLinkTagHelperBase. Add the following using statements to the new file:

```
using AutoLot.Mvc.Controllers;
using AutoLot.Mvc.TagHelpers.Base;
using Microsoft.AspNetCore.Mvc.Infrastructure;
using Microsoft.AspNetCore.Mvc.Routing;
using Microsoft.AspNetCore.Razor.TagHelpers;

namespace AutoLot.Mvc.TagHelpers
{
    public class ItemEditTagHelper : ItemLinkTagHelperBase
    {
    }
}
```

Add a public constructor to take in the required object instances and pass them to the base class.

```
public ItemEditTagHelper(
    IActionContextAccessor contextAccessor,
    IUrlHelperFactory urlHelperFactory)
    : base(contextAccessor, urlHelperFactory) { }
```

Override the Process() method, calling the BuildContent() method in the base class.

```
public override void Process(TagHelperContext context, TagHelperOutput output)
{
    BuildContent(output, nameof(CarsController.Edit), "text-warning", "Edit", "edit");
}
```

This creates the Edit link with the Font Awesome pencil image. To prevent compiler errors, add a basic Edit() method to the CarsController.

```
public IActionResult Edit()
{
    return View();
}
```

# The Item Create Tag Helper

Create a new class named ItemCreateTagHelper.cs in the TagHelpers folder. Make the class public and inherit from ItemLinkTagHelperBase. Add the following using statements to the new file:

```
using AutoLot.Mvc.Controllers;
using AutoLot.Mvc.TagHelpers.Base;
using Microsoft.AspNetCore.Mvc.Infrastructure;
using Microsoft.AspNetCore.Mvc.Routing;
using Microsoft.AspNetCore.Razor.TagHelpers;
```