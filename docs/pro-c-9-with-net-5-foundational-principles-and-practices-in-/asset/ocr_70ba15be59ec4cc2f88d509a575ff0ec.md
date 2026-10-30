# Working with the File Type

The File type uses several static members to provide functionality almost identical to that of the FileInfo type. Like FileInfo, File supplies AppendText(), Create(), CreateText(), Open(), OpenRead(), OpenWrite(), and OpenText() methods. In many cases, you can use the File and FileInfo types interchangeably. Note that OpenText() and OpenRead() require the file to already exist. To see this in action, you can simplify each of the previous FileStream examples by using the File type instead, like so:

```
var fileName = $@"C{Path.VolumeSeparatorChar}{Path.DirectorySeparatorChar}Test.dat";
...
//Using File instead of FileInfo
using (FileStream fs8 = File.Create(fileName))
{
    // Use the FileStream object...
}
File.Delete(fileName);
// Make a new file via FileInfo.Open().
using(FileStream fs9 = File.Open(fileName,
    FileMode.OpenOrCreate, FileAccess.ReadWrite,
    FileShare.None))
{
    // Use the FileStream object...
}
// Get a FileStream object with read-only permissions.
using(FileStream readOnlyStream = File.OpenRead(fileName))
{}
File.Delete(fileName);
// Get a FileStream object with write-only permissions.
using(FileStream writeOnlyStream = File.OpenWrite(fileName))
{}
// Get a StreamReader object.
using(StreamReader sreader = File.OpenText(fileName))
{}
File.Delete(fileName);
// Get some StreamWriters.
using(StreamWriter swriter = File.CreateText(fileName))
{}
File.Delete(fileName);

using(StreamWriter swriterAppend =
    File.AppendText(fileName))
{}
File.Delete(fileName);
```

# Additional File-centric Members

The File type also supports a few members, shown in Table 20-6, which can greatly simplify the processes of reading and writing textual data.