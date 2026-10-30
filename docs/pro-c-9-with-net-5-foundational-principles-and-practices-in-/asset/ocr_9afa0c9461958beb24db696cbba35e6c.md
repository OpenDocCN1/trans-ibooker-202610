Note If you are wondering why there aren’t any minified files listed, that will be covered shortly.

Once you save the file (in Visual Studio), the files will be loaded into the wwwroot\lib folder of the project. If running from the command line, enter the following command to reload all the files:

```
libman restore
```

Additional command-line options are available. Enter libman -h to explore all the options.

# Update the JavaScript and CSS References

The location of many of the JavaScript and CSS files changed with the move to Library Manager. Bootstrap and jQuery were loaded from a \dist\ folder. We also added Font Awesome into the application.

The location for the Bootstrap files needs to be updated to ~/lib/bootstrap/css instead of ~/lib/bootstrap/dist/css. Add Font Awesome to the end, just before site.css. Update the _Head.cshtml file to the following:

```
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>@ViewData["Title"] - AutoLot.Mvc</title>
<environment include="Development">
    <link rel="stylesheet" href="~/lib/bootstrap/css/bootstrap.css" asp-append-version="true" />
</environment>
<environment exclude="Development">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
          asp-fallback-href="~/lib/bootstrap/css/bootstrap.css"
          asp-fallback-test-class="sr-only" asp-fallback-test-property="position" asp-fallback-test-value="absolute"
          crossorigin="anonymous"
          integrity="sha384-ggOyROiXCbMq9xX..." />
</environment>
<link rel="stylesheet" href="~/lib/font-awesome/css/all.css" asp-append-version="true" />
<link rel="stylesheet" href="~/css/site.css" asp-append-version="true" />
```

Next, update _JavaScriptFiles.cshtml to take the \dist out of the jQuery and Bootstrap locations.

```
<environment include="Development">
    <script src="~/lib/jquery/jquery.js" asp-append-version="true"></script>
    <script src="~/lib/bootstrap/js/bootstrap.bundle.js" asp-append-version="true"></script>
</environment>
<environment exclude="Development">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js"
            asp-fallback-src="~/lib/jquery/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha256-FgpCb/KJQ1NLfOu91ta32o/NM...">
    </script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/js/bootstrap.bundle.min.js">
```