```
{
    r.MapHub<Chat>("/chat");
});
```

Note that we have added `app.UseStaticFiles()` to the `Configure` method. Static files are assets that an ASP.NET Core app serves directly to clients. Examples of static files include HTML, CSS, JavaScript, and images. The call to `app.UseDefaultFiles()` (which must be called prior to `app.UseStaticFiles()`) tells the middleware to search for a predefined list of HTML files, including `index.html`.

That's our server done.

+   3. Finally, we'll need to tell the SignalR Services how to connect to Azure; create a new file in the project called `appsettings.json`:

![](bbox=[0.104, 0.388, 0.891, 0.838])