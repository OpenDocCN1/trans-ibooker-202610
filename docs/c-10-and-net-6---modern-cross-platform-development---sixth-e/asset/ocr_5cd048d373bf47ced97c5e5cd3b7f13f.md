```
<ProjectReference
    Include="../PacktLibraryModern/PacktLibraryModern.csproj" />
</ItemGroup>
</Project>
```

3. Build the PeopleApp project.

# Defining flight passengers

In this example, we will define some classes that represent various types of passengers on a flight and then we will use a switch expression with pattern matching to determine the cost of their flight.

+   1. In the PacktLibraryModern project/folder, rename the file Class1.cs to FlightPatterns.cs.
+   2. In FlightPatterns.cs, add statements to define three types of passengers with different properties, as shown in the following code:

```
namespace Packt.Shared; // C# 10 file-scoped namespace

public class BusinessClassPassenger
{
    public override string ToString()
    {
        return $"Business Class";
    }
}

public class FirstClassPassenger
{
    public int AirMiles { get; set; }

    public override string ToString()
    {
        return $"First Class with {AirMiles:N0} air miles";
    }
}

public class CoachClassPassenger
{
    public double CarryOnKG { get; set; }

    public override string ToString()
    {
        return $"Coach Class with {CarryOnKG:N2} KG carry on";
    }
}
```