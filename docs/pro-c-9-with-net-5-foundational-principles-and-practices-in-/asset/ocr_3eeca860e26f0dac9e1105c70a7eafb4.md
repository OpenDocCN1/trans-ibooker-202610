The input tag helper adds the name and id attributes to the rendered tag, the existing value for the property (if there is one), and HTML5 validation attributes. Both of the fields are required and have a string length limit of 50. Here is the rendered markup for those two properties:

```
<input class="form-control" type="text" data-val="true" data-val-length="The field Pet Name must be a string with a maximum length of 50." data-val-length-max="50" data-val-required="The Pet Name field is required." id="PetName" maxlength="50" name="PetName" value="Zippy">
```

```
<input class="form-control valid" type="text" data-val="true" data-val-length="The field Color must be a string with a maximum length of 50." data-val-length-max="50" data-val-required="The Color field is required." id="Color" maxlength="50" name="Color" value="Black" aria-describedby="Color-error" aria-invalid="false">
```

# The TextArea Tag Helper

The <textarea> tag helper adds the id and name attributes automatically and any HTML5 validation tags defined for the property. For example, the following line creates a textarea tag for the Description property:

```
<textarea asp-for="Description"></textarea>
```

# The Select Tag Helper

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