It would be useful to enable CORS on the server and configure our web service to only allow requests that originate from the MVC website:

+   1. In the Northwind.WebApi project, open Program.cs.
+   2. Add a statement in the services configuration section to add support for CORS, as shown in the following code:
+   3. Add a statement in the HTTP pipeline configuration section, before calling UseEndpoints, to use CORS and allow GET, POST, PUT, and DELETE requests from any website like Northwind MVC that has an origin of https://localhost:5001, as shown in the following code:
+   4. Start the Northwind.WebApi project and confirm that the web service is listening only on port 5002, as shown in the following output:
+   5. Start the Northwind.Mvc project and confirm that the website is listening on ports 5000 and 5002, as shown in the following output:
+   6. Start Chrome.
+   7. In the customer form, enter a country like Germany, UK, or USA, click Submit, and note the list of customers, as shown in Figure 16.18:

```
builder.Services.AddCors();
```

```
app.UseCors(configurePolicy: options =>
{
    options.WithMethods("GET", "POST", "PUT", "DELETE");
    options.WithOrigins(
        "https://localhost:5001" // allow requests from the MVC client
    );
});
```

```
info: Microsoft.Hosting.Lifetime[14]
  Now listening on: https://localhost:5002
```

```
info: Microsoft.Hosting.Lifetime[14]
  Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[14]
  Now listening on: http://localhost:5000
```