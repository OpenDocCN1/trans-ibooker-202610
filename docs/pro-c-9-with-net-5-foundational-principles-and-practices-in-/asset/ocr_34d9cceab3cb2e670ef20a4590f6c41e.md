# Table 20-3. Key Members of the DirectoryInfo Type

| Member | Meaning in Life |
|---|---|
| Create() | Creates a directory (or set of subdirectories) when given a path name |
| CreateSubdirectory() |  |
| Delete() | Deletes a directory and all its contents |
| GetDirectories() | Returns an array of DirectoryInfo objects that represent all subdirectories in the current directory |
| GetFiles() | Retrieves an array of FileInfo objects that represent a set of files in the given directory |
| MoveTo() | Moves a directory and its contents to a new path |
| Parent | Retrieves the parent directory of this directory |
| Root | Gets the root portion of a path |

You begin working with the DirectoryInfo type by specifying a particular directory path as a constructor parameter. Use the dot (.) notation if you want to obtain access to the current working directory (the directory of the executing application). Here are some examples:

```
// Bind to the current working directory.
DirectoryInfo dir1 = new DirectoryInfo(".");
// Bind to C:\Windows,
// using a verbatim string.
DirectoryInfo dir2 = new DirectoryInfo(@"C:\Windows");
```

In the second example, you assume that the path passed into the constructor (C:\Windows) already exists on the physical machine. However, if you attempt to interact with a nonexistent directory, a System.IO.DirectoryNotFoundException is thrown. Thus, if you specify a directory that is not yet created, you need to call the Create() method before proceeding, like so:

```
// Bind to a nonexistent directory, then create it.
DirectoryInfo dir3 = new DirectoryInfo(@"C:\MyCode\Testing");
dir3.Create();
```

The path syntax used in the previous example is Windows-centric. If you are developing .NET Core applications for different platforms, you should use the Path.VolumeSeparatorChar and Path.DirectorySeparatorChar constructs, which will yield the appropriate characters based on the platform. Update the previous code to the following:

```
DirectoryInfo dir3 = new DirectoryInfo(
$@"C{Path.VolumeSeparatorChar}{Path.DirectorySeparatorChar}MyCode{Path.DirectorySeparatorChar}Testing");
```

After you create a DirectoryInfo object, you can investigate the underlying directory contents using any of the properties inherited from FileSystemInfo. To see this in action, create a new Console Application project named DirectoryApp and update your C# file to import System and System.IO. Update your Program