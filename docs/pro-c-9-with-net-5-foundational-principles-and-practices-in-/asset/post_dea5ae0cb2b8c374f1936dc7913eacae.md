
## Table 20-1. Key Members of the System.IO Namespace

| Nonabstract I/O Class Type | Meaning in Life |
|---|---|
| BinaryReader<br>BinaryWriter | These classes allow you to store and retrieve primitive data types (integers, Booleans, strings, and whatnot) as a binary value. |
| BufferedStream | This class provides temporary storage for a stream of bytes that you can commit to storage later. |
| Directory<br>DirectoryInfo | You use these classes to manipulate a machine's directory structure. The Directory type exposes functionality using static members, while the DirectoryInfo type exposes similar functionality from a valid object reference. |
| DriveInfo | This class provides detailed information regarding the drives that a given machine uses. |
| File<br>FileInfo | You use these classes to manipulate a machine's set of files. The File type exposes functionality using static members, while the FileInfo type exposes similar functionality from a valid object reference. |
| FileStream | This class gives you random file access (e.g., seeking capabilities) with data represented as a stream of bytes. |
| FileSystemWatcher | This class allows you to monitor the modification of external files in a specified directory. |
| MemoryStream | This class provides random access to streamed data stored in memory rather than in a physical file. |
| Path | This class performs operations on System.String types that contain file or directory path information in a platform-neutral manner. |
| StreamWriter<br>StreamReader | You use these classes to store (and retrieve) textual information to (or from) a file. These types do not support random file access. |
| StringWriter<br>StringReader | Like the StreamReader/StreamWriter classes, these classes also work with textual information. However, the underlying storage is a string buffer rather than a physical file. |

In addition to these concrete class types, System.IO defines several enumerations, as well as a set of abstract classes (e.g., Stream, TextReader, and TextWriter), that define a shared polymorphic interface to all descendants. You will read about many of these types in this chapter.

## The Directory(Info) and File(Info) Types

System.IO provides four classes that allow you to manipulate individual files, as well as interact with a machine's directory structure. The first two types, Directory and File, expose creation, deletion, copying, and moving operations using various static members. The closely related FileInfo and DirectoryInfo types expose similar functionality as instance-level methods (therefore, you must allocate them with the new keyword). The Directory and File classes directly extend System.Object, while DirectoryInfo and FileInfo derive from the abstract FileSystemInfo type.

FileInfo and DirectoryInfo typically serve as better choices for obtaining full details of a file or directory (e.g., time created or read/write capabilities) because their members tend to return strongly typed objects. In contrast, the Directory and File class members tend to return simple string values rather than strongly typed objects. This is only a guideline, however; in many cases, you can get the same work done using File/FileInfo or Directory/DirectoryInfo.

## The Abstract FileSystemInfo Base Class

The DirectoryInfo and FileInfo types receive many behaviors from the abstract FileSystemInfo base class. For the most part, you use the members of the FileSystemInfo class to discover general characteristics (such as time of creation, various attributes, etc.) about a given file or directory. Table 20-2 lists some core properties of interest.

### Table 20-2. FileSystemInfo Properties

| Property | Meaning in Life |
|---|---|
| Attributes | Gets or sets the attributes associated with the current file that are represented by the FileAttributes enumeration (e.g., is the file or directory read-only, encrypted, hidden, or compressed?) |
| CreationTime | Gets or sets the time of creation for the current file or directory |
| Exists | Determines whether a given file or directory exists |
| Extension | Retrieves a file's extension |
| FullName | Gets the full path of the directory or file |
| LastAccessTime | Gets or sets the time the current file or directory was last accessed |
| LastWriteTime | Gets or sets the time when the current file or directory was last written to |
| Name | Obtains the name of the current file or directory |

FileSystemInfo also defines the Delete() method. This is implemented by derived types to delete a given file or directory from the hard drive. Also, you can call Refresh() prior to obtaining attribute information to ensure that the statistics regarding the current file (or directory) are not outdated.

## Working with the DirectoryInfo Type

The first creatable I/O-centric type you will examine is the DirectoryInfo class. This class contains a set of members used for creating, moving, deleting, and enumerating over directories and subdirectories. In addition to the functionality provided by its base class (FileSystemInfo), DirectoryInfo offers the key members detailed in Table 20-3.

### Table 20-3. Key Members of the DirectoryInfo Type

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

class with the following new static method that creates a new DirectoryInfo object mapped to C:\Windows (adjust your path if need be), which displays several interesting statistics:

```
using System;
using System.IO;

Console.WriteLine("***** Fun with Directory(Info) *****
");
ShowWindowsDirectoryInfo();
Console.ReadLine();

static void ShowWindowsDirectoryInfo()
{
    // Dump directory information. If you are not on Windows, plug in another directory
    DirectoryInfo dir = new DirectoryInfo($"@"C{Path.VolumeSeparatorChar}{Path.DirectorySeparatorChar}Windows");
    Console.WriteLine("***** Directory Info *****");
    Console.WriteLine("FullName: {0}", dir.FullName);
    Console.WriteLine("Name: {0}", dir.Name);
    Console.WriteLine("Parent: {0}", dir.Parent);
    Console.WriteLine("Creation: {0}", dir.CreationTime);
    Console.WriteLine("Attributes: {0}", dir.Attributes);
    Console.WriteLine("Root: {0}", dir.Root);
    Console.WriteLine("***************************\n");
}
```

While your output might differ, you should see something like the following:

```
***** Fun with Directory(Info) *****
***** Directory Info *****
FullName: C:\Windows
Name: Windows
Parent:
Creation: 3/19/2019 00:37:22
Attributes: Directory
Root: C:\
***************************
```

## Enumerating Files with the DirectoryInfo Type

In addition to obtaining basic details of an existing directory, you can extend the current example to use some methods of the DirectoryInfo type. First, you can leverage the GetFiles() method to obtain information about all *.jpg files located in the C:\Windows\Web\Wallpaper directory.

Note If you are not on a Windows machine, retrofit this code to read files of a directory on your machine. Remember to use the Path.VolumeSeparatorChar and Path.DirectorySeparatorChar values to make your code cross-platform compatible.
