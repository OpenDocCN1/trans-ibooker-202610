3. On the next screen, select our **Research** model for the **Model class** and **ResearchContext** for the **Data context class**. You can leave the rest as is unless you'd like to change the **Controller name**:

![](bbox=[0.23, 0.18, 0.77, 0.52])

A brief look at the created controller shows us that we now have our basic **create, read, update, and delete (CRUD)** tasks in place. Now, it's time for the main event.

# # Running the application

Before we jump in and run the application, let's make sure our new page is easily accessible. The simplest way to do that is to just set it as the default home page:

+   1. Have a look at the `Configure` method in `Startup.cs`. You'll notice that the default route is specified as the `Home` controller.

+   2. Simply change the controller to your `Research` controller, as follows:

```
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Research}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});
```