
```
<script src="_framework/blazor.server.js"></script>
</body>
</html>
```

While reviewing the preceding markup, note the following:

- `<div id="blazor-error-ui">` for showing Blazor errors that will appear as a yellow bar at the bottom of the web page when an error occurs
- The script block for blazor.server.js manages the SignalR connection back to the server

9. In the Northwind.BlazorServer folder, open App.razor and note that it defines a Router for all components found in the current assembly, as shown in the following code:

```
<Router AppAssembly="@typeof(App).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData"
                    DefaultLayout="@typeof(MainLayout)" />
        <FocusOnNavigate RouteData="@routeData" Selector="h1" />
    </Found>
    <NotFound>
        <PageTitle>Not found</PageTitle>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

While reviewing the preceding markup, note the following:

- If a matching route is found, then RouteView is executed that sets the default layout for the component to MainLayout and passes any route data parameters to the component.
- If a matching route is not found, then LayoutView is executed that renders the internal markup (in this case, a simple paragraph element with a message telling the visitor there is nothing at this address) inside MainLayout.

10. In the Shared folder, open MainLayout.razor and note that it defines <div> for a sidebar containing a navigation menu that is implemented by the NavMenu.razor component file in this project, and an HTML5 element such as <main> and <article> for the content, as shown in the following code:

```
@inherits LayoutComponentBase

<PageTitle>Northwind.BlazorServer</PageTitle>

<div class="page">
    <div class="sidebar">
    <NavMenu />
    </div>

    <main>
        <div class="top-row px-4">
            <a href="https://docs.microsoft.com/aspnet/"
            target="_blank">About</a>
        </div>
        <article class="content px-4">
            @Body
        </article>
    </main>
</div>
```

11. In the Shared folder, open MainLayout.razor.css and note that it contains isolated CSS styles for the component.

12. In the Shared folder, open NavMenu.razor and note that it has three menu items for Home, Counter, and Fetch data. These are created by using a Microsoft-provided Blazor component named NavLink, as shown in the following markup:

```
<div class="top-row ps-3 navbar navbar-dark">
    <div class="container-fluid">
        <a class="navbar-brand" href="">Northwind.BlazorServer</a>
        <button title="Navigation menu" class="navbar-toggler"
            @onclick="ToggleNavMenu">
            <span class="navbar-toggler-icon"></span>
        </button>
    </div>
</div>

<div class="@NavMenuCssClass" @onclick="ToggleNavMenu">
    <nav class="flex-column">
        <div class="nav-item px-3">
            <NavLink class="nav-link" href="" Match="NavLinkMatch.All">
                <span class="oi oi-home" aria-hidden="true"></span> Home
            </NavLink>
        </div>
        <div class="nav-item px-3">
            <NavLink class="nav-link" href="counter">
                <span class="oi oi-plus" aria-hidden="true"></span> Counter
            </NavLink>
        </div>
        <div class="nav-item px-3">
            <NavLink class="nav-link" href="fetchdata">
                <span class="oi oi-list-rich" aria-hidden="true"></span> Fetch data
            </NavLink>
        </div>
    </nav>
</div>

@code {
    private bool collapseNavMenu = true;

    private string? NavMenuCssClass => collapseNavMenu ? "collapse" : null;

    private void ToggleNavMenu()
    {
        collapseNavMenu = !collapseNavMenu;
    }
}
```

13. In the Pages folder, open FetchData.razor and note that it defines a component that fetches weather forecasts from an injected dependency weather service and then renders them in a table, as shown in the following code:

```
@page "/fetchdata"

<PageTitle>Weather forecast</PageTitle>

@using Northwind.BlazorServer.Data
@inject WeatherForecastService ForecastService

<h1>Weather forecast</h1>

<p>This component demonstrates fetching data from a service.</p>

@if (forecasts == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <table class="table">
        <thead>
            <tr>
                <th>Date</th>
                <th>Temp. (C)</th>
                <th>Temp. (F)</th>
                <th>Summary</th>
            </tr>
        </thead>
        <tbody>
        @foreach (var forecast in forecasts)
{
    <tr>
        <td>@forecast.Date.ToShortDateString()</td>
        <td>@forecast.TemperatureC</td>
        <td>@forecast.TemperatureF</td>
        <td>@forecast.Summary</td>
    </tr>
}
    </tbody>
</table>
}

@code {
    private WeatherForecast[]? forecasts;

    protected override async Task OnInitializedAsync()
    {
        forecasts = await ForecastService.GetForecastAsync(DateTime.Now);
    }
}
```

14. In the Data folder, open WeatherForecastService.cs and note that it is not a Web API controller class; it is just an ordinary class that returns random weather data, as shown in the following code:

```
namespace Northwind.BlazorServer.Data
{
    public class WeatherForecastService
    {
        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm",
            "Balmy", "Hot", "Sweltering", "Scorching"
        };

        public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
        {
            return Task.FromResult(Enumerable.Range(1, 5)
                .Select(index => new WeatherForecast
                {
                    Date = startDate.AddDays(index),
                    TemperatureC = Random.Shared.Next(-20, 55),
                    Summary = Summaries[Random.Shared.Next(Summaries.Length)]
                }).ToArray());
        }
    }
}
```

## Understanding CSS and JavaScript isolation

Blazor components often need to provide their own CSS to apply styling or JavaScript for activities that cannot be performed purely in C#, like access to browser APIs. To ensure this does not conflict with site-level CSS and JavaScript, Blazor supports CSS and JavaScript isolation. If you have a component named Index.razor, simply create a CSS file named Index.razor.css. The styles defined within this file will override any other styles in the project.

## Understanding Blazor routing to page components

The Router component that we saw in the App.razor file enables routing to components. The markup for creating an instance of a component looks like an HTML tag where the name of the tag is the component type. Components can be embedded on a web page using an element, for example, <Rating Stars="5" />, or can be routed to like a Razor Page or MVC controller.

## How to define a routable page component

To create a routable page component, add the @page directive to the top of a component's .razor file, as shown in the following markup:

```
@page "customers"
```

The preceding code is the equivalent of an MVC controller decorated with the [Route] attribute, as shown in the following code:

```
[Route("customers")]
public class CustomersController
{
```

The Router component scans the assembly specifically in its AppAssembly parameter for components decorated with the [Route] attribute and registers their URL paths.

Any single-page component can have multiple @page directives to register multiple routes.

At runtime, the page component is merged with any specific layout that you have specified, just like an MVC view or Razor Page would be. By default, the Blazor Server project template defines MainLayout.razor as the layout for page components.

> **Good Practice:** By convention, put routable page components in the Pages folder.

## How to navigate Blazor routes

Microsoft provides a dependency service named NavigationManager that understands Blazor routing and the NavLink component.
