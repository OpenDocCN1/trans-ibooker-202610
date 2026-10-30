> Note Child actions in classic ASP.NET MVC were action methods on a controller that could not serve as client-facing endpoints. They do not exist in ASP.NET Core.

For AutoLot, the view component will dynamically create the menu based on the makes that are in the database. The menu is visible on every page, so the logical place for it is in the _Layout.cshtml. But the _Layout.cshtml doesn’t have a server-side component (unlike views), so every action in the app would have to supply the data to _Layout.cshtml. This can be done in the OnActionExecuting event handler and the records placed in the ViewBag, but that can be messy to maintain. The blending of the server-side capabilities and the encapsulation of the UI make this scenario a perfect user case for view components.

# The Server-Side Code

Create a new folder named ViewComponents in the root directory of the AutoLot.Mvc project. Add a new class file named MenuViewComponent.cs into this folder. The convention is to name the view component classes with the ViewComponent suffix, just like controllers. And just like controllers, the ViewComponent suffix is dropped when calling view components.

Add the following using statements to the top of the file:

```
using System.Linq;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.ViewComponents;
```

Change the class to public and inherit from ViewComponent. View components don’t have to inherit from the ViewComponent base class, but like the Controller base class, inheriting from ViewComponent simplifies much of the work. Create a constructor that takes an instance of the IMakeRepo interface and assign that to a class-level variable. The code at this point looks like this:

```
namespace AutoLot.Mvc.ViewComponents
{
    public class MenuViewComponent : ViewComponent
    {
        private readonly IMakeRepo _makeRepo;
        public MenuViewComponent(IMakeRepo makeRepo)
        {
            _makeRepo = makeRepo;
        }
    }
```

There are two methods that are available for view components, Invoke() and InvokeAsync(). One of them must be implemented, and since the MakeRepo only makes synchronous calls, add in the Invoke() method, like this:

```
public async IViewComponentResult Invoke()
{
}
```

When a view component is rendered from a view, the public method Invoke()/InvokeAsync() is called. This method returns an IViewComponentResult, which is conceptually similar to a PartialViewResult, but much more streamlined. In the Invoke() method, get the list of Makes from the repo and, if successful, return a ViewViewComponentResult (no, that’s not a typo; it’s actually the name of the type) using the list of