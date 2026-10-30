```
<AssemblyVersion>1.0.0.1</AssemblyVersion>
<FileVersion>1.0.0.2</FileVersion>
<Version>1.0.0.3</Version>
</PropertyGroup>
```

> Note The rest of the entries from Figure 16-4 (and the project file listing) are used when generating NuGet packages from your assembly. This is covered later in the chapter.

# Exploring the CIL

Recall that an assembly does not contain platform-specific instructions; rather, it contains platform-agnostic Common Intermediate Language (CIL) instructions. When the .NET Core runtime loads an assembly into memory, the underlying CIL is compiled (using the JIT compiler) into instructions that can be understood by the target platform. For example, the TurboBoost() method of the SportsCar class is represented by the following CIL:

```
.method public hidebysig virtual
    instance void TurboBoost() cil managed
{
    .maxstack 8
    IL_0000: nop
    IL_0001: ldstr "Ramming speed! Faster is better..."
    IL_0006: call void [System.Console]System.Console::WriteLine(string)
    IL_000b: nop
    IL_000c: ret
}
// end of method SportsCar::TurboBoost
```

As with the other CIL examples in this book, most .NET Core developers don’t need to be deeply concerned with the details. Chapter 19 provides more details on its syntax and semantics, which can be helpful when you are building more complex applications that require advanced services, such as runtime construction of assemblies.

# Exploring the Type Metadata

Before you build some applications that use your custom .NET library, examine the metadata for the types within the CarLibrary.dll assembly. For an example, here is the TypeDef for the EngineStateEnum:

```
TypeDef #1 (02000002)
--------------------------------------------------
TypeDefName: CarLibrary.EngineStateEnum
Flags      : [Public] [AutoLayout] [Class] [Sealed] [AnsiClass]
Extends    : [TypeRef] System.Enum
Field #1
--------------------------------------------------
Field Name: value__
Flags      : [Public] [SpecialName] [RTSpecialName]
CallCnvtnt: [FIELD]
```