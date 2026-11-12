
## Integrating LUIS into the Bot Framework

It's worth noting that this process is well documented by Microsoft; feel free to follow this section or to have a look at the documentation found at https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-howto-v4-luis?view=azure-bot-service-4.0&tabs=csharp.

Let's go through the following steps:

- 1. The first thing we need in order to integrate LUIS is to add the connection details. In the LUIS portal, select MANAGE and from Application Information, copy the Application ID:

![](img/762492a8287f369cc1ac4621a212f193_462_0.png)

- 2. Add this into your appsettings.json file:

```
{
  "MicrosoftAppId": "",
  "MicrosoftAppPassword": "",
  "LuisAppId": "ed7252fa-43c1-4d4a-a084-2b8a58c69028",
  "LuisAPIKey": "",
  "LuisAPIHostName": ""
}
```

- 3. Now, select **Keys and Endpoints** and copy the **Authoring Key**:

![](img/762492a8287f369cc1ac4621a212f193_463_0.png)

- 4. At the bottom of the same page, you'll find the endpoint if you scroll down:

![](img/762492a8287f369cc1ac4621a212f193_463_1.png)

These values also need to be copied into `appsettings.json`:

```
{
  "MicrosoftAppId": "",
  "MicrosoftAppPassword": "",
  "LuisAppId": "ed7252fa-43c1-4d4a-a084-2b8a58c69028",
  "LuisAPIKey": "51d06b40b7df4f41be26b7d80cd80e73",
  "LuisAPIHostName": "https://westus.api.cognitive.microsoft.com"
}
```

> Note that the hostname is trimmed after `.microsoft.com`.

- 5. Once this is done, you'll need to **Publish** your model:

![](img/762492a8287f369cc1ac4621a212f193_464_0.png)

- 6. The next step is to install the LUIS NuGet package:

```
Install-Package Microsoft.Bot.Builder.AI.Luis
```

- 7. Finally, we need to plumb in the code; let's replace our check for Hello with a call to LUIS:

```
protected override async Task
OnMessageActivityAsync(ITurnContext<IMessageActivity> turnContext,
CancellationToken cancellationToken)
{
    var luisApplication = new LuisApplication(
        _configuration["LuisAppId"],
        _configuration["LuisAPIKey"],
        _configuration["LuisAPIHostName"]
    );

    var recognizer = new LuisRecognizer(luisApplication);
    var recognizerResult = await
    recognizer.RecognizeAsync(turnContext, cancellationToken);

    var (intent, score) = recognizerResult.GetTopScoringIntent();
    if (intent == "Hello")
    {
        await
    turnContext.SendActivityAsync(MessageFactory.Text($"Hello, my name
is Boris"), cancellationToken);
        }
        else
        {
            await
turnContext.SendActivityAsync(MessageFactory.Text($"Echo:
{turnContext.Activity.Text}"), cancellationToken);
        }
    }
```

Let's quickly review what we've changed here. Firstly, we instantiate a new LuisApplication and provide the credentials that we collected in the appsettings.json. In order to do this, we also inject IConfiguration into the class, which we'll come back to shortly.

We then call RecognizeAsync, which performs the actual analysis of the phrase, and finally, we ask it to tell us what it thinks the intent is. Let's see what that means for the bot:

![](img/762492a8287f369cc1ac4621a212f193_465_0.png)

As you can see, Boris now understands a little more; however, our response is still quite wooden, and it only works for the `Hello` intent. Let's address the other intents next, but first, we injected the `IConfiguration` into the class. Let's have a look at that change:

```
private readonly IConfiguration _configuration;
public BorisBot(IConfiguration configuration)
{ _configuration = configuration;
}
```

The next stage is to upgrade our project to use .NET Core 3.

At the time of writing, the templates use .NET Core 2.1; however, if that is not the case by the time you read this, please feel free to skip the next section.

## Upgrading the template from .NET Core 2.1 to 3.0

At this stage, we'll need to bump up the version from .NET Core 2.1 to .NET Core 3.0. This will likely result in us needing to make a couple of little tweaks.

The first tweak is that `IHostingEnvironment` should be replaced with `IHostEnvironment` (for a full transcript of this method, see the following):

```
public void Configure(IApplicationBuilder app, IHostEnvironment env)
```

This is to avoid namespace conflicts introduced in 2.1.

The next change is the compatibility version: it shouldn't surprise you to learn that the app now needs to be set to be compatible with version 3:

```
services.AddMvc()
.SetCompatibilityVersion(CompatibilityVersion.Version_3_0);
```

Now that we've added MVC to the dependency injection system, we need to register it as middleware in the application.

### UseMvc

Finally, there have been a number of changes around the endpoint routing. The net effect of this is that UseMvc now seems to configure too many settings in one go; as a result, the Configure method should look more like this:

```
public void Configure(IApplicationBuilder app, IHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseDefaultFiles();
    app.UseStaticFiles();

    //app.UseHttpsRedirection();
    //app.UseMvc();
    app.UseRouting();
    app.UseCors();
    app.UseEndpoints(e =>
    e.MapControllerRoute("default",
    "{controller=Home}/{action=Index}/{id?}")
    );
}
```

We no longer call UseMvc, but instead, call the specific aspects of middleware that we need.

### AllowSynchronousIO

In .NET Core 3.0, a breaking change was introduced, whereby the default of allowing synchronous calls was changed so that they were not allowed. At the time of writing, BotFrameworkAdapter breaks that rule (perhaps because it's using Newtonsoft JSON serialization—in which case this may not be an issue by the time this book is published). You might get the following error when running the project:

```
System.InvalidOperationException: Synchronous operations are disallowed.
Call ReadAsync or set AllowSynchronousIO to true instead.'
```

The following changes should be implemented only if you get the preceding error when running the project:

- 1. In BotController, change the AllowSynchronousIO flag:

```
[HttpPost]
public async Task PostAsync()
{
    var syncIOFeature =
    _httpContextAccessor.HttpContext.Features.Get<IHttpBodyControlFeature>();
    if (syncIOFeature != null)
    {
        syncIOFeature.AllowSynchronousIO = true;
    }

    // Delegate the processing of the HTTP POST to the adapter.
    // The adapter will invoke the bot.
    await _adapter.ProcessAsync(Request, Response, _bot);
}
```

- 2. We'll need to inject IHttpContextAccessor into BotController; this is what the constructor should now look like:

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

- 3. Lastly, in Startup.ConfigureServices, add the context accessor:

```
services.AddHttpContextAccessor();
```

Your project should now compile using .NET Core 3.0. Now let's investigate broadening our code to deal with other intents.

## Intent/response matrix

This section covers, essentially, the final two requirements of this API. We're going to create a JSON file that maps an intent to a possible reply (or replies). In fact, we're going to link each intent to multiple replies and then use one at random; that way, the bot will give the impression of responding uniquely to each message.

Let's start with the JSON document. Here's what it might look like:

```
{
 "Hello": [
 "Hi there!",
 "Hello, My name is Boris",
 "Nice to meet you",
 "Hello"
 ],
 "Family": [
 "Tell me more about your family",
 "Do you have any siblings?",
 "Let's talk about your family some more"
 ],
 "Feelings": [
 "It's important to talk about your feelings",
 "It's healthy to talk about how you feel",
 "Please tell me: how are you feeling"
 ],
 "Weather": [
 "How's the weather where you are",
 "In 1987, the BBC weatherman Michael Fish failed to predict a Hurricane",
 "The weather is very unpredictable",
 "What's the forecast for tomorrow?"
 ],
 "None": [
 "I'm afraid I'm not sure what you mean",
 "That sounds interesting, please tell me more",
 "Wow - really?"
 ]
}
```
