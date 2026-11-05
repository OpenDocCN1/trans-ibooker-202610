```
<body>
    <div class="container">
        <div class="jumbotron">
            <h1 class="display-3">Welcome to Northwind B2B</h1>
            <p class="lead">We supply products to our customers.</p>
            <hr />
            <h2>This is a static HTML page.</h2>
            <p>Our customers include restaurants, hotels, and cruise lines.</p>
            <p>
                <a class="btn btn-primary"
                   href="https://www.asp.net/">Learn more</a>
            </p>
        </div>
    </div>
</body>

</html>
```

> Good Practice: To get the latest <link> element for Bootstrap, copy and paste it from the documentation at the following link: https://getbootstrap.com/docs/5.0/getting-started/introduction/#starter-template.

# Enabling static and default files

If you were to start the website now and enter http://localhost:5000/index.html in the address box, the website would return a 404 Not Found error saying no web page was found. To enable the website to return static files such as index.html, we must explicitly configure that feature.

Even if we enable static files, if you were to start the website and enter http://localhost:5000/ in the address box, the website will return a 404 Not Found error because the web server does not know what to return by default if no named file is requested.

You will now enable static files, explicitly configure default files, and change the URL path registered that returns the plain text Hello World! response:

+   1. In Startup.cs, in the Configure method, add statements after enabling HTTPS redirection to enable static files and default files, and modify the statement that maps a GET request to return the Hello World! plain text response to only respond to the URL path /hello, as shown highlighted in the following code:

```
app.UseHttpsRedirection();

app.UseDefaultFiles(); // index.html, default.html, and so on
```