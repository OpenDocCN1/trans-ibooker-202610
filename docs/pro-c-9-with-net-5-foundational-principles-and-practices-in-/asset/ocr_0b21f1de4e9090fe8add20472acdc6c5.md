Note Recall from Chapter 1 that all .NET Core assemblies (class libraries or console apps) are compiled to assemblies that have a *.dll extension. They are executed using the .NET Core CLI. New in .NET Core 3+ (and later), the dotnet.exe file is copied into the output directory and renamed to match the assembly name. So, while it looks like your project was compiled to RoundTrip.exe, it was compiled to RoundTrip.dll with dotnet.exe copied to RoundTrip.exe along with the required command-line arguments needed to execute Roundtrip.dll.

Next execute ildasm.exe against RoundTrip.dll using the following command (executed from the solution folder level):

```
ildasm /all /METADATA /out=..\RoundTrip\RoundTrip.il .\RoundTrip\bin\Debug\net5.0\RoundTrip.dll
```

Note ildasm.exe will also generate a *.res file when dumping the contents of an assembly to file. These resource files can be ignored (and deleted) throughout this chapter, as you will not be using them. This file contains some low-level CLR security information (among other things).

Now you can view RoundTrip.il using your text editor of choice. Here is the (slightly reformatted and annotated) result:

```
// Referenced assemblies.
.assembly extern System.Runtime
{
    .publickeytoken = (B0 3F 5F 7F 11 D5 0A 3A)
    .ver 5:0:0:0
}
.assembly extern System.Console
{
    .publickeytoken = (B0 3F 5F 7F 11 D5 0A 3A )
    .ver 5:0:0:0
}

// Our assembly.
.assembly RoundTrip
{
    ...
    .hash algorithm 0x00008004
    .ver 1:0:0:0
}
.module RoundTrip.dll
.imagebase 0x00400000
.file alignment 0x00000200
.stackreserve 0x00100000
.subsystem 0x0003
.corflags 0x00000001
```