Suppose your business requirements specified to only allow updating the PetName and Color fields in the Create() method. Adding the Bind attribute (shown in the following example) limits the properties that participate in binding and instructs the model binder to ignore the rest.

```
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Create(
[Bind(nameof(Car.PetName),nameof(Car.Color))]Car car)
{
    if (ModelState.IsValid)
    {
        //Save the data;
    }
    //Allow the user to retry
}
```

The Bind attribute can also be used to specify a prefix for the property names. If the names of the name-value pairs have a prefix added when sent to the action method, the Bind attribute is used to inform the ModelBinder how you map the names to the type's properties. The following example sets a prefix for the names and allows all properties to be bound:

```
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Create(
[Bind(Prefix="MakeList")]Car car)
{
    if (ModelState.IsValid)
    {
        //Save the data;
    }
}
```

# Controlling Model Binding Sources in ASP.NET Core

Binding sources can be controlled through a set of attributes on the action parameters. Custom model binders can also be created; however, that is beyond the scope of this book. Table 29-4 lists the attributes that can be used to control model binding.