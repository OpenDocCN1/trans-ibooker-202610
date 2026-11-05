```
public Employee[] Employees { get; set; } = null!;

public void OnGet()
{
    ViewData["Title"] = "Northwind B2B - Employees";
    Employees = db.Employees.OrderBy(e => e.LastName)
        .ThenBy(e => e.FirstName).ToArray();
}
}
```

8. Modify Employees.cshtml, as shown in the following markup:

```
@page
@using Packt.Shared
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@model PacktFeatures.Pages.EmployeesPageModel
<div class="row">
    <h1 class="display-2">Employees</h1>
</div>
<div class="row">
@foreach(Employee employee in Model.Employees)
{
    <div class="col-sm-3">
        <partial name="_Employee" model="employee" />
    </div>
}
</div>
```

While reviewing the preceding markup, note the following:

+   - We import the Packt.Shared namespace so that we can use classes in it such as Employee.
+   - We add support for tag helpers so that we can use the <partial> element.
+   - We declare the @model type for this Razor Page to use the page model class that you just defined.
+   - We enumerate through the Employees in the model, outputting each one using a partial view.

# Implementing a partial view to show a single employee

The <partial> tag helper was introduced in ASP.NET Core 2.1. A partial view is like a piece of a Razor Page. You will create one in the next few steps to render a single employee: