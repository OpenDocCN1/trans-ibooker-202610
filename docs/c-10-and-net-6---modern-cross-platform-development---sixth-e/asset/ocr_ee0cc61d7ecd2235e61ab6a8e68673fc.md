3. Update the version of the Swashbuckle.AspNetCore package to the latest, for example, at the time of writing in September 2021, it is 6.2.1.

4. In Program.cs, note the import for Microsoft's OpenAPI models namespace, as shown in the following code:

```
using Microsoft.OpenApi.Models;
```

5. Import Swashbuckle's SwaggerUI namespace, as shown in the following code:

```
using Swashbuckle.AspNetCore.SwaggerUI; // SubmitMethod
```

6. About halfway down Program.cs, note the statement to add Swagger support including documentation for the Northwind service, indicating that this is the first version of your service, and change the title, as shown highlighted in the following code:

```
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new()
    { Title = "Northwind Service API", Version = "v1" });
});
```

7. In the section that configures the HTTP request pipeline, note the statements to use Swagger and Swagger UI when in development mode, and define an endpoint for the OpenAPI specification JSON document.

8. Add code to explicitly list the HTTP methods that we want to support in our web service and change the endpoint name, as shown highlighted in the following code:

```
var app = builder.Build();

// Configure the HTTP request pipeline.
if (builder.Environment.IsDevelopment()
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "Northwind Service API Version 1");
        c.SupportedSubmitMethods(new[] {
            SubmitMethod.Get, SubmitMethod.Post,
            SubmitMethod.Put, SubmitMethod.Delete });
    });
}
```

# Testing requests with Swagger UI

You are now ready to test an HTTP request using Swagger:

+   1. Start the Northwind.WebApi web service.