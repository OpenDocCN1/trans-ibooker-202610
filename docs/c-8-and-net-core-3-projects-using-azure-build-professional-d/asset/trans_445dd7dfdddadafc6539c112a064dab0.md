

让我们先创建我们的IdentityServer，它可以只是一个标准的ASP.NET Core Web应用程序：

![](img/762492a8287f369cc1ac4621a212f193_286_0.png)

同样，我们将创建一个空应用程序，以便我们可以清楚地看到这是如何构建的。

> 除非另有说明，本节中的所有说明都与您刚刚创建的新（IdentityServer）项目相关。

在包管理器控制台中，我们将安装IdentityServer4包：

```
Install-Package IdentityServer4 -ProjectName StockChecker.IdentityServer
```

> 如果您决定将项目命名为与我不同的名称，那么您需要更改前面的项目名称。

在Startup.cs中，我们需要添加IdentityServer：

```
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddIdentityServer()
        .AddDeveloperSigningCredential();
}
```

这里我们做了两件事：我们将IdentityServer添加到**依赖注入（DI）**系统中，并添加了一些临时凭据。

我们稍后会再回来，添加一些（更多）有效的凭据，但这将让我们运行起来。

接下来，我们需要将IdentityServer注册到ASP.NET Core中间件管道中：

```
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseIdentityServer();
}
```

为了让IdentityServer运行，我们需要更改的最后一件事是告诉它将请求信息的是什么；这可以在ConfigureServices（在Startup.cs中）中完成：

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

显然，这在现阶段不会让我们实际对任何东西进行身份验证。本质上，为了正常运行，IdentityServer需要知道三件事：

- 谁需要访问（用户）
- 他们需要访问什么（资源）
- 他们将如何获得访问权限（客户端）

> IdentityServer提供帮助程序方法，例如`AddInMemoryClients`，作为入门方式。在将来的某个阶段，可能需要添加其他客户端或资源，并且可以轻松重构，以便将每个列表持久化到数据存储中。

## IdentityServer

目前，我们有一个运行中的身份服务器；但是，我们可以在没有任何凭据的情况下运行客户端并使用我们的应用程序。在将资源、客户端或用户添加到身份服务器之前，下一步是让它拒绝我们进入（因为我们还没有设置任何这些内容）。

## 保护 API

为了保护API，我们只需要做两件事（其中两者都不需要IdentityServer——并且其中一项已经为我们完成！）。首先，我们需要告诉ASP.NET Core我们想要使用授权。在我们的启动文件中，我们已经在调用`AddControllers`。因为ASP.NET Core现在是开源的，我们可以简单地查看它为我们做了什么：

```
```csharp
private static IMvcCoreBuilder AddControllersCore(IServiceCollection services)
{
    return services
        .AddMvcCore()
        .AddApiExplorer()
        .AddAuthorization()
        .AddCors()
        .AddDataAnnotations()
        .AddFormatterMappings();
}
```
```

> 许多Microsoft产品（尤其是.NET Core产品）中的常见做法是使用**生成器模式**来允许配置中间件。该模式的前提很简单：方法执行一个操作，然后返回对其进行调用的对象的引用。这允许更易于人类阅读的代码流（如前面的代码所示）。

这里的相关行是AddAuthorization。接下来我们需要做的是告诉ASP.NET Core我们想要保护什么。在控制器中，添加以下装饰器：

```
[Authorize]
[Route("api/[controller]")]
[ApiController]
public class StockController : ControllerBase
{
```

现在，如果您尝试访问API，将会收到错误。好吧；现在我们完全无法访问API；让我们插入IdentityServer代码。这里的原理很简单：我们将从我们的IdentityServer请求一个令牌，该令牌将允许我们访问我们的API。在我们的API中，我们将安装一个IdentityServer包：

```
Install-Package IdentityServer4.AccessTokenValidation -ProjectName StockChecker.Api
```

严格来说，您可以通过添加JWTBearer身份验证来完全不使用IdentityServer完成这部分。但是，使用IdentityServer确实会在此处为您提供某些优势，而且，既然我们已经在使用IdentityServer，为解决方案的一部分重新造轮子真的没有多大意义。

在我们的API中，我们将在ConfigureServices中需要以下代码：

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();

    services.AddAuthentication(IdentityServerAuthenticationDefaults.AuthenticationScheme)
        .AddIdentityServerAuthentication(options =>
        {
        // Base-address of our IdentityServer
        // (if you haven't purposely changed it then this is likely correct)
        options.Authority = "https://localhost:5001";

        // Name of the API resource
        options.ApiName = "StockCheckerApi";
        });
```

我们将身份验证添加到管道中：

```
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseAuthentication();

    app.UseRouting();
    app.UseAuthorization();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

## 客户端配置

我明白本章的流程可能看起来有点杂乱，但实际上，其中经过了深思熟虑。我们首先保护了API，这意味着我们已经保护了我们的资源。接下来我们要做的是更改客户端，以便它获取令牌并正确调用API（就是本节）；显然，这目前还无法工作，因为我们还没有更改我们的IdentityServer。在下一节中，我们将更改IdentityServer，我们应该看到一切突然生机勃勃。之所以按此顺序进行，是因为我经常发现，如果先看到它不工作，那么更容易理解某事是如何工作的（否则，你其实不知道自己做对了什么）。

在我们的UWP应用程序中，我们将需要另一个NuGet包：

```
Install-Package IdentityModel -ProjectName StockChecker.UWP
```

UWP应用程序（或一般的桌面应用程序）与Web应用程序在两个重要方面不同：首先，在Web应用程序中，您必须处理这样一个事实，即用户可以简单地在应用程序中导航到任何位置。例如，用户可以简单地将登录屏幕上的地址栏更改为以下内容：

https://www.mysecurisite.com/products/stock/1

因此，在保护Web应用程序时，您不能依赖屏幕的预期流程；但是，在桌面应用程序中，您可以。

第二个考虑是，用户在其桌面上拥有桌面应用程序的代码。我们正在使用.NET，这意味着逆向工程代码是一件非常简单的事情；但是，据我所知，没有哪种语言在有人足够决心的情况下不能在某种程度上被逆向工程。

这里的要点是，我们可以指望用户进入登录屏幕，并且只需很少的努力就能让他们留在那里，但我们绝不应该在客户端设备上存储任何可能允许用户访问服务器的内容。

## 登录屏幕

让我们创建一个新的登录屏幕；我将我的页面命名为LoginView。以下代码放在`<Page>`元素内：

```
<Grid HorizontalAlignment="Center" VerticalAlignment="Center">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" MinWidth="200" />
        <ColumnDefinition Width="Auto" MinWidth="200" />
    </Grid.ColumnDefinitions>
    <TextBlock Text="Username" Margin="5"
        Grid.Row="0" Grid.Column="0" />
    <TextBlock Text="Password" Margin="5"
        Grid.Row="1" Grid.Column="0" />
    <TextBox Text="{Binding Username, Mode=TwoWay,
        UpdateSourceTrigger=PropertyChanged}" Margin="5"
        Grid.Row="0" Grid.Column="1" />
    <PasswordBox Password="{Binding Password, Mode=TwoWay,
        UpdateSourceTrigger=PropertyChanged}" Margin="5"
        Grid.Row="1" Grid.Column="1" />
    <Button Grid.Row="2" Grid.Column="0" Grid.ColumnSpan="2"
        HorizontalAlignment="Center" Margin="5"
        Command="{Binding LoginCommand}">Login</Button>
</Grid>
```

这里有相当多的代码，但其中大部分只是语法（XAML像其父级XML一样相当冗长）。正如您所见，我们在这里使用数据绑定，就像我们之前做的那样。表单上只有一个按钮（因为用户要么登录，要么关闭应用程序）。

在这个阶段，我们还没有设置绑定或任何功能，因此视图将什么也不做。我们还需要告诉UWP应用程序启动到这个视图，而不是我们之前在App.xaml.cs（在OnLaunched方法中）创建的主视图：

```
if (e.PrelaunchActivated == false)
{
    if (rootFrame.Content == null)
    {
```

