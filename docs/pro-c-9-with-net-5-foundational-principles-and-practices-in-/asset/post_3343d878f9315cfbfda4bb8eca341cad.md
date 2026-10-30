
```
hasThis
ReturnType: String
No arguments.

...
```

```
//  Method #2
//  ------------------------------------------------
//      MethodName: set_PetName
//      Flags      : [Public] [HideBySig] [ReuseSlot] [SpecialName]
//      RVA        : 0x00002058
//      ImplFlags  : [IL] [Managed]
//      CallCnvtn : [DEFAULT]
//      hasThis
//      ReturnType: Void
//      1 Arguments
//      Argument #1: String
//      1 Parameters
//          (1) ParamToken : Name : value flags: [none]
...
```

```
//  Property #1
//  ------------------------------------------------
//      Prop.Name  : PetName
//      Flags      : [none]
//      CallCnvtn: [PROPERTY]
//      hasThis
//      ReturnType: String
//      No arguments.
//      DefltValue:
//      Setter     : set_PetName
//      Getter     : get_PetName
//      0 Others
...
```

First, note that the Car class metadata marks the type's base class (System.Object) and includes various flags that describe how this type was constructed (e.g., [Public], [Abstract], and whatnot). Methods (such as the Car's constructor) are described by their parameters, return value, and name.

Note how an automatic property results in a compiler-generated private backing field (which was named <PetName>k__BackingField) and two compiler-generated methods (in the case of a read-write property) named, in this example, get_PetName() and set_PetName(). Finally, the actual property is mapped to the internal get/set methods using the .NET Core metadata Getter/Setter tokens.

## Examining a TypeRef

Recall that an assembly's metadata will describe not only the set of internal types (Car, EngineStateEnum, etc.) but also any external types the internal types reference. For example, given that CarLibrary.dll has defined two enumerations, you find a TypeRef block for the System.Enum type, as follows:

```
// TypeRef #19
// --------------------------------------------------
// Token:          0x01000013
// ResolutionScope: 0x23000001
// TypeRefName:    System.Enum
```

## Documenting the Defining Assembly

The CarLibrary.il file also allows you to view the .NET Core metadata that describes the assembly itself using the Assembly token. The following is a partial dump of the manifest of CarLibrary.dll:

```
// Assembly
// --------------------------------------------------
// Token: 0x20000001
// Name : CarLibrary
// Public Key  :
// Hash Algorithm : 0x00008004
// Version: 1.0.0.1
// Major Version: 0x00000001
// Minor Version: 0x00000000
// Build Number: 0x00000000
// Revision Number: 0x00000001
// Locale: <null>
// Flags : [none] (00000000)
```

## Documenting Referenced Assemblies

In addition to the Assembly token and the set of TypeDef and TypeRef blocks, .NET Core metadata also makes use of AssemblyRef #n tokens to document each external assembly. Given that each .NET Core assembly references the System.Runtime base class library assembly, you find an AssemblyRef for the System.Runtime assembly, as shown in the following code:

```
// AssemblyRef #1 (23000001)
// --------------------------------------------------
// Token: 0x23000001
// Public Key or Token: b0 3f 5f 7f 11 d5 0a 3a
// Name: System.Runtime
// Version: 5.0.0.0
// Major Version: 0x00000005
// Minor Version: 0x00000000
// Build Number: 0x00000000
// Revision Number: 0x00000000
// Locale: <null>
// HashValue Blob:
// Flags: [none] (00000000)
```

## Documenting String Literals

The final point of interest regarding .NET Core metadata is the fact that every string literal in your code base is documented under the User Strings token.

```
// User Strings
// ------------------------------------------------------------
// 70000001 : (23) L"CarLibrary Version 2.0!"
// 70000031 : (13) L"Quiet time..."
// 7000004d : (11) L"Jamming {0}"
// 70000065 : (32) L"Eek! Your engine block exploded!"
// 700000a7 : (34) L"Ramming speed! Faster is better..."
```

Note As illustrated in this last metadata listing, always be aware that all strings are clearly documented in the assembly metadata. This could have huge security consequences if you were to use string literals to represent passwords, credit card numbers, or other sensitive information.

The next question on your mind may be (in the best-case scenario) "How can I leverage this information in my applications?" or (in the worst-case scenario) "Why should I care about metadata?" To address both points of view, allow me to introduce .NET Core reflection services. Be aware that the usefulness of the topics presented over the pages that follow may be a bit of a head-scratcher until this chapter's endgame. So, hang tight.

Note You will also find a number of CustomAttribute tokens displayed by the METAINFO section, which documents the attributes applied within the code base. You will learn about the role of .NET Core attributes later in this chapter.

## Understanding Reflection

In the .NET Core universe, reflection is the process of runtime type discovery. Using reflection services, you can programmatically obtain the same metadata information generated by ildasm.exe using a friendly object model. For example, through reflection, you can obtain a list of all types contained within a given *.dll or *.exe assembly, including the methods, fields, properties, and events defined by a given type. You can also dynamically discover the set of interfaces supported by a given type, the parameters of a method, and other related details (base classes, namespace information, manifest data, etc.).

Like any namespace, System.Reflection (which is defined in System.Runtime.dll) contains several related types. Table 17-1 lists some of the core items you should be familiar with.

### Table 17-1. A Sampling of Members of the System.Reflection Namespace

| Type | Meaning in Life |
|------|-----------------|
| Assembly | This abstract class contains members that allow you to load, investigate, and manipulate an assembly. |
| AssemblyName | This class allows you to discover numerous details behind an assembly's identity (version information, culture information, etc.). |
| EventInfo | This abstract class holds information for a given event. |
| FieldInfo | This abstract class holds information for a given field. |
| MemberInfo | This is the abstract base class that defines common behaviors for the EventInfo, FieldInfo, MethodInfo, and PropertyInfo types. |
| MethodInfo | This abstract class contains information for a given method. |
| Module | This abstract class allows you to access a given module within a multifile assembly. |
| ParameterInfo | This class holds information for a given parameter. |
| PropertyInfo | This abstract class holds information for a given property. |

To understand how to leverage the System.Reflection namespace to programmatically read .NET Core metadata, you need to first come to terms with the System.Type class.

## The System.Type Class

The System.Type class defines members that can be used to examine a type's metadata, a great number of which return types from the System.Reflection namespace. For example, Type.GetMethods() returns an array of MethodInfo objects, Type.GetFields() returns an array of FieldInfo objects, and so on. The complete set of members exposed by System.Type is quite expansive; however, Table 17-2 offers a partial snapshot of the members supported by System.Type (see the .NET Core documentation for full details).

### Table 17-2. Select Members of System.Type

| Member | Meaning in Life |
|--------|-----------------|
| IsAbstract | These properties (among others) allow you to discover a number of basic traits about the Type you are referring to (e.g., if it is an abstract entity, an array, a nested class, etc.). |
| IsArray |  |
| IsClass |  |
| IsCOMObject |  |
| IsEnum |  |
| IsGenericTypeDefinition |  |
| IsGenericParameter |  |
| IsInterface |  |
| IsPrimitive |  |
| IsNestedPrivate |  |
| IsNestedPublic |  |
| IsSealed |  |
| IsValueType |  |

Table 17-2. (continued)

| Member | Meaning in Life |
|---|---|
| GetConstructors() | These methods (among others) allow you to obtain an array representing |
| GetEvents() | the items (interface, method, property, etc.) you are interested in. Each |
| GetFields() | method returns a related array (e.g., GetFields() returns a FieldInfo array, |
| GetInterfaces() | GetMethods() returns a MethodInfo array, etc.). Be aware that each of these |
| GetMembers() | methods has a singular form (e.g., GetMethod(), GetProperty(), etc.) that |
| GetMethods() | allows you to retrieve a specific item by name, rather than an array of all |
| GetNestedTypes() | related items. |
| GetProperties() | |
| FindMembers() | This method returns a MemberInfo array based on search criteria. |
| GetType() | This static method returns a Type instance given a string name. |
| InvokeMember() | This method allows "late binding" for a given item. You'll learn about late binding later in this chapter. |

## Obtaining a Type Reference Using System.Object.GetType()

You can obtain an instance of the Type class in a variety of ways. However, the one thing you cannot do is directly create a Type object using the new keyword, as Type is an abstract class. Regarding your first choice, recall that System.Object defines a method named GetType(), which returns an instance of the Type class that represents the metadata for the current object.

```
// Obtain type information using a SportsCar instance.
SportsCar sc = new SportsCar();
Type t = sc.GetType();
```

Obviously, this approach will work only if you have compile-time knowledge of the type you want to reflect over (SportsCar in this case) and currently have an instance of the type in memory. Given this restriction, it should make sense that tools such as ildasm.exe do not obtain type information by directly calling System.Object.GetType() for each type, given that ildasm.exe was not compiled against your custom assemblies.

## Obtaining a Type Reference Using typeof()

The next way to obtain type information is using the C# typeof operator, like so:

```
// Get the type using typeof.
Type t = typeof(SportsCar);
```

Unlike System.Object.GetType(), the typeof operator is helpful, in that you do not need to first create an object instance to extract type information. However, your code base must still have compile-time knowledge of the type you are interested in examining, as typeof expects the strongly typed name of the type.
