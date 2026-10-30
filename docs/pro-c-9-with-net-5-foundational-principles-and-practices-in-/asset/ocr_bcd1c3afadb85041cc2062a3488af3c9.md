# Invoking View Components

View components are typically rendered from a view (although they can be rendered from a controller action method as well). The syntax is straightforward: Component.Invoke(<view component name>) or @ await Component.InvokeAsync(<view component name>). Just like with controllers, the ViewComponent suffix must be removed when invoking a view component.

```
@await Component.InvokeAsync("Menu") //async version
@Component.Invoke("Menu") //non-async version
```

# Invoking View Components As Custom Tag Helpers

Introduced in ASP.NET 1.1, view components can be invoked using tag helper syntax. Instead of using Component.InvokeAsync()/Component.Invoke(), simply call the view component like this:

```
<vc:menu></vc:menu>
```

To use this method of calling view components, your application must opt in to using them. This is done by adding the @addTagHelper command with the name of the assembly that contains the view component. The following line must be added to the _ViewImports.cshtml file, which was already added for the custom tag helpers:

```
@addTagHelper *, AutoLot.Mvc
```

# Updating the Menu

Open the _Menu.cshtml partial and navigate to just after the <li></li> block that maps to the Home/Index action method. Copy the following markup to the partial:

```
<li class="nav-item dropdown">
  <a class="nav-link dropdown-toggle text-dark" data-toggle="dropdown">Inventory <i class="fa fa-car"></i></a>
  <vc:menu />
</li>
```

The line in bold renders the MenuViewComponent into the menu. The surrounding markup is Bootstrap formatting.

Now when you run the application, you will see the Inventory menu with the makes listed as submenu items, as shown in Figure 31-10.