The following changes should be implemented only if you get the preceding error when running the project:

+   1. In BotController, change the AllowSynchronousIO flag:

```
[HttpPost]
public async Task PostAsync()
{
    var syncIOFeature =
    _httpContextAccessor.HttpContext.Features.Get<IHttpBodyControlFeatureau
re>();
    if (syncIOFeature != null)
    {
        syncIOFeature.AllowSynchronousIO = true;
    }

    // Delegate the processing of the HTTP POST to the adapter.
    // The adapter will invoke the bot.
    await _adapter.ProcessAsync(Request, Response, _bot);
}
```

+   2. We'll need to inject IHttpContextAccessor into BotController; this is what
+   the constructor should now look like:

```
private readonly IHttpContextAccessor _httpContextAccessor;

public BotController(IBotFrameworkHttpAdapter adapter,
    IBot bot, IHttpContextAccessor httpContextAccessor)
{
    _adapter = adapter;
    _bot = bot;
    _httpContextAccessor = httpContextAccessor;
}
```

+   3. Lastly, in Startup.ConfigureServices, add the context accessor:

```
services.AddHttpContextAccessor();
```

Your project should now compile using .NET Core 3.0. Now let's investigate broadening
our code to deal with other intents.