6. Modify the OnParametersSetAsync method to call the service, as shown highlighted in the following code:

```
protected override async Task OnParametersSetAsync()
{
    if (string.IsNullOrWhiteSpace(Country))
    {
        customers = await service.GetCustomersAsync();
    }
    else
    {
        customers = await service.GetCustomersAsync(Country);
    }
}
```

7. Start the Northwind.BlazorServer website project and confirm that it retains the same functionality as before.

# Defining forms using the EditForm component

Microsoft provides ready-made components for building forms. We will use them to provide, create, and edit functionality for customers.

Microsoft provides the EditForm component and several form elements such as InputText to make it easier to use forms with Blazor.

EditForm can have a model set to bind it to an object with properties and event handlers for custom validation, as well as recognizing standard Microsoft validation attributes on the model class, as shown in the following code:

```
<EditForm Model="@customer" OnSubmit="ExtraValidation">
    <DataAnnotationsValidator />
    <ValidationSummary />
    <InputText id="name" @bind-Value="customer.CompanyName" />
    <button type="submit">Submit</button>
</EditForm>

@code {
    private Customer customer = new();

    private void ExtraValidation()
    {
        // perform any extra validation
    }
}
```

As an alternative to a ValidationSummary component, you can use the ValidationMessage component to show a message next to an individual form element.