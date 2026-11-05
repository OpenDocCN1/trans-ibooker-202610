
You could check a file or directory's attributes, as shown in the following code:

```
FileInfo info = new(backupFile);
WriteLine("Is the backup file compressed? {0}",
    info.Attributes.HasFlag(FileAttributes.Compressed));
```

## Reading and writing with streams

A stream is a sequence of bytes that can be read from and written to. Although files can be processed rather like arrays, with random access provided by knowing the position of a byte within the file, it can be useful to process files as a stream in which the bytes can be accessed in sequential order.

Streams can also be used to process terminal input and output and networking resources such as sockets and ports that do not provide random access and cannot seek (that is, move) to a position. You can write code to process some arbitrary bytes without knowing or caring where it comes from. Your code simply reads or writes to a stream, and another piece of code handles where the bytes are actually stored.

## Understanding abstract and concrete streams

There is an abstract class named Stream that represents any type of stream. Remember that an abstract class cannot be instantiated using new; they can only be inherited.

There are many concrete classes that inherit from this base class, including FileStream, MemoryStream, BufferedStream, GZipStream, and SslStream, so they all work the same way. All streams implement IDisposable, so they have a Dispose method to release unmanaged resources.

Some of the common members of the Stream class are described in the following table:

| Member | Description |
|---|---|
| CanRead, CanWrite | These properties determine if you can read from and write to the stream. |
| Length, Position | These properties determine the total number of bytes and the current position within the stream. These properties may throw an exception for some types of streams. |
| Dispose | This method closes the stream and releases its resources. |
| Flush | If the stream has a buffer, then this method writes the bytes in the buffer to the stream and the buffer is cleared. |
| CanSeek | This property determines if the Seek method can be used. |
| Seek | This method moves the current position to the one specified in its parameter. |
| Read, ReadAsync | These methods read a specified number of bytes from the stream into a byte array and advance the position. |
| ReadByte | This method reads the next byte from the stream and advances the position. |
| Write, WriteAsync | These methods write the contents of a byte array into the stream. |
| WriteByte | This method writes a byte to the stream. |

## Understanding storage streams

Some storage streams that represent a location where the bytes will be stored are described in the following table:

| Namespace | Class | Description |
|---|---|
| System.IO | FileStream | Bytes stored in the filesystem. |
| System.IO | MemoryStream | Bytes stored in memory in the current process. |
| System.Net.Sockets | NetworkStream | Bytes stored at a network location. |

FileStream has been re-written in .NET 6 to have much higher performance and reliability on Windows.

## Understanding function streams

Some function streams that cannot exist on their own, but can only be "plugged onto" other streams to add functionality, are described in the following table:

| Namespace | Class | Description |
|---|---|
| System.Security.Cryptography | CryptoStream | This encrypts and decrypts the stream. |
| System.IO.Compression | GZipStream, DeflateStream | These compress and decompress the stream. |
| System.Net.Security | AuthenticatedStream | This sends credentials across the stream. |

## Understanding stream helpers

Although there will be occasions where you need to work with streams at a low level, most often, you can plug helper classes into the chain to make things easier. All the helper types for streams implement IDisposable, so they have a Dispose method to release unmanaged resources.

Some helper classes to handle common scenarios are described in the following table:

| Namespace | Class | Description |
|---|---|
| System.IO | StreamReader | This reads from the underlying stream as plain text. |
| System.IO | StreamWriter | This writes to the underlying stream as plain text. |
| System.IO | BinaryReader | This reads from streams as .NET types. For example, the ReadDecimal method reads the next 16 bytes from the underlying stream as a decimal value and the ReadInt32 method reads the next 4 bytes as an int value. |
| System.IO | BinaryWriter | This writes to streams as .NET types. For example, the Write method with a decimal parameter writes 16 bytes to the underlying stream and the Write method with an int parameter writes 4 bytes. |

| System.Xml | XmlReader | This reads from the underlying stream using XML format. |
| System.Xml | XmlWriter | This writes to the underlying stream using XML format. |

## Writing to text streams

Let's type some code to write text to a stream:

-   1. Use your preferred code editor to add a new console app named `WorkingWithStreams` to the `Chapter09` solution/workspace:
-      1. In Visual Studio, set the startup project for the solution to the current selection.
-      2. In Visual Studio Code, select `WorkingWithStreams` as the active OmniSharp project.
-   2. In the `WorkingWithStreams` project, in `Program.cs`, import the `System.Xml` namespace and statically import the `System.Console`, `System.Environment`, and `System.IO.Path` types.
-   3. At the bottom of `Program.cs`, define a static class named `Viper` with a static array of string values named `Callsigns`, as shown in the following code:
-   4. Above the `Viper` class, define a `WorkWithText` method that enumerates the Viper call signs, writing each one on its own line in a single text file, as shown in the following code:

```csharp
static class Viper
{
    // define an array of Viper pilot call signs
    public static string[] Callsigns = new[]
    {
        "Husker", "Starbuck", "Apollo", "Boomer",
        "Bulldog", "Athena", "Helo", "Racetrack"
    };
}
```

```csharp
static void WorkWithText()
{
    // define a file to write to
    string textFile = Combine(CurrentDirectory, "streams.txt");

    // create a text file and return a helper writer
    StreamWriter text = File.CreateText(textFile);

    // enumerate the strings, writing each one
    // to the stream on a separate line
    foreach (string item in Viper.Callsigns)
    {
        text.WriteLine(item);
    }
}
```

```
}
text.Close(); // release resources

// output the contents of the file
WriteLine("{0} contains {1:N0} bytes.",
    arg0: textFile,
    arg1: new FileInfo(textFile).Length);

WriteLine(File.ReadAllText(textFile));
}
```

-   5. Below the namespace imports, call the WorkWithText method.
-   6. Run the code and view the result, as shown in the following output:
-   7. Open the file that was created and check that it contains the list of call signs.

```
/Users/markjprice/Code/Chapter09/WorkingWithStreams/streams.txt contains
60 bytes.
Husker
Starbuck
Apollo
Boomer
Bulldog
Athena
Helo
Racetrack
```

## Writing to XML streams

There are two ways to write an XML element, as follows:

-   - WriteStartElement and WriteEndElement: Use this pair when an element might have child elements.
-   - WriteElementString: Use this when an element does not have children.

Now, let's try storing the Viper pilot call signs array of string values in an XML file:

-   1. Create a WorkWithXml method that enumerates the call signs, writing each one as an element in a single XML file, as shown in the following code:

```csharp
static void WorkWithXml()
{
    // define a file to write to
    string xmlFile = Combine(CurrentDirectory, "streams.xml");

    // create a file stream
    FileStream xmlFileStream = File.Create(xmlFile);
```

```
// wrap the file stream in an XML writer helper
// and automatically indent nested elements
XmlWriter xml = XmlWriter.Create(xmlFileStream,
    new XmlWriterSettings { Indent = true });

// write the XML declaration
xml.WriteStartDocument();

// write a root element
xml.WriteStartElement("callsigns");

// enumerate the strings writing each one to the stream
foreach (string item in Viper.Callsigns)
{
    xml.WriteElementString("callsign", item);
}

// write the close root element
xml.WriteEndElement();

// close helper and stream
xml.Close();
xmlFileStream.Close();

// output all the contents of the file
WriteLine("{0} contains {1:N0} bytes.",
    arg0: xmlFile,
    arg1: new FileInfo(xmlFile).Length);

WriteLine(File.ReadAllText(xmlFile));
}
```

-   2. In Program.cs, comment out the previous method call, and add a call to the WorkWithXml method.
-   3. Run the code and view the result, as shown in the following output:

/Users/markjprice/Code/Chapter09/WorkingWithStreams/streams.xml contains
310 bytes.
<?xml version="1.0" encoding="utf-8"?>
<callsigns>
    <callsign>Husker</callsign>
    <callsign>Starbuck</callsign>
    <callsign>Apollo</callsign>
    <callsign>Boomer</callsign>
    <callsign>Bulldog</callsign>
