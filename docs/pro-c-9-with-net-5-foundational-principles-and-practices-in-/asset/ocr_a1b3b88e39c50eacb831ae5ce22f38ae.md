```
using System;
using System.IO;
using System.Text;

Console.WriteLine("***** Fun with StreamWriter / StreamReader *****
");

// Get a StreamWriter and write string data.
using(StreamWriter writer = File.CreateText("reminders.txt"))
{
    writer.WriteLine("Don't forget Mother's Day this year...");
    writer.WriteLine("Don't forget Father's Day this year...");
    writer.WriteLine("Don't forget these numbers:");
    for(int i = 0; i < 10; i++)
    {
        writer.Write(i + " ");
    }

    // Insert a new line.
    writer.Write(writer.NewLine);
}
Console.WriteLine("Created file and wrote some thoughts...");
Console.ReadLine();
//File.Delete("reminders.txt");
```

After you run this program, you can examine the contents of this new file. You will find this file in the root directory of your project (Visual Studio Code) or under the bin\Debug\net5.0 folder (Visual Studio) because you did not specify an absolute path at the time you called CreateText() and the file location defaults to the current execution directory of the assembly.

# Reading from a Text File

Next, you will learn to read data from a file programmatically by using the corresponding StreamReader type. Recall that this class derives from the abstract TextReader, which offers the functionality described in Table 20-9.

Table 20-9. TextReader Core Members

| Member | Meaning in Life |
|---|---|
| Peek() | Returns the next available character (expressed as an integer) without changing the position of the reader. A value of -1 indicates you are at the end of the stream. |
| Read() | Reads data from an input stream. |
| ReadAsync() |  |
| ReadBlock() | Reads a specified maximum number of characters from the current stream and writes |
| ReadBlockAsync() | the data to a buffer, beginning at a specified index. |
| ReadLine() | Reads a line of characters from the current stream and returns the data as a string (a |
| ReadLineAsync() | null string indicates EOF). |
| ReadToEnd() | Reads all characters from the current position to the end of the stream and returns |
| ReadToEndAsync() | them as a single string. |