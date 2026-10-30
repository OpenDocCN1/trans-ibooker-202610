```
asp-fallback-src="~/lib/bootstrap/js/bootstrap.bundle.min.js"
asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
crossorigin="anonymous"
integrity="sha384-xrRywqdh3PHs8keKZN+8zzc5TXOGRTLCCmivcbNJWm2rs5C8PRhcEn3czEjhA0Q0">
</script>
</environment>
<script src="~/js/site.js" asp-append-version="true"></script>
```

The final change is to update the locations of jquery.validate in the _ValidationScriptsPartial.cshtml partial view.

```
<environment include="Development">
<script src="~/lib/jquery-validation/jquery.validate.js" asp-append-version="true"></script>
<script src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.js" asp-append-version="true"></script>
</environment>
<environment exclude="Development">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.1/jquery.validate.min.js"
asp-fallback-src="~/lib/jquery-validation/jquery.validate.min.js"
asp-fallback-test="window.jQuery && window.jQuery.validator"
crossorigin="anonymous"
integrity="sha256-F6h55Qw6weK+t7SiOJX+2bpSAa3b/fnlrVCJVmEjA1A="></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validation-unobtrusive/3.2.11/jquery.validate.unobtrusive.min.js"
asp-fallback-src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.min.js"
asp-fallback-test="window.jQuery && window.jQuery.validator && window.jQuery.validator.unobtrusive"
crossorigin="anonymous"
integrity="sha256-9GycpJnlUiUjDVDqPoUEu/bsm9U+3dnQUH8+3W1ovkY="></script>
</environment>
```

# Finish the CarsController and Cars Views

This section finishes the CarsController and Cars views. If you set the RebuildDatabase flag in appsettings.development.json to true, then any changes you make while testing these views will be reset the next time you start the application.

# The CarsController

The CarsController is the focal point for the AutoLot.Mvc application, with create, read, update, and delete capabilities. This version of the CarsController uses the data access layer directly. Later in this chapter you will create another version of CarsController that uses the AutoLot.Api service for data access.

Begin by updating the using statements for the CarsController class to match the following:

```
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Models.Entities;
using AutoLot.Services.Logging;
```