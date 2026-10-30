```
// ------------------------------------------------------------
//
// Global MemberRefs
// ------------------------------------------------------------
//
// TypeDef #1
// ------------------------------------------------------------
//    TypDefName: CarLibrary.Car
//    Flags      : [Public] [AutoLayout] [Class] [Abstract] [AnsiClass] [BeforeFieldInit]
//    Extends    : [TypeRef] System.Object
//    Field #1
// ------------------------------------------------------------
//    Field Name: value__
//    Flags     : [Private]
//    CallCnvtnt: [FIELD]
//    Field type: String
//
```

As you can see, the .NET Core type metadata is verbose (the actual binary format is much more compact). In fact, if I were to list the entire metadata description representing the CarLibrary.dll assembly, it would span several pages. Given that this act would be a woeful waste of paper, let’s just glimpse into some key metadata descriptions of the CarLibrary.dll assembly.

> Note  Don’t be too concerned with the exact syntax of every piece of .NET Core metadata in the next few sections. The bigger point to absorb is that .NET Core metadata is very descriptive and lists each internally defined (and externally referenced) type found within a given code base.

# Viewing (Partial) Metadata for the EngineStateEnum Enumeration

Each type defined within the current assembly is documented using a TypeDef #n token (where TypeDef is short for type definition). If the type being described uses a type defined within a separate .NET Core assembly, the referenced type is documented using a TypeRef #n token (where TypeRef is short for type reference). A TypeRef token is a pointer (if you will) to the referenced type’s full metadata definition in an external assembly. In a nutshell, .NET Core metadata is a set of tables that clearly list all type definitions (TypeDefs) and referenced types (TypeRefs), all of which can be examined using ilasm.exe.

As far as CarLibrary.dll goes, one TypeDef is the metadata description of the CarLibrary.EngineStateEnum enumeration (your number may differ; TypeDef numbering is based on the order in which the C# compiler processes the file).

```
// TypeDef #2
// ------------------------------------------------------------
//    TypDefName: CarLibrary.EngineStateEnum
//    Flags      : [Public] [AutoLayout] [Class] [Sealed] [AnsiClass]
//    Extends    : [TypeRef] System.Enum
//    Field #1
// ------------------------------------------------------------
//    Field Name: value__
//    Flags     : [Public] [SpecialName] [RTSpecialName]
```