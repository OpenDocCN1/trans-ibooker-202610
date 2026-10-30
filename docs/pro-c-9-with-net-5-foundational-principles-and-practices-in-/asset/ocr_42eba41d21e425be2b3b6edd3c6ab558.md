# The Validation Tag Helpers

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

# Create a New Car

+   - The Pet Name field is required.
+   - The Color field is required.

![](bbox=[0.1, 0.58, 0.45, 0.81])

Figure 31-3. The validation tag helpers in action