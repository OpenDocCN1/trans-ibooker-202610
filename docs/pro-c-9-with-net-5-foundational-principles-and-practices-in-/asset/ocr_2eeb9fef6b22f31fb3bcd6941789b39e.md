The first change from the default code is to add metadata to OpenApiInfo. Update the AddSwaggerGen() call to the following, which updates the title and adds a description and license information:

```
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1",
        new OpenApiInfo
        {
            Title = "AutoLot Service",
            Version = "v1",
            Description = "Service to support the AutoLot dealer site",
            License = new OpenApiLicense
            {
                Name = "Skimedic Inc",
                Url = new Uri("http://www.skimedic.com")
            }
        });
});
```

The next step is to move UseSwagger() and UseSwaggerUI() out of the development-only block and into the main execution path in Configure(). Also, update the title to “AutoLot Service v1” from “AutoLot.Api v1.”

```
public void Configure(IApplicationBuilder app, IWebHostEnvironment env, ApplicationDbContext context)
{
    if (env.IsDevelopment())
    {
        //If in development environment, display debug info
        app.UseDeveloperExceptionPage();
        //Original code
        //app.UseSwagger();
        //app.UseSwaggerUI(c => c.SwaggerEndpoint("/swagger/v1/swagger.json", "AutoLot.Api v1"));
        //Initialize the database
        if (Configuration.GetValue<bool>("RebuildDataBase"))
        {
            SampleDataInitializer.ClearAndReseedDatabase(context);
        }
    }

    // Enable middleware to serve generated Swagger as a JSON endpoint.
    app.UseSwagger();
    // Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.),
    // specifying the Swagger JSON endpoint.
    app.UseSwaggerUI(c => { c.SwaggerEndpoint("/swagger/v1/swagger.json", "AutoLot Service v1"); });
    ...
}
```

The preceding code opts in to using Swagger (app.UseSwagger()) and the Swagger UI (app.useSwaggerUI()). It also configures the endpoint for the swagger.json file.