```
// TypeRef #19
// --------------------------------------------------
// Token:          0x01000013
// ResolutionScope: 0x23000001
// TypeRefName:    System.Enum
```

# Documenting the Defining Assembly

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

# Documenting Referenced Assemblies

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