```
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-U1DAWAzNBHeqEIlVSCgzaq+c9gqGAJ5c/t99JyeKa9xxaYpSvHU5awsuZVVIhvj" crossorigin="anonymous"></script>

@RenderSection("Scripts", required: false)

</body>
</html>
```

While reviewing the preceding markup, note the following:

+   - <title> is set dynamically using server-side code from a dictionary named ViewData. This is a simple way to pass data between different parts of an ASP.NET Core website. In this case, the data will be set in a Razor Page class file and then output in the shared layout.
+   - @RenderBody() marks the insertion point for the view being requested.
+   - A horizontal rule and footer will appear at the bottom of each page.
+   - At the bottom of the layout is a script to implement some cool features of Bootstrap that we can use later, such as a carousel of images.
+   - After the <script> elements for Bootstrap, we have defined a section named Scripts so that a Razor Page can optionally inject additional scripts that it needs.

6. Modify index.cshtml to remove all HTML markup except <div class="jumbotron"> and its contents, and leave the C# code in the @functions block that you added earlier.

7. Add a statement to the OnGet method to store a page title in the ViewData dictionary, and modify the button to navigate to a suppliers page (which we will create in the next section), as shown highlighted in the following markup:

```
@page

@functions
{
    public string? DayName { get; set; }

    public void OnGet()
    {
        ViewData["Title"] = "Northwind B2B";

        Model.DayName = DateTime.Now.ToString("dddd");
    }
}
<div class="jumbotron">
    <h1 class="display-3">Welcome to Northwind B2B</h1>
    <p class="lead">We supply products to our customers.</p>
```