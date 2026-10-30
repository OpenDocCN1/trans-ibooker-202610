# Working with StringWriters and StringReaders

You can use the StringWriter and StringReader types to treat textual information as a stream of in-memory characters. This can prove helpful when you would like to append character-based information to an underlying buffer. The following Console Application project (named StringReaderWriterApp) illustrates this by writing a block of string data to a StringWriter object, rather than to a file on the local hard drive (do not forget to import System.IO and System.Text):

```
using System;
using System.IO;
using System.Text;

Console.WriteLine("**** Fun with StringWriter / StringReader ****\n");

// Create a StringWriter and emit character data to memory.
using(StringWriter strWriter = new StringWriter())
{
    strWriter.WriteLine("Don't forget Mother's Day this year...");
    // Get a copy of the contents (stored in a string) and dump
    // to console.
    Console.WriteLine("Contents of StringWriter:\n{0}", strWriter);
}
Console.ReadLine();
```

StringWriter and StreamWriter both derive from the same base class (TextWriter), so the writing logic is similar. However, given the nature of StringWriter, you should also be aware that this class allows you to use the following GetStringBuilder() method to extract a System.Text.StringBuilder object:

```
using (StringWriter strWriter = new StringWriter())
{
    strWriter.WriteLine("Don't forget Mother's Day this year...");
    Console.WriteLine("Contents of StringWriter:\n{0}", strWriter);

    // Get the internal StringBuilder.
    StringBuilder sb = strWriter.GetStringBuilder();
    sb.Insert(0, "Hey!! ");
    Console.WriteLine("-> {0}", sb.ToString());
    sb.Remove(0, "Hey!! ".Length);
    Console.WriteLine("-> {0}", sb.ToString());
}
```

When you want to read from a stream of character data, you can use the corresponding StringReader type, which (as you would expect) functions identically to the related StreamReader class. In fact, the StringReader class does nothing more than override the inherited members to read from a block of character data, rather than from a file, as shown here:

```
using (StringWriter strWriter = new StringWriter())
{
    strWriter.WriteLine("Don't forget Mother's Day this year...");
    Console.WriteLine("Contents of StringWriter:\n{0}", strWriter);
```