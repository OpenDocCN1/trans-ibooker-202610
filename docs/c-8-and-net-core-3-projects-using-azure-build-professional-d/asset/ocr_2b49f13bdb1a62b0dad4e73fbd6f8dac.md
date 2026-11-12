Let's plumb this in. The changes that we need are in the bot file BorisBot.cs. We're going to change the OnMessageActivityAsync method. Along the way, we'll meet another couple of new features in .NET Core 3 and C# 8:

+   1. Let's start by removing the code from here that we no longer require:

```
if (intent == "Hello")
{
    await turnContext.SendActivityAsync(MessageFactory.Text($"Hello, my
name is Boris"), cancellationToken);
}
else
{
    await turnContext.SendActivityAsync(MessageFactory.Text($"Echo:
{turnContext.Activity.Text}"), cancellationToken);
}
```

We're going to replace this placeholder code with some code that generates a more realistic response.

+   2. Our next step is to read the JSON file into text, resulting in result:

```
var assembly = Assembly.GetExecutingAssembly();
var resourceName = "Boris.Data.Intent-Response.json";
using Stream stream =
    assembly.GetManifestResourceStream(resourceName);
using StreamReader reader = new StreamReader(stream);
string result = reader.ReadToEnd();
```

Note that we're using the new using syntax in C# 8. This is especially nice in methods such as this because without the new syntax, the method would be longer, and in the following code the using statements would need to be nested. The new using statement has an implicit scope from the declaration point to the end of the function; in all other respects, it behaves exactly like a standard using statement.

+   3. We then pass the text contents of the JSON file and the intent to a new method, which then returns a reply array. We then select a random reply from that array, and send that:

```
var response = ReadResponse(result, intent);
string selectedResponse = response[_rnd.Next(response.Length)];
await
```