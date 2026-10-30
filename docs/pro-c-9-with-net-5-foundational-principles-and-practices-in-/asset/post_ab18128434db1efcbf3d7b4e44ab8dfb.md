
# Delete Zippy

Are you sure you want to delete this car?

Make
VW
Color
Black
Pet Name
Zippy

Delete | Back to List

Figure 31-9. The delete view

## The Delete Post Method

The Delete() HTTP Post action method only sends the Id and TimeStamp to the service wrapper.

```
[HttpPost("{id}")]
[ValidateAntiForgeryToken]
public IActionResult Delete(int id, Car car)
{
    if (id != car.Id)
    {
        return BadRequest();
    }
    _repo.Delete(car);
    return RedirectToAction(nameof(Index));
}
```

The Delete() HTTP Post method is streamlined to only send the values needed by EF Core to delete a record.

This completes the views and the controller for the Car entity.

## View Components

View components are another new feature in ASP.NET Core. They combine the benefits of partial views with child actions to render parts of the UI. Like partial views, they are called from another view, but unlike partial views by themselves, view components also have a server-side component. This combination makes them a great fit for functions like creating dynamic menus (as shown shortly), login panels, sidebar content, or anything that needs to run server-side code but doesn’t qualify to be a stand-alone view.

> Note Child actions in classic ASP.NET MVC were action methods on a controller that could not serve as client-facing endpoints. They do not exist in ASP.NET Core.

For AutoLot, the view component will dynamically create the menu based on the makes that are in the database. The menu is visible on every page, so the logical place for it is in the _Layout.cshtml. But the _Layout.cshtml doesn’t have a server-side component (unlike views), so every action in the app would have to supply the data to _Layout.cshtml. This can be done in the OnActionExecuting event handler and the records placed in the ViewBag, but that can be messy to maintain. The blending of the server-side capabilities and the encapsulation of the UI make this scenario a perfect user case for view components.

## The Server-Side Code

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

When a view component is rendered from a view, the public method Invoke()/InvokeAsync() is called. This method returns an IViewComponentResult, which is conceptually similar to a PartialViewResult, but much more streamlined. In the Invoke() method, get the list of Makes from the repo and, if successful, return a ViewViewComponentResult (no, that’s not a typo; it’s actually the name of the type) using the list of makes as the view model. If the call to get the Make records fails, return a ContentViewComponentResult with an error message. Update the method to the following:

```
public IViewComponentResult Invoke()
{
    var makes = _makeRepo.GetAll().ToList();
    if (!makes.Any())
    {
        return new ContentViewComponentResult("Unable to get the makes");
    }
    return View("MenuView", makes);
}
```

The View helper method from the base ViewComponent class is similar to the Controller class helper method of the same name, with a couple of key differences. The first difference is that the default view file name is Default.cshtml instead of the name of the method. However, like the controller view helper method, the name of the view can be anything as long as the name is passed into the method call (without the .cshtml extension). The second difference is that the location of the view must be one of these three directories:

```
Views/< controller>/Components/<view_component_name>/<view_name>
Views/Shared/Components/<view_component_name>/<view_name>
Pages/Shared/Components/<view_component_name>/<view_name>
```

> Note ASP.NET Core 2.x introduced Razor Pages as another mechanism for creating web applications. This book doesn’t cover Razor Pages.

The C# class can live anywhere (even in another assembly), but <viewname>.cshtml must be in one of the directories listed earlier.

## Build the Partial View

The partial view rendered by the MenuViewComponent will iterate through the Make records, adding each as a list item to be displayed in the Bootstrap menu. The All menu item is added first as a hard-coded value.

Create a new folder named Components under the Views\Shared folder. In this new folder, create another new folder named Menu. This folder name must match the name of the view component class created earlier, minus the ViewComponent suffix. In this folder, create a partial view named MenuView.cshtml.

Clear out the existing code and add the following markup:

```
@model IEnumerable<Make>
<div class="dropdown-menu">
<a class="dropdown-item text-dark" asp-area="" asp-controller="Cars" asp-action="Index">All</a>

@foreach (var item in Model)
{
    <a class="dropdown-item text-dark" asp-controller="Cars" asp-action="ByMake" asp-route-makeId="@item.Id" asp-route-makeName="@item.Name">@item.Name</a>
}
</div>
```

## Invoking View Components

View components are typically rendered from a view (although they can be rendered from a controller action method as well). The syntax is straightforward: Component.Invoke(<view component name>) or @ await Component.InvokeAsync(<view component name>). Just like with controllers, the ViewComponent suffix must be removed when invoking a view component.

```
@await Component.InvokeAsync("Menu") //async version
@Component.Invoke("Menu") //non-async version
```

## Invoking View Components As Custom Tag Helpers

Introduced in ASP.NET 1.1, view components can be invoked using tag helper syntax. Instead of using Component.InvokeAsync()/Component.Invoke(), simply call the view component like this:

```
<vc:menu></vc:menu>
```

To use this method of calling view components, your application must opt in to using them. This is done by adding the @addTagHelper command with the name of the assembly that contains the view component. The following line must be added to the _ViewImports.cshtml file, which was already added for the custom tag helpers:

```
@addTagHelper *, AutoLot.Mvc
```

## Updating the Menu

Open the _Menu.cshtml partial and navigate to just after the <li></li> block that maps to the Home/Index action method. Copy the following markup to the partial:

```
<li class="nav-item dropdown">
  <a class="nav-link dropdown-toggle text-dark" data-toggle="dropdown">Inventory <i class="fa fa-car"></i></a>
  <vc:menu />
</li>
```

The line in bold renders the MenuViewComponent into the menu. The surrounding markup is Bootstrap formatting.

Now when you run the application, you will see the Inventory menu with the makes listed as submenu items, as shown in Figure 31-10.
