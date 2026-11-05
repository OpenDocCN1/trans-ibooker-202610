
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

2. 在 Program.cs 顶部附近，调用 WorkWithCompression 两次，一次使用默认的 Brotli，一次使用 GZIP，如以下代码所示：

```
WorkWithCompression();
WorkWithCompression(useBrotli: false);
```

3. 运行代码并比较两个压缩 XML 文件的大小。Brotli 的密度比高出 21% 以上，如以下编辑后的输出所示：

```
/Users/markjprice/Code/Chapter09/WorkingWithStreams/streams.brotli contains 118 bytes.
/Users/markjprice/Code/Chapter09/WorkingWithStreams/streams.gz contains 150 bytes.
```

## 编码和解码文本

文本字符可以用不同的方式表示。例如，字母表可以使用摩尔斯电码编码为一系列点和划，以便通过电报线路传输。

同样，计算机内部的文本以位（0 和 1）的形式存储，表示代码空间中的一个代码点。大多数代码点代表单个字符，但它们也可以具有其他含义，如格式。

例如，ASCII 具有包含 128 个代码点的代码空间。.NET 使用名为 Unicode 的标准在内部编码文本。Unicode 拥有超过一百万个代码点。

有时，您需要将文本移出 .NET 以供不使用 Unicode 或使用 Unicode 变体的系统使用，因此学习如何在编码之间转换非常重要。

下表列出了计算机常用的一些替代文本编码：

| 编码 | 描述 |
|---|---|
| ASCII | 这使用字节的低七位对有限范围的字符进行编码。 |
| UTF-8 | 这将每个 Unicode 代码点表示为一到四个字节的序列。 |
| UTF-7 | 这旨在比 UTF-8 在 7 位通道上更高效，但它存在安全性和健壮性问题，因此建议使用 UTF-8 而非 UTF-7。 |
| UTF-16 | 这将每个 Unicode 代码点表示为一个或两个 16 位整数的序列。 |
| UTF-32 | 这将每个 Unicode 代码点表示为 32 位整数，因此是一种定长编码，而与其他所有 Unicode 编码（均为变长编码）不同。 |
| ANSI/ISO 编码 | 这提供了对多种代码页的支持，这些代码页用于支持特定语言或语言组。 |

**Good Practice:** 在大多数情况下，今天 UTF-8 是一个不错的默认值，这就是为什么它实际上是默认编码，即 Encoding.Default。

## 将字符串编码为字节数组

让我们探索文本编码：

- 1. 使用您喜欢的代码编辑器，在 `Chapter09` 解决方案/工作区中添加一个名为 `WorkingWithEncodings` 的新控制台应用。
- 2. 在 Visual Studio Code 中，将 `WorkingWithEncodings` 选择为活动的 OmniSharp 项目。
- 3. 在 `Program.cs` 中，导入 `System.Text` 命名空间并静态导入 `Console` 类。
- 4. 添加语句，使用用户选择的编码对字符串进行编码，遍历每个字节，然后将其解码回 `string` 并输出，如以下代码所示：

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

- 5. 运行代码并注意避免使用 Encoding.UTF7 的警告，因为它不安全。当然，如果您需要为与另一系统兼容而使用该编码生成文本，它需要在 .NET 中仍然是一个选项。
- 6. 按 1 选择 ASCII 并注意在输出字节时，英镑符号 (£) 和带重音的 e (é) 不能在 ASCII 中表示，因此它使用问号代替。

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

7. 重新运行代码并按 3 选择 UTF-8 并注意 UTF-8 需要为每个需要 2 个字节的两个字符额外使用两个字节（总共 18 个字节而不是 16 个字节），但它可以编码和解码 é 和 £ 字符。

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

8. 重新运行代码并按 4 选择 Unicode (UTF-16) 并注意 UTF-16 为每个字符需要两个字节，因此总共 32 个字节，并且它可以编码和解码 é 和 £ 字符。此编码由 .NET 在内部用于存储 char 和 string 值。

## 在文件中编码和解码文本

在使用 StreamReader 和 StreamWriter 等流助手类时，您可以指定要使用的编码。当您向助手写入时，文本将自动编码，而当您从助手读取时，字节将自动解码。

要指定编码，请将编码作为第二个参数传递给助手类型的构造函数，如以下代码所示：

```
StreamReader reader = new(stream, Encoding.UTF8);
StreamWriter writer = new(stream, Encoding.UTF8);
```

> **Good Practice:** 通常，您没有选择编码的余地，因为您将为另一系统使用而生成文件。但是，如果您有选择，请选择使用最少字节数但能存储所需每个字符的编码。

## 序列化对象图

序列化是将实时对象使用指定格式转换为字节序列的过程。反序列化是反向过程。您会这样做以保存实时对象的当前状态，以便将来可以重新创建它。例如，保存游戏的当前状态，以便明天可以继续在同一位置。序列化对象通常存储在文件或数据库中。

您可以指定的格式有几十种，但最常见的两种是可扩展标记语言 (XML) 和 JavaScript 对象表示法 (JSON)。

![](img/c63b00dca7192cc58cfb9e1f1e64f483_424_0.png)

> **Good Practice:** JSON 更紧凑，最适合 Web 和移动应用。XML 更冗长，但在更多旧系统中的支持更好。使用 JSON 以最小化序列化对象图的大小。JSON 也是向 Web 应用和移动应用发送对象图时不错的选择，因为 JSON 是 JavaScript 的原生序列化格式，而且移动应用通常通过有限带宽进行调用，因此字节数很重要。

.NET 有多个类可将数据序列化为 XML 和 JSON 以及反序列化。我们将从 XmlSerializer 和 JsonSerializer 开始。

## 作为 XML 序列化

让我们从 XML 开始，可能是目前世界上使用最广泛的序列化格式（到目前为止）。为了展示一个典型的示例，我们将定义一个自定义类来存储有关人员的信息，然后使用包含嵌套的 Person 实例列表创建对象图：

- 1. 使用您喜欢的代码编辑器，在 Chapter09 解决方案/工作区中添加一个名为 WorkingWithSerialization 的新控制台应用。
- 2. 在 Visual Studio Code 中，将 WorkingWithSerialization 选择为活动的 OmniSharp 项目。
- 3. 添加一个名为 Person 的类，其中 Salary 属性受保护，这意味着它只能由其自身和派生类访问。为了填充薪水，该类有一个带单个参数的构造函数来设置初始薪水，如以下代码所示：

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
