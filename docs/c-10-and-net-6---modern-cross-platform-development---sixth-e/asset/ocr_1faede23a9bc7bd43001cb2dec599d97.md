5. In the Pages folder, in Index.razor, delete the two Customers component instances and then add a button and a code block that uses the Blazor JavaScript runtime dependency service to call a JavaScript function, as shown in the following code:

```
<button type="button" class="btn btn-info" @onclick="AlertBrowser">
    Poke the browser</button>

<hr />

<input id="colorBox" />

<button type="button" class="btn btn-info" @onclick="SetColor">
    Set Color</button>

<button type="button" class="btn btn-info" @onclick="GetColor">
    Get Color</button>

@code {
    [Inject]
    public IJSRuntime JSRuntime { get; set; } = null!;

    public async Task AlertBrowser()
    {
        await JSRuntime.InvokeVoidAsync(
            "messageBox", "Blazor poking the browser");
    }

    public async Task SetColor()
    {
        await JSRuntime.InvokeVoidAsync("setColorInStorage");
    }

    public async Task GetColor()
    {
        await JSRuntime.InvokeVoidAsync("getColorFromStorage");
    }
}
```

6. Start the Northwind.BlazorServer project.

7. Start Chrome and navigate to https://localhost:5001/.

8. On the home page, in the textbox, enter red and then click the Set Color button.