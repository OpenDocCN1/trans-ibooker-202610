```
// because when the XmlWriter disposes, it will
// automatically end any elements of any depth
}
} // also closes the underlying stream

// output all the contents of the compressed file
WriteLine("{0} contains {1:N0} bytes.",
    filePath, new FileInfo(filePath).Length);

WriteLine($"The compressed contents:");
WriteLine(File.ReadAllText(filePath));

// read a compressed file
WriteLine("Reading the compressed XML file:");
file = File.Open(filePath, FileMode.Open);

Stream decompressor = new GZipStream(file,
    CompressionMode.Decompress);

using (decompressor)
{
    using (XmlReader reader = XmlReader.Create(decompressor))
    {
        while (reader.Read()) // read the next XML node
        {
            // check if we are on an element node named callsign
            if ((reader.NodeType == XmlNodeType.Element)
                && (reader.Name == "callsign"))
            {
                reader.Read(); // move to the text inside element
                WriteLine($"({reader.Value})"); // read its value
            }
        }
    }
}
```

3. In Program.cs, leave the call to WorkWithXml, and add a call to WorkWithCompression, as shown highlighted in the following code:

```
// WorkWithText();
WorkWithXml();
WorkWithCompression();
```