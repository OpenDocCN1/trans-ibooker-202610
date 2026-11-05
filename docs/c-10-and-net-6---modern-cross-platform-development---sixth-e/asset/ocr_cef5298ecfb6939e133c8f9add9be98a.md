```
<script src="_framework/blazor.server.js"></script>
</body>
</html>
```

While reviewing the preceding markup, note the following:

+   - `<div id="blazor-error-ui">` for showing Blazor errors that will appear as a yellow bar at the bottom of the web page when an error occurs
+   - The script block for blazor.server.js manages the SignalR connection back to the server

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

+   - If a matching route is found, then RouteView is executed that sets the default layout for the component to MainLayout and passes any route data parameters to the component.
+   - If a matching route is not found, then LayoutView is executed that renders the internal markup (in this case, a simple paragraph element with a message telling the visitor there is nothing at this address) inside MainLayout.

10. In the Shared folder, open MainLayout.razor and note that it defines <div> for a sidebar containing a navigation menu that is implemented by the NavMenu.razor component file in this project, and an HTML5 element such as <main> and <article> for the content, as shown in the following code:

```
@inherits LayoutComponentBase

<PageTitle>Northwind.BlazorServer</PageTitle>

<div class="page">
    <div class="sidebar">
```