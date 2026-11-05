```
```xml
<Project Sdk="Microsoft.NET.Sdk.BlazorWebAssembly">

    <PropertyGroup>
        <TargetFramework>net6.0</TargetFramework>
        <Nullable>enable</Nullable>
        <ImplicitUsings>enable</ImplicitUsings>
        <ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include=
            "Microsoft.AspNetCore.Components.WebAssembly"
            Version="6.0.0" />
        <PackageReference Include=
            "Microsoft.AspNetCore.Components.WebAssembly.DevServer"
            Version="6.0.0" PrivateAssets="all" />
    </ItemGroup>

    <ItemGroup>
        <ProjectReference Include=
            "..\Shared\Northwind.BlazorWasm.Shared.csproj" />
    </ItemGroup>

    <ItemGroup>
        <ServiceWorker Include="wwwroot\service-worker.js"
            PublishedContent="wwwroot\service-worker.published.js" />
    </ItemGroup>

</Project>
```
```

3. In the Client folder, open Program.cs and note that the host builder is for WebAssembly instead of server-side ASP.NET Core, and that it registers a dependency service for making HTTP requests, which is an extremely common requirement for Blazor WebAssembly apps, as shown in the following code:

```
```csharp
using Microsoft.AspNetCore.Components.Web;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Northwind.BlazorWasm.Client;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient
    { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

await builder.Build().RunAsync();
```
```