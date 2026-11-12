In our IdentityServer services.cs file, we already have this:

```
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddIdentityServer()
        .AddDeveloperSigningCredential()
        .AddInMemoryClients(new List<Client>())
        .AddInMemoryIdentityResources(new List<IdentityResource>());
}
```

We are already telling IdentityServer about our list of clients, so let's introduce the other two concepts:

```
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddIdentityServer()
        .AddDeveloperSigningCredential()
        .AddInMemoryClients(IdentityServerHelper.GetClients())
        .AddInMemoryApiResources(IdentityServerHelper.GetApiResources())
        .AddTestUsers(IdentityServerHelper.GetUsers())
        .AddInMemoryIdentityResources(new List<IdentityResource>());
}
```

Now, we have introduced our three concepts. As is becoming the standard, we have used some methods (and a class) that don't exist yet.

> The method to add the users is not called AddTestUsers by accident. Although what we are doing here will work, it is not very extensible, and defining a data store for the users in the system is something that should be high up on the list of things to do to extend this project.

Our first method will be to add the clients:

```
public static class IdentityServerHelper
{
    internal static IEnumerable<Client> GetClients()
    {
        var clients = new List<Client>
        {
            new Client
            {
                ClientId = "StockChecker",
                AllowedGrantTypes = GrantTypes.ResourceOwnerPassword,
                ClientSecrets =
                {

```