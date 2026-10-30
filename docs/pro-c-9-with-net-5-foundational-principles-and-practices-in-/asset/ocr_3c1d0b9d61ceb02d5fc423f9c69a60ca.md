# Table 17-3. A Tiny Sampling of Predefined Attributes

| Attribute | Meaning in Life |
|---|---|
| [CLSCompliant] | Enforces the annotated item to conform to the rules of the Common Language Specification (CLS). Recall that CLS-compliant types are guaranteed to be used seamlessly across all .NET Core programming languages. |
| [DllImport] | Allows .NET Core code to make calls to any unmanaged C- or C++-based code library, including the API of the underlying operating system. |
| [Obsolete] | Marks a deprecated type or member. If other programmers attempt to use such an item, they will receive a compiler warning describing the error of their ways. |

Understand that when you apply attributes in your code, the embedded metadata is essentially useless until another piece of software explicitly reflects over the information. If this is not the case, the blurb of metadata embedded within the assembly is ignored and completely harmless.

# Attribute Consumers

As you would guess, the .NET Core Framework ships with numerous utilities that are indeed on the lookout for various attributes. The C# compiler (csc.exe) itself has been preprogrammed to discover the presence of various attributes during the compilation cycle. For example, if the C# compiler encounters the [CLSCompliant] attribute, it will automatically check the attributed item to ensure it is exposing only CLS-compliant constructs. By way of another example, if the C# compiler discovers an item attributed with the [Obsolete] attribute, it will display a compiler warning in the Visual Studio Error List window.

In addition to development tools, numerous methods in the .NET Core base class libraries are preprogrammed to reflect over specific attributes. Chapter 20 introduces XML and JSON serialization, both of which use attributes to control the serialization process.

Finally, you are free to build applications that are programmed to reflect over your own custom attributes, as well as any attribute in the .NET Core base class libraries. By doing so, you are essentially able to create a set of “keywords” that are understood by a specific set of assemblies.

# Applying Attributes in C#

To illustrate the process of applying attributes in C#, create a new Console Application project named ApplyingAttributes and add a reference to System.Text.Json. Assume you want to build a class named Motorcycle that can be persisted to JSON format. If you have a field that should not be exported to JSON, you can apply the [JsonIgnore] attribute.

```
public class Motorcycle
{
    [JsonIgnore]
    public float weightOfCurrentPassengers;
    // These fields are still serializable.
    public bool hasRadioSystem;
    public bool hasHeadSet;
    public bool hasSissyBar;
}
```