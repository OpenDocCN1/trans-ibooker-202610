```
<hr />
<p>It's @Model.DayName! Our customers include restaurants, hotels, and cruise lines.</p>
<p>
<a class="btn btn-primary" href="suppliers">
Learn more about our suppliers</a>
</p>
</div>
```

8. Start the website, visit it with Chrome, and note that it has similar behavior as before, although clicking the button for suppliers will give a 404 Not Found error because we have not created that page yet.

# Using code-behind files with Razor Pages

Sometimes, it is better to separate the HTML markup from the data and executable code, so Razor Pages allows you to do this by putting the C# code in code-behind class files. They have the same name as the .cshtml file but end with .cshtml.cs.

You will now create a page that shows a list of suppliers. In this example, we are focusing on learning about code-behind files. In the next topic, we will load the list of suppliers from a database, but for now, we will simulate that with a hardcoded array of string values:

+   1. In the Pages folder, add two new files named Suppliers.cshtml and Suppliers.cshtml.cs. (The Visual Studio item template is named Razor Page - Empty and it creates both files.)
+   2. Add statements to the code-behind file named Suppliers.cshtml.cs, as shown in the following code:

```
using Microsoft.AspNetCore.Mvc.RazorPages; // PageModel

namespace Northwind.Web.Pages;

public class SuppliersModel : PageModel
{
    public IEnumerable<string>? Suppliers { get; set; }

    public void OnGet()
    {
        ViewData["Title"] = "Northwind B2B - Suppliers";

        Suppliers = new[]
        {
            "Alpha Co", "Beta Limited", "Gamma Corp"
        };
    }
}
```