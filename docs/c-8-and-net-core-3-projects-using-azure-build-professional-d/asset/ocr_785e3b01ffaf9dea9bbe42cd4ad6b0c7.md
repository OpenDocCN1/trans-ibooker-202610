# Bot client

The final step is to create the client that we referenced in our UWP application. Let's start by creating a new project and immediately reference it from the UWP application. A .NET Standard 2.0 class library will be sufficient for what we need:

![](bbox=[0.302, 0.22, 0.692, 0.662])

The first thing we'll need is to import two NuGet packages: the Direct Line package itself and the Rest Client Runtime:

```
Install-Package Microsoft.Bot.Connector.DirectLine
Install-Package Microsoft.Rest.ClientRuntime
```

We'll only need a single class in this project—let's call it `DirectLineWrapper`.

> As a general principle, I always like to try to wrap external dependencies in a Wrapper Project to protect the main project from having too many direct external dependencies: this allows you to swap out the wrapper much easier if, for example, you wished to use a different bot service.