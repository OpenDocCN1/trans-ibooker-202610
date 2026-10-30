# The FileInfo.Create() Method

The next set of examples are all in a Console Application named SimpleFileIO. One way you can create a file handle is to use the FileInfo.Create() method, like so:

```
using System;
using System.IO;

Console.WriteLine("***** Simple IO with the File Type ****\n");
//Change to a folder on your machine that you have read/write access to, or run as administrator
var fileName = $@"C{Path.VolumeSeparatorChar}{Path.DirectorySeparatorChar}temp{Path.DirectorySeparatorChar}Test.dat";
// Make a new file on the C drive.
FileInfo f = new FileInfo(fileName);
FileStream fs = f.Create();

// Use the FileStream object...

// Close down file stream.
fs.Close();
```

> Note These examples might require running Visual Studio as an Administrator, depending on your user permissions and system configuration.

Notice that the FileInfo.Create() method returns a FileStream object, which exposes synchronous and asynchronous write/read operations to/from the underlying file (more details in a moment). Be aware that the FileStream object returned by FileInfo.Create() grants full read/write access to all users.

Also notice that after you finish with the current FileStream object, you must ensure you close the handle to release the underlying unmanaged stream resources. Given that FileStream implements IDisposable, you can use the C# using scope to allow the compiler to generate the teardown logic (see Chapter 8 for details), like so:

```
var fileName = $@"C{Path.VolumeSeparatorChar}{Path.DirectorySeparatorChar}Test.dat";
...
//wrap the file stream in a using statement
// Defining a using scope for file I/O
FileInfo f1 = new FileInfo(fileName);
using (FileStream fs1 = f1.Create())
{
    // Use the FileStream object...
}
f1.Delete();
```

> Note Almost all of these examples in this chapter include using statements. I could have used the new using declaration syntax but chose not to in this rewrite to keep the examples focused on the System.IO components that we are examining.