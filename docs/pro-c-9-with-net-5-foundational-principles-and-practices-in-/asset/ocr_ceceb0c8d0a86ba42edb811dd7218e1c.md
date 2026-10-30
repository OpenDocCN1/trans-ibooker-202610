# The Operating System (Windows) File Header

The operating system file header establishes the fact that the assembly can be loaded and manipulated by the target operating system (in the following example, Windows). This header data also identifies the kind of application (console-based, GUI-based, or *.dll code library) to be hosted by the operating system.

Open the CarLibrary.dll file (in the book’s repo or created later in this chapter) using the dumpbin.exe utility (via the developer command prompt) with the /headers flag as so:

```
dumpbin /headers CarLibrary.dll
```

This displays the assembly’s operating system header information (shown in the following when built for Windows). Here is the (partial) Windows header information for CarLibrary.dll:

```
Dump of file carlibrary.dll
PE signature found
File Type: DLL

FILE HEADER VALUES
    14C machine (x86)
    3 number of sections
BB89DC3D time date stamp
    0 file pointer to symbol table
    0 number of symbols
E0 size of optional header
2022 characteristics
    Executable
    Application can handle large (>2GB) addresses
    DLL
...
```

Now, remember that most .NET Core programmers will never need to concern themselves with the format of the header data embedded in a .NET Core assembly. Unless you happen to be building a new .NET Core language compiler (where you would care about such information), you are free to remain blissfully unaware of the grimy details of the header data. Do be aware, however, that this information is used under the covers when the operating system loads the binary image into memory.

# The CLR File Header

The CLR header is a block of data that all .NET Core assemblies must support (and do support, courtesy of the C# compiler) to be hosted by the .NET Core Runtime. In a nutshell, this header defines numerous flags that enable the runtime to understand the layout of the managed file. For example, flags exist that identify the location of the metadata and resources within the file, the version of the runtime the assembly was built against, the value of the (optional) public key, and so forth. Execute dumpbin.exe again with the /clrheader flag.

```
dumpbin /clrheader CarLibrary.dll
```