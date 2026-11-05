1. In NullHandling.csproj, in Program.cs, at the bottom of the file, add statements to declare an Address class with four fields, as shown in the following code:

```
class Address
{
    public string? Building;
    public string Street;
    public string City;
    public string Region;
}
```

2. After a few seconds, note the warnings about non-nullable fields, like Street not being initialized, as shown in Figure 6.2:

![](bbox=[0.17, 0.3, 0.93, 0.5])

Figure 6.2: Warning messages about non-nullable fields in the PROBLEMS window

3. Assign the empty string value to each of the three fields that are non-nullable, as shown in the following code:

```
public string Street = string.Empty;
public string City = string.Empty;
public string Region = string.Empty;
```

4. In Program.cs, at the top of the file, statically import Console and then add statements to instantiate an Address and set its properties, as shown in the following code:

```
Address address = new();
address.Building = null;
address.Street = null;
address.City = "London";
address.Region = null;
```