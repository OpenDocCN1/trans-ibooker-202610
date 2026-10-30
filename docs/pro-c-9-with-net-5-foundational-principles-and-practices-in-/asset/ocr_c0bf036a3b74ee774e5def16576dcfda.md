The action method is decorated with the HTTPGet attribute to set this method as the application endpoint for the /Home/RazorSyntax as long as the request is an HTTP Get. The FromServices attribute on the ICarRepo parameter informs ASP.NET Core that the parameter should not be data bound with any incoming data, but instead the method receives the instance of the ICarRepo from the dependency injection container. The method gets an instance of a Car and returns a ViewResult using the View method. Since a view wasn’t named, ASP.NET Core will look for a view named RazorSyntax.cshtml in the Views\Home directory or the Views\Shared directory. If the view it isn’t found in either location, an exception will be returned to the client (browser).

Run the application and navigate your browser to https://localhost:5001/Home/RazorSyntax (if you are using Visual Studio and IIS, you will need to update the port). Since there isn’t a view in the project that can satisfy the request, an exception is returned to the browser. Recall from Chapter 29 that the Configure() method of the Startup class adds the UseDeveloperExceptionPage() method into the HTTP pipeline if the environment is Development. Figure 31-1 shows the results of this method in action.

![](bbox=[0.105, 0.315, 0.885, 0.555])

Figure 31-1. Error messaging from the developer exception page

The developer exception page provides ample information to debug your application, including raw exception details complete with the stack trace. Now, comment out that line in the Configure() method, replacing it with the “standard” error handler, like this:

```
if (env.IsDevelopment())
{
    //app.UseDeveloperExceptionPage();
    app.UseExceptionHandler("/Home/Error");
    ...
}
```

Run the app again and navigate to http://localhost:5001/Home/RazorSyntax, and you will see the standard error page, as shown in Figure 31-2.