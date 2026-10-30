If a type implementing IAppFunctionality is found, the DoIt() method is called and then sent to the DisplayCompanyData() method to output additional information from the reflected type.

```
static void LoadExternalModule(string assemblyName)
{
    Assembly theSnapInAsm = null;
    try
    {
        // Dynamically load the selected assembly.
        theSnapInAsm = Assembly.LoadFrom(assemblyName);
    }
    catch (Exception ex)
    {
        Console.WriteLine($"An error occurred loading the snapin: {ex.Message}");
        return;
    }

    // Get all IAppFunctionality compatible classes in assembly.
    var theClassTypes = theSnapInAsm
        .GetTypes()
        .Where(t => t.IsClass && (t.GetInterface("IAppFunctionality") != null))
        .ToList();
    if (!theClassTypes.Any())
    {
        Console.WriteLine("Nothing implements IAppFunctionality!");
    }

    // Now, create the object and call DoIt() method.
    foreach (Type t in theClassTypes)
    {
        // Use late binding to create the type.
        IAppFunctionality itfApp = (IAppFunctionality) theSnapInAsm.CreateInstance(t.FullName, true);
        itfApp?.DoIt();
        // Show company info.
        DisplayCompanyData(t);
    }
}
```

The final task is to display the metadata provided by the [CompanyInfo] attribute. Create the DisplayCompanyData() method as follows. Notice this method takes a single System.Type parameter.

```
static void DisplayCompanyData(Type t)
{
    // Get [CompanyInfo] data.
    var compInfo = t
        .GetCustomAttributes(false)
        .Where(ci => (ci is CompanyInfoAttribute));
    // Show data.
    foreach (CompanyInfoAttribute c in compInfo)
```