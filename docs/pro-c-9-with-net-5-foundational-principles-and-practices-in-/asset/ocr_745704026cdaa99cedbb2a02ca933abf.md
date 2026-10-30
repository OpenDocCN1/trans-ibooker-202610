```
// Create new assembly.
var builder = AssemblyBuilder.DefineDynamicAssembly(
    assemblyName, AssemblyBuilderAccess.Run);
```

# The Role of the ModuleBuilder Type

ModuleBuilder is the key type used during the development of dynamic assemblies. As you would expect, ModuleBuilder supports several members that allow you to define the set of types contained within a given module (classes, interfaces, structures, etc.) as well as the set of embedded resources (string tables, images, etc.) contained within. Table 19-10 describes two of the creation-centric methods. (Do note that each method will return to you a related type that represents the type you want to construct.)

# Table 19-10. Select Members of the ModuleBuilder Type

| Method | Meaning in Life |
|---|---|
| DefineEnum() | Used to emit a .NET enum definition |
| DefineType() | Constructs a TypeBuilder, which allows you to define value types, interfaces, and class types (including delegates) |

The key member of the ModuleBuilder class to be aware of is DefineType(). In addition to specifying the name of the type (via a simple string), you will also use the System.Reflection.TypeAttributes enum to describe the format of the type itself. Table 19-11 lists some (but not all) of the key members of the TypeAttributes enumeration.

# Table 19-11. Select Members of the TypeAttributes Enumeration

| Member | Meaning in Life |
|---|---|
| Abstract | Specifies that the type is abstract |
| Class | Specifies that the type is a class |
| Interface | Specifies that the type is an interface |
| NestedAssembly | Specifies that the class is nested with assembly visibility and is thus accessible only by methods within its assembly |
| NestedFamANDAssem | Specifies that the class is nested with assembly and family visibility and is thus accessible only by methods lying in the intersection of its family and assembly |
| NestedFamily | Specifies that the class is nested with family visibility and is thus accessible only by methods within its own type and any subtypes |
| NestedFamORAssem | Specifies that the class is nested with family or assembly visibility and is thus accessible only by methods lying in the union of its family and assembly |
| NestedPrivate | Specifies that the class is nested with private visibility |
| NestedPublic | Specifies that the class is nested with public visibility |
| NotPublic | Specifies that the class is not public |
| Public | Specifies that the class is public |
| Sealed | Specifies that the class is concrete and cannot be extended |
| Serializable | Specifies that the class can be serialized |