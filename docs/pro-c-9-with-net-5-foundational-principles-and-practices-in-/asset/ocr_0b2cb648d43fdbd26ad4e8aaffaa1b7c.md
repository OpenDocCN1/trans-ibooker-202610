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

# Enumerating Files with the DirectoryInfo Type

In addition to obtaining basic details of an existing directory, you can extend the current example to use some methods of the DirectoryInfo type. First, you can leverage the GetFiles() method to obtain information about all *.jpg files located in the C:\Windows\Web\Wallpaper directory.

Note If you are not on a Windows machine, retrofit this code to read files of a directory on your machine. Remember to use the Path.VolumeSeparatorChar and Path.DirectorySeparatorChar values to make your code cross-platform compatible.