
```
{
    xml.WriteElementString("callsign", item);
}
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

Stream decompressor;
if (useBrotli)
{
    decompressor = new BrotliStream(
        file, CompressionMode.Decompress);
}
else
{
    decompressor = new GZipStream(
        file, CompressionMode.Decompress);
}

using (decompressor)
{
    using (XmlReader reader = XmlReader.Create(decompressor))
    {
        while (reader.Read())
        {
            // check if we are on an element node named callsign
            if ((reader.NodeType == XmlNodeType.Element)
                && (reader.Name == "callsign"))
            {
                reader.Read(); // move to the text inside element
                WriteLine($"{reader.Value}"); // read its value
            }
        }
    }
}
```

2. Near the top of Program.cs, call WorkWithCompression twice, once with the default using Brotli and once with GZIP, as shown in the following code:

```
WorkWithCompression();
WorkWithCompression(useBrotli: false);
```

3. Run the code and compare the sizes of the two compressed XML files. Brotli is more than 21% denser, as shown in the following edited output:

```
/Users/markjprice/Code/Chapter09/WorkingWithStreams/streams.brotli contains 118 bytes.
/Users/markjprice/Code/Chapter09/WorkingWithStreams/streams.gz contains 150 bytes.
```

## Encoding and decoding text

Text characters can be represented in different ways. For example, the alphabet can be encoded using Morse code into a series of dots and dashes for transmission over a telegraph line.

In a similar way, text inside a computer is stored as bits (ones and zeros) representing a code point within a code space. Most code points represent a single character, but they can also have other meanings like formatting.

For example, ASCII has a code space with 128 code points. .NET uses a standard called Unicode to encode text internally. Unicode has more than one million code points.

Sometimes, you will need to move text outside .NET for use by systems that do not use Unicode or use a variation of Unicode, so it is important to learn how to convert between encodings.

The following table lists some alternative text encodings commonly used by computers:

| Encoding | Description |
|---|---|
| ASCII | This encodes a limited range of characters using the lower seven bits of a byte. |
| UTF-8 | This represents each Unicode code point as a sequence of one to four bytes. |
| UTF-7 | This is designed to be more efficient over 7-bit channels than UTF-8 but it has security and robustness issues, so UTF-8 is recommended over UTF-7. |
| UTF-16 | This represents each Unicode code point as a sequence of one or two 16-bit integers. |
| UTF-32 | This represents each Unicode code point as a 32-bit integer and is therefore a fixed-length encoding unlike the other Unicode encodings, which are all variable-length encodings. |
| ANSI/ISO encodings | This provides support for a variety of code pages that are used to support a specific language or group of languages. |

**Good Practice:** In most cases today, UTF-8 is a good default, which is why it is literally the default encoding, that is, Encoding.Default.

## Encoding strings as byte arrays

Let's explore text encodings:

- 1. Use your preferred code editor to add a new console app named `WorkingWithEncodings` to the `Chapter09` solution/workspace.
- 2. In Visual Studio Code, select `WorkingWithEncodings` as the active OmniSharp project.
- 3. In `Program.cs`, import the `System.Text` namespace and statically import the `Console` class.
- 4. Add statements to encode a string using an encoding chosen by the user, loop through each byte, and then decode it back into a `string` and output it, as shown in the following code:

```
WriteLine("Encodings");
WriteLine("[1] ASCII");
WriteLine("[2] UTF-7");
WriteLine("[3] UTF-8");
WriteLine("[4] UTF-16 (Unicode)");
WriteLine("[5] UTF-32");
WriteLine("[any other key] Default");

// choose an encoding
Write("Press a number to choose an encoding: ");
ConsoleKey number = ReadKey(intercept: false).Key;
WriteLine();
WriteLine();

Encoding encoder = number switch
{
    ConsoleKey.D1 => Encoding.ASCII,
    ConsoleKey.D2 => Encoding.UTF7,
    ConsoleKey.D3 => Encoding.UTF8,
    ConsoleKey.D4 => Encoding.Unicode,
    ConsoleKey.D5 => Encoding.UTF32,
    _ => Encoding.Default
};

// define a string to encode
string message = "Café cost: £4.39";
```

```
// encode the string into a byte array
byte[] encoded = encoder.GetBytes(message);

// check how many bytes the encoding needed
WriteLine("{0} uses {1:N0} bytes.",
    encoder.GetType().Name, encoded.Length);
WriteLine();

// enumerate each byte
WriteLine("$BYTE HEX CHAR");
foreach (byte b in encoded)
{
    WriteLine($"{b,4} {b.ToString("X"),4} {(char)b,5}");
}

// decode the byte array back into a string and display it
string decoded = encoder.GetString(encoded);
WriteLine(decoded);
```

- 5. Run the code and note the warning to avoid using Encoding.UTF7 because it is insecure. Of course, if you need to generate text using that encoding for compatibility with another system, it needs to remain an option in .NET.
- 6. Press 1 to choose ASCII and note that when outputting the bytes, the pound sign (£) and accented e (é) cannot be represented in ASCII, so it uses a question mark instead.

```
BYTE  HEX  CHAR
   67   43   C
   97   61   a
  102   66   f
   63   3F   ?
   32   20
  111   6F   o
  115   73   s
  116   74   t
   58   3A   :
   32   20
   63   3F   ?
   52   34   4
   46   2E   .
   51   33   3
   57   39   9
Caf? cost: ?4.39
```

7. Rerun the code and press 3 to choose UTF-8 and note that UTF-8 requires two extra bytes for the two characters that need 2 bytes each (18 bytes instead of 16 bytes total) but it can encode and decode the é and £ characters.

```
UTF8EncodingSealed uses 18 bytes.

BYTE HEX CHAR
  67  43  C
  97  61  a
 102  66  f
 195  C3  Ã
 169  A9  ©
  32  20
 111  6F  o
 115  73  s
 116  74  t
  58  3A  :
  32  20
 194  C2  Â
 163  A3  £
  52  34  4
  46  2E  .
  51  33  3
  57  39  9
Café cost: £4.39
```

8. Rerun the code and press 4 to choose Unicode (UTF-16) and note that UTF-16 requires two bytes for every character, so 32 bytes in total, and it can encode and decode the é and £ characters. This encoding is used internally by .NET to store char and string values.

## Encoding and decoding text in files

When using stream helper classes, such as StreamReader and StreamWriter, you can specify the encoding you want to use. As you write to the helper, the text will automatically be encoded, and as you read from the helper, the bytes will be automatically decoded.

To specify an encoding, pass the encoding as a second parameter to the helper type's constructor, as shown in the following code:

```
StreamReader reader = new(stream, Encoding.UTF8);
StreamWriter writer = new(stream, Encoding.UTF8);
```

> **Good Practice:** Often, you won't have the choice of which encoding to use, because you will be generating a file for use by another system. However, if you do, pick one that uses the least number of bytes, but can store every character you need.

## Serializing object graphs

Serialization is the process of converting a live object into a sequence of bytes using a specified format. Deserialization is the reverse process. You would do this to save the current state of a live object so that you can recreate it in the future. For example, saving the current state of a game so that you can continue at the same place tomorrow. Serialized objects are usually stored in a file or database.

There are dozens of formats you can specify, but the two most common ones are eXtensible Markup Language (XML) and JavaScript Object Notation (JSON).

![](img/c63b00dca7192cc58cfb9e1f1e64f483_424_0.png)

> **Good Practice:** JSON is more compact and is best for web and mobile applications. XML is more verbose but is better supported in more legacy systems. Use JSON to minimize the size of serialized object graphs. JSON is also a good choice when sending object graphs to web applications and mobile applications because JSON is the native serialization format for JavaScript and mobile apps often make calls over limited bandwidth, so the number of bytes is important.

.NET has multiple classes that will serialize to and from XML and JSON. We will start by looking at XmlSerializer and JsonSerializer.

## Serializing as XML

Let's start by looking at XML, probably the world's most used serialization format (for now). To show a typical example, we will define a custom class to store information about a person and then create an object graph using a list of Person instances with nesting:

- 1. Use your preferred code editor to add a new console app named WorkingWithSerialization to the Chapter09 solution/workspace.
- 2. In Visual Studio Code, select WorkingWithSerialization as the active OmniSharp project.
- 3. Add a class named Person with a Salary property that is protected, meaning it is only accessible to itself and derived classes. To populate the salary, the class has a constructor with a single parameter to set the initial salary, as shown in the following code:

```
namespace Packt.Shared;

public class Person
{
    public Person(decimal initialSalary)
    {
        Salary = initialSalary;
    }
}
```
