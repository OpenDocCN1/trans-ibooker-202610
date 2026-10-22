
├── Pages
│   ├── Counter.razor
│   ├── FetchData.razor
│   └── Index.razor
├── Program.cs
├── Properties
│   └── launchSettings.json
├── Shared
│   ├── MainLayout.razor
│   ├── MainLayout.razor.css
│   ├── NavMenu.razor
│   ├── NavMenu.razor.css
│   └── SurveyPrompt.razor
├── _Imports.razor
├── bin
... <====== A bunch of .NET dependencies removed
├── blazor-web.csproj
├── obj
... <====== A bunch of .NET dependencies removed
└── wwwroot
    ├── css
    │   ├── app.css
    │   ├── bootstrap
    │   │   ├── bootstrap.min.css
    │   │   └── bootstrap.min.css.map
    │   └── open-iconic
    │       ├── FONT-LICENSE
    │       ├── ICON-LICENSE
    │       ├── README.md
    │       └── font
    │           ├── css
    │           │   └── open-iconic-bootstrap.min.css
    │           └── fonts
    │               ├── open-iconic.eot
    │               ├── open-iconic.otf
    │               ├── open-iconic.svg
    │               ├── open-iconic.ttf
    │               └── open-iconic.woff
    ├── favicon.ico
    ├── index.html
    └── sample-data
        └── weather.json

After the application-specific files and a metric ton of .NET dependencies, we see a directory called wwwroot which represents various assets and resources that will need to be served up to complete the application. This includes fonts, some sample JSON data, and a minimized Bootstrap base for the application.

I cannot give a detailed tutorial on Razor components and everything that goes into them, but I want to walk through the basics of the application to allow you to thread the needle with respect to its functionality. The main views are stored in the Pages directory. Shared layouts are stored in the Shared directory. The main application file shown in Example 13-6 is in App.razor in the main project directory.

## Example 13-6. The top level application component defined in Razor

```
<Router AppAssembly="@typeof(Program).Assembly" PreferExactMatches="@true">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

This represents high level details of the application including the default layout identified as a MainLayout. If it is unable to find a working layout, notice it has a default message. This is an SPA we are expecting to be served over HTTP, so we do need a mechanism for routing endpoints to pages. We will see those details shortly.

In Example 13-7 we have the default layout from Shared/MainLayout.razor. We can see that it inherits behavior from a LayoutComponentBase and is comprised by a NavMenu in a side bar <div> element on the left. This highlights that Razor components mix C# object interactions and HTML layouts. It is a bit of a funky syntax, but once you get used to it, it is not especially onerous.

## Example 13-7. The default layout is a shared template

```
@inherits LayoutComponentBase

<div class="page">
<div class="sidebar">
    <NavMenu />
</div>

<div class="main">
    <div class="top-row px-4">
        <a href="http://blazor.net" target="_blank" class="ml-md-auto">About</a>
    </div>

    <div class="content px-4">
        @Body
    </div>
</div>
</div>
```

By convention, the file *Index.razor* holds the landing page just like most websites are defined by a file called *index.html*. In Example 13-8, the body of this component will be inserted into the `@Body` location in the `MainLayout` when the default route is invoked.

## Example 13-8. The default route points to Index.razor in the Pages directory

```
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Title="How is Blazor working for you?" />
```

We see our first `@page` directive above which indicates to the main application that default HTTP requests to "/" should point to this rendered page. We see our typical friendly greeting rendered as HTML as well as an embedded component for a survey.

As this page and its assets do need to be served over HTTP, we will start it up using the following command.

```
brian@tweezer ~/g/w/s/blazor-web> dotnet run
Building...
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
```

```
info: Microsoft.Hosting.Lifetime[0]
    Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[0]
    Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
    Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
    Content root path: /Users/brian/src/blazor-web
```

In Figure 13-5 you will see the generated output if you point your browser to http://localhost:5000. If you wish to see all of the .NET assemblies and the runtime loaded, make sure to open up the network view first. Note that if you click on the survey link, it will take you to an actual SurveyMonkey survey!

![](img/16b7990eb0dbf589db177bdbd2f4a189_290_0.png)

Figure 13-5. Generated Blazor WebAssembly application in action

Routing to a read-only rendered HTML snippet is cool and all, but how about maintaining some state and using non-default routes? The Counter link on the left in the sidebar will take you to a view rendered from the *Pages/Counter.razor* file shown in Example 13-9.

## Example 13-9. A page with stateful fields and interactive UI elements

```
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

We see some more interesting behavior in this code sample. First, you will notice the `@page` directive points to a location other than the default route so you can imagine setting up multiple views for your SPA. You see some more of the funky syntax, but this time we define a `<button>` element and the behavior we wish to invoke when the user presses it. In this case, the `@onclick` hint tells us we want to call a function called `IncrementCount` on our component. This is defined in the `@code` block further down as is the state the component will maintain in a field called `currentCount`. This is initialized to 0 but is incremented every time the button is pressed.

While you may not choose to embed your components directly in the *.Razor* files like this, it is at least possible to do so and makes for a pretty simple effort to get state maintained and reflected in the view. Click on the `Counter` link and see it in action. Stop the server and try printing out a message to the console using `Console.WriteLine` every time the button is pressed. Spend a minute marveling how far you have come in this book compared to our early laments about WebAssembly not supporting strings.

The final thing I want to point out here is the contents of the Pages/FetchData.razor file shown in Example 13-10. This includes a bunch of details that you would need to handle for a more useful SPA such as fetching remote data, using conditional logic in the view, and iterating over the data that you have fetched.

## Example 13-10. A more sophisticated page with a dependency-injected HttpClient

```
@page "/fetchdata"
@inject HttpClient Http

<h1>Weather forecast</h1>

<p>This component demonstrates fetching data from the server.</p>

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
```
