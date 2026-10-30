
To render this view, add a new controller named CarsController to the Controllers folder. Update the code to the following (this code will be updated later in this chapter):

```
using Microsoft.AspNetCore.Mvc;

namespace AutoLot.Mvc.Controllers
{
    [Route("[controller]/[action]")]
    public class CarsController : Controller
    {
        public IActionResult Create()
        {
            return View();
        }
    }
}
```

Now run the application and navigate to http://localhost:5001/Cars/Create. Inspecting the source will reveal the form has the action attribute based on the asp-controller and asp-action, the method is set to post, and the __RequestVerificationToken was added as a hidden form input.

```
<form action="/Cars/Create" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="CfDJ8Hqg5HsrvCt0kkLRHY4ukxwvix0vkQ3vOvezvtJWdIOP5IwbI5-FFWXh8KCFZo7eKxveCuK8NRJywj8Jz23pP2nV37fIGqqcITRyISGgq7tRYZD uPv8NMIY z2nCWRiDbx0vlkg61DTDW9BrJxr8H63Y">
</form>
```

The Create view will be updated throughout this chapter.

## The Form Action Tag Helper

The Form Action tag helper is used on buttons and images to change the action for the form that contains them. For example, the following button added to the edit form will cause the post request to go to the Create endpoint:

```
<button type="submit" asp-action="Create">Index</button>
```

## The Anchor Tag Helper

The Anchor tag helper replaces the Html.ActionLink HTML helper. For example, to create a link for the RazorSyntax view, use the following code:

```
<a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="RazorSyntax">
    Razor Syntax
</a>
```

To add the Razor syntax page to the menu, update the _Menu.cshtml to the following, adding the new menu item between the Home and Privacy menu items (the <li> tags surrounding the anchor tags are for the Bootstrap menu):

```
...
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
</li>
```

```
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="RazorSyntax">Razor Syntax</a>
</li>
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
</li>
```

## The Input Tag Helper

The <input> tag helper is one of the most versatile tag helpers. In addition to autogenerating the HTML id and name attributes, as well as any HTML5 data-val validation attributes, the tag helper builds the appropriate HTML markup based on the data type of the target property. Table 31-3 lists the HTML type that is created based on the .NET Core type of the property.

Table 31-3. HTML Types Generated from .NET Types Using the Input Tag Helper

| .NET Type | Generated HTML Type |
|---|---|
| Bool | type="checkbox" |
| String | type="text" |
| DateTime | type="datetime" |
| Byte, Int, Single, Double | type="number" |

Additionally, the Input tag helper will add HTML5 type attributes based on data annotations. Table 31-4 lists some of the most common annotations and the generated HTML5 type attributes.

Table 31-4. HTML5 Type Attributes Generated from .NET Data Annotations

| .NET Data Annotation | Generated HTML5 Type Attribute |
|---|---|
| EmailAddress | type="email" |
| Url | type="url" |
| HiddenInput | type="hidden" |
| Phone | type="tel" |
| DataType(DataType.Password) | type="password" |
| DataType(DataType.Date) | type="date" |
| DataType(DataType.Time) | type="time" |

The Car.cshtml editor template contains <input> tags for the PetName and Color properties. As a reminder, just those tags are listed here:

```
<input asp-for="PetName" class="form-control" />
<input asp-for="Color" class="form-control"/>
```

The input tag helper adds the name and id attributes to the rendered tag, the existing value for the property (if there is one), and HTML5 validation attributes. Both of the fields are required and have a string length limit of 50. Here is the rendered markup for those two properties:

```
<input class="form-control" type="text" data-val="true" data-val-length="The field Pet Name must be a string with a maximum length of 50." data-val-length-max="50" data-val-required="The Pet Name field is required." id="PetName" maxlength="50" name="PetName" value="Zippy">
```

```
<input class="form-control valid" type="text" data-val="true" data-val-length="The field Color must be a string with a maximum length of 50." data-val-length-max="50" data-val-required="The Color field is required." id="Color" maxlength="50" name="Color" value="Black" aria-describedby="Color-error" aria-invalid="false">
```

## The TextArea Tag Helper

The <textarea> tag helper adds the id and name attributes automatically and any HTML5 validation tags defined for the property. For example, the following line creates a textarea tag for the Description property:

```
<textarea asp-for="Description"></textarea>
```

## The Select Tag Helper

The <select> tag helper builds input select tags from a model property and a collection. As with the other input tag helpers, the id and name are added to the markup, as well as any HTML5 data-val attributes. If the model property value matches one of the select list item's values, that option gets the selected attribute added to the markup.

For example, take a model that has a property named Country and a SelectList named Countries, with the list defined as follows:

```
public List<SelectListItem> Countries { get; } = new List<SelectListItem>
{
    new SelectListItem { Value = "MX", Text = "Mexico" },
    new SelectListItem { Value = "CA", Text = "Canada" },
    new SelectListItem { Value = "US", Text = "USA" },
};
```

The following markup will render the select tag with the appropriate options:

```
<select asp-for="Country" asp-items="Model.Countries"></select>
```

If the value of the Country property is set to CA, the following full markup will be output to the view:

```
<select id="Country" name="Country">
    <option value="MX">Mexico</option>
    <option selected="selected" value="CA">Canada</option>
    <option value="US">USA</option>
</select>
```

## The Validation Tag Helpers

The validation message and validation summary tag helpers closely mirror the Html.ValidationMessageFor and Html.ValidationSummaryFor HTML helpers. The first is applied to an HTML span for a specific property on the model, and the latter is applied to a div tag and represents the entire model. The validation summary has the option of All errors, ModelOnly (excluding errors on model properties), or None.
Recall the validation tag helpers from the Car.cshtml file’s EditorTemplate (shown here in bold):

```
<div asp-validation-summary="All" class="text-danger"></div>
<div class="form-group">
<label asp-for="PetName" class="col-form-label"></label>
<input asp-for="PetName" class="form-control" />
<span asp-validation-for="PetName" class="text-danger"></span>
</div>
<div class="form-group">
<label asp-for="MakeId" class="col-form-label"></label>
<select asp-for="MakeId" class="form-control" asp-items="ViewBag.MakeId"></select>
</div>
<div class="form-group">
<label asp-for="Color" class="col-form-label"></label>
<input asp-for="Color" class="form-control"/>
<span asp-validation-for="Color" class="text-danger"></span>
</div>
```

These helpers will display ModelState errors from binding and validation, as shown in Figure 31-3.

## Create a New Car

- The Pet Name field is required.
- The Color field is required.

![](img/587021f4d5ba039be70a11107130377b_1314_0.png)

Figure 31-3. The validation tag helpers in action

## The Environment Tag Helper

The <environment> tag helper is typically used to conditionally load JavaScript and CSS files (or any markup for that matter) based on the environment that the site is running under. Open the _Head.cshtml partial and update the markup to the following:

```
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>@ViewData["Title"] - AutoLot.Mvc</title>
<environment include="Development">
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
</environment>
<environment exclude="Development">
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
</environment>
<link rel="stylesheet" href="~/css/site.css" />
```

The first <environment> tag helper uses the include="Development" attribute to include the contained files when the environment is set to Development. In the preceding code, the nonminified version of Bootstrap is loaded. The second tag helper uses exclude="Development" to use the contained files when the environment is anything but Development and loads the minified version of bootstrap.css. The site.css file doesn’t change between the development and nondevelopment environments, so it is listed outside the <environment> tag helper.

Also, update the _JavaScriptFiles.cshtml partial to the following (notice the files in the Development section no longer have the .min extensions):

```
<environment include="Development">
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.js"></script>
</environment>
<environment exclude="Development">
<script src="~/lib/jquery/dist/jquery.min.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
</environment>
<script src="~/js/site.js" asp-append-version="true"></script>
```

## The Link Tag Helper

The <link> tag helper has attributes used for both local and remote. The asp-append-version attribute, used with local files, adds the hash of the file as a query string parameter to the URL that is sent to the browser. When the file changes, the hash changes, updating the URL sent to the browser. Since the link changed, the browser purges the cache for that file and reloads it. Update the bootstrap.css and site.css link tags in the _Head.cshtml file to the following:

```
<environment include="Development">
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" asp-append-version="true"/>
</environment>
<environment exclude="Development">
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
</environment>
<link rel="stylesheet" href="~/css/site.css" asp-append-version="true"/>
```
