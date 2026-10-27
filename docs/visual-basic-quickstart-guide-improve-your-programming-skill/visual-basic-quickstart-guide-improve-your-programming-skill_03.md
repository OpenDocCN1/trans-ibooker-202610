## 第 2 部分：Visual Basic 文件与数据结构

在本书的第二部分，我们将专注于培养编写更高级程序的技能。你将学习如何格式化数据，并持久化到磁盘上的文件中。我们还将了解 Visual Basic 编程语言中提供给您的先进数据结构。

本部分包含以下章节：

- - *[第 8 章](#)*, *格式化与修改数据*
- - *[第 9 章](#)*, *文件输入与输出*
- - *[第 10 章](#)*, *集合*
- - *[第 11 章](#)*, *项目第二部分*

## 格式化与修改数据

格式化数据是编程语言中的常见任务，目的是以所需格式呈现数据。程序员还需要转换数据，使其保持相同格式或转换为不同格式，以便在算法中使用。

在本章中，我们将介绍以下主要主题：

- - 修改字符串数据
- - 修改数值数据
- - 修改日期和时间数据
- - 格式化数值
- - 格式化日期和时间值

## 修改字符串数据

在 VB 中，你可以使用各种内置函数和运算符来修改字符串数据。以下是你可以在字符串数据上执行的一些标准操作：

## 连接

在 VB 中有几种组合字符串的方法：

- - **& 运算符：** 你可以使用 **&** 运算符拼接字符串。示例如下：

```
Dim firstName As String = "Sally"
Dim lastName As String = "Stone"
Dim fullName As String = firstName & " " & lastName
```

上述代码将从三个独立的字符串（**Sally**、一个空格和姓氏 **Stone**）组成一个字符串。连接运算符适用于所有 VB 家族成员。

- - **String.Concat 方法：** 另一种拼接字符串的方法是使用 **String.Concat** 方法。以下是使用 **String.Concat** 方法的相同示例：

```
Dim firstName As String = "Sally"
Dim lastName As String = "Stone"
Dim fullName As String = String.Concat(firstName, " ", lastName)
```

上述代码与连接运算符示例具有相同的效果。它将从三个独立的字符串（**Sally**、一个空格和姓氏 **Stone**）组成一个字符串。**String.Concat** 方法仅在 VB.NET 家族成员中可用。





## 子字符串提取

在VB中，有几种方法可以从一个字符串中提取子字符串：

- Mid函数：使用Mid函数，你可以从字符串中提取子字符串，例如：

  ```
  Dim myStr As String = "Hello World!"
  Dim substr As String = Mid(myStr, 6, 5)
  ```

该示例提取从索引6开始、长度为5的子字符串，以取出单词World并将其存储到另一个变量中。最后一个参数可以省略。

- Substring方法：使用Substring方法是VB.NET新增的，你可以通过它从字符串中提取子字符串，例如：

  ```
  Dim myStr As String = "Hello World!"
  Dim substr As String = myStr.Substring(6, 5)
  ```

上述示例提取从索引6开始、长度为5的子字符串，以取出单词World并将其存储到另一个变量中。使用VB.NET时，你仍然可以利用Mid函数完成相同操作。

## 字符串替换

使用Replace方法，你可以替换字符串中特定出现的子字符串。以下是一个VB.NET示例：

```
Dim myStr As String = "Hello World!"
Dim repStr As String = myStr.Replace("World", "Sally")
```

上面的代码将单词"World"替换为"Sally"。repStr变量现在将保存字符串"Hello Sally!"。

在较早的VB家族成员中，Replace函数将要操作的字符串作为第一个参数。以下是一个VBA示例：

```
Dim myStr As String = "Hello World!"
Dim repStr As String = Replace(myStr,"World", "Sally")
```

这段代码的工作方式与VB.NET版本相同，将单词"World"替换为"Sally"。repStr变量现在将保存"Hello Sally!"字符串。

## 大小写转换

使用ToUpper方法，你可以将字符串转换为大写。例如，在VB.NET中，你可以这样做：

```
Dim myStr As String = "hello world!"
Dim uppStr As String = MyStr.ToUpper()
```

这段代码将字符串转换为大写。uppStr变量将保存"HELLO WORLD!"值。

在较早的VB家族成员中，有一个类似的函数叫UCase。UCase函数将要操作的字符串作为参数。以下是一个VBA示例：

```
Dim myStr As String = "hello world!"
Dim uppStr As String = Ucase(MyStr)
```

此代码的工作方式与VB.NET版本相同，将字符串转换为大写。uppStr变量将保存"HELLO WORLD!"值。

使用ToLower方法，你可以将字符串转换为小写。例如，在VB.NET中，你可以这样做：

```
Dim myStr As String = "Hello World!"
Dim lowStr As String = MyStr.ToLower()
```

上面的代码将字符串转换为小写。lowStr变量将保存"hello world!"值。

在较早的VB家族成员中，有一个类似的函数叫LCase。LCase函数将要操作的字符串作为参数。以下是一个VBA示例：

```
Dim myStr As String = "hello world!"
Dim lowStr As String = LCase(MyStr)
```

此代码的工作方式与VB.NET版本相同，将字符串转换为小写。lowStr变量将保存"hello world!"值。

接下来，我们将介绍如何在VB中修改数值数据。

## 修改数值数据

在VB中，你可以使用许多不同的运算符和函数来修改数值数据。我们将重点介绍三种数值运算：数学运算、舍入/截断和类型转换。

### 数学运算

VB支持传统的数学运算：

- **加法**：可以使用+运算符将数值相加。例如：

  ```
  Dim n1 As Integer = 4
  Dim n2 As Integer = 2
  Dim n3 As Integer = n1 + n2
  ```

上面的代码将声明两个整数变量（n1和n2），并将它们相加，把结果存储到名为n3的第三个变量中。

- **减法**：可以使用-运算符对数值进行相减。例如：

  ```
  Dim n1 As Integer = 4
  Dim n2 As Integer = 2
  Dim n3 As Integer = n1 - n2
  ```

上面的代码将声明两个整数变量（n1和n2），然后从n1中减去n2，将结果存储到名为n3的第三个变量中。

- **乘法**：可以使用*运算符将数值相乘。例如：

  ```
  Dim n1 As Integer = 4
  Dim n2 As Integer = 2
  Dim n3 As Integer = n1 * n2
  ```

这段代码将声明两个整数变量（n1和n2），然后将它们相乘，把结果存储到名为n3的第三个变量中。

- **除法**：可以使用/运算符对数值进行除法。结果是实数。例如：

  ```
  Dim n1 As Integer = 5
  Dim n2 As Integer = 2
  Dim n3 As Double = n1 / n2
  ```

上面的代码将声明两个整数变量（n1和n2），然后对它们进行除法，把结果存储到类型为Double的第三个变量n3中。

- **模除法**：在计算机科学中，我们经常需要除法运算的余数。这种运算称为模除法。可以使用Mod运算符执行模除法。例如：

  ```
  Dim n1 As Integer = 5
  Dim n2 As Integer = 2
  Dim n3 As Integer = n1 Mod n2
  ```

上面的代码将声明两个整数变量（n1和n2），然后对它们进行除法，把余数存储到名为n3的第三个变量中。

### 舍入和截断

有几种函数可以让你获得精度较低的十进制值：

- **舍入**：使用Round函数，你可以将数值舍入到最接近的整数。在VB.NET中，你需要调用Math命名空间中的Round方法，例如：

  ```
  Dim pi As Double = 3.14159
  Dim roundedPi As Double = Math.Round(pi)
  ```

上面的代码将值3存储到roundedPi变量中。

在较早的VB家族成员中，无需使用**Math**命名空间也能完成相同的操作：

```
Dim pi As Double = 3.14159
Dim roundedPi As Double = Round(pi)
```

在VB.NET和较早的VB家族成员中，你还可以传递第二个参数——要舍入的小数位数，例如：

```
Dim pi As Double = 3.14159
Dim roundedPi As Double = Math.Round(pi,4)
```

这段代码将舍入到四位小数，把值**3.1416**存储到**roundedPi**变量中。

- **截断**：你可以使用**truncate**函数截断数值。截断会移除实数中剩余的数字。在VB.NET中，你需要调用**Math**命名空间中的**truncate**方法，例如：

  ```
  Dim pi As Double = 3.14159
  Dim trunPi As Double = Math.Truncate(pi)
  ```

上面的代码将值**3**存储到**trunPi**变量中。

在VBA中，可以使用**Fix**函数实现相同的操作，无需**Math**命名空间：

```
Dim pi As Double
Pi = 3.14159
Dim trunPi As Double
trunPi = Fix(pi)
```

VBScript和VB6没有**truncate**函数。如果你想去掉小数部分，还有其他函数，例如**Fix**和**Int**。要在使用这些函数的同时保留小数点右边的数字，有一个技巧：将数字乘以10的倍数，再将结果除以同一个数。下面是一个**VBScript**示例：

```
Dim pi
pi = 3.14159
Dim trunPi
trunPi = Int(pi * 10000) / 10000
```

上面的代码将截断到四位小数，把值**3.1415**存储到**trunPi**变量中。

### 类型转换

使用强制转换运算符，你可以将数值从一种数据类型转换为另一种数据类型。例如：

```
Dim n1 As Integer = 25
Dim n2 As Double = CDbl(n1)
```

**DateAdd**函数的第一个参数是一个VB常量，用于表示要添加到第三个参数中的值的类型。以下是可能的类型及其使用的常量：

- **yyyy**：年
- **q**：季度
- **m**：月
- **y**：一年中的第几天
- **d**：日
- **w**：星期几
- **ww**：周
- **h**：小时
- **n**：分钟
- **s**：秒

## 日期和时间组件提取

使用相应的属性或函数，你可以从**Date**或**Time**值中提取特定组件（年、月、日、小时、分钟或秒）。在VB.NET中，**DateTime**值上有一个属性可用。在VB6、VBScript和VBA中，有一个同名函数。以下是一个VB.NET示例：

```
Dim curDate As Date = Date.Now
Dim curYear As Integer = curDate.Year
Dim curMonth As Integer = curDate.Month
Dim curDay As Integer = curDate.Day
Dim curHour As Integer = curDate.Hour
Dim curMinute As Integer = curDate.Minute
Dim curSecond As Integer = curDate.Second
```

在较早的VB家族成员中，同样的示例会是这样：

```
Dim curDate As Date = Now()
Dim curYear As Integer = Year(curDate)
Dim curMonth As Integer = Month(curDate)
Dim curDay As Integer = Day(curDate)
Dim curHour As Integer = Hour(curDate)
Dim curMinute As Integer = Minute(curDate)
Dim curSecond As Integer = Second(curDate)
```

接下来，我们将介绍格式化数值。

## 格式化数值

在VB中，你可以使用各种函数和方法来格式化数据，以所需的形式呈现出来。以下是一些在Visual Basic中常用的格式化数据技巧：




在VB.NET中，你可以使用带格式说明符的`ToString`方法来格式化数值，例如：

```
Dim pi as double = 3.14159
Dim strOut As String = pi.ToString("0.00")
```

上述VB.NET代码会将值3.14存储在`strOut`变量中。

你还可以在VB.NET中使用字符串插值在字符串内格式化数值，例如：

```
Dim pi = 3.14159
Dim strOut As String = $"{pi:0.00}"
```

在早期VB家族成员（VBScript、VBA和VB6）中，你可以使用`FormatNumber`函数按所需格式来格式化数字。下面是一个在VBScript中实现该操作的示例：

```
Dim pi
pi = 3.14159
Dim formattedPi
formattedPi = FormatNumber(pi, 2)
WScript.Echo formattedPi
```

上述代码将输出`formattedPi`变量的值3.14。在此示例中，`FormatNumber`函数接受两个参数：要格式化的数字和要显示的小数位数。最后，它返回一个表示格式化后数字的字符串。

`FormatNumber`函数还有一些可选参数，可用于进一步自定义格式化。例如，你可以指定是否包含千位分隔符或小数点分隔符，以及负数的格式。

接下来，我们将介绍如何格式化日期和时间值。

## 格式化日期和时间值

你可以在VB.NET中使用带有自定义格式说明符的`ToString`方法来格式化日期和时间值，例如：

```
Dim curDate As Date = Date.Now
Dim fDate As String = curDate.ToString("MM/dd/yyyy")
Dim fTime As String = curDate.ToString("hh:mm:ss tt")
```

如果上述代码在2023年6月5日上午11:30运行，则会将日期格式化为“06/05/2023”，将时间格式化为“11:30:00 AM”。

在早期VB家族成员（VBScript、VBA和VB6）中，你可以使用`FormatDateTime`函数根据所需格式来格式化日期和时间值。下面是一个在VBScript中实现该操作的示例：

```
Dim curDate
curDate = Date()
Dim fDate
fDate = FormatDateTime(curDate, vbShortDate)
WScript.Echo fDate
```

`FormatDateTime`函数根据提供的自定义格式来格式化日期和时间。

你可以将各种预定义常量和自定义格式字符串与`FormatDateTime`函数一起使用，以实现所需的日期和时间格式。预定义常量包括`vbGeneralDate`、`vbLongDate`、`vbShortDate`、`vbLongTime`和`vbShortTime`。自定义格式字符串可以包含多个占位符，例如`yyyy`表示年份，`MM`表示月份，`dd`表示日，`hh`表示小时，`mm`表示分钟，`ss`表示秒，`AM/PM`表示时间指示符。

## 总结

本章介绍了函数和过程，以便将代码模块化以复用。在格式化数据时，语法根据VB家族成员的不同而略有变化。在下一章中，我们将讨论如何将数据持久化到文件系统，并将这些数据读回到程序中。

## 文件输入与输出

存储在磁盘驱动器上的文件允许我们的程序在多次执行之间持久化数据，使我们能够开发出强大的工具来帮助完成日常工作。此外，我们可以将文件既视为文本数据又视为二进制数据。我们将探讨这两种方法的优势。我们还将考虑如何在目录中组织程序中使用的文件。

在本章中，我们将介绍以下主要主题：

- 使用目录和文件
- 将文本写入文件
- 将二进制数据写入文件
- 从文件读取文本
- 从文件读取二进制数据

## 使用目录和文件

我们将会针对**VB.NET**和早期VB家族成员使用两种不同的方法。在本章中我们会反复看到这种模式，因为微软在.NET语言中新增了文件处理特性，你可以在VB.NET和ASP.NET中利用这些特性。

## 在VB.NET中处理文件

在VB.NET中，你可以使用**System.IO**命名空间中的**File**类来处理文件。**File**类提供了用于执行各种文件相关操作的静态方法。

若要创建文件，可以使用**Create**方法，该方法返回一个**FileStream**对象，稍后可用该对象向文件写入数据：

```
Imports System.IO
File.Create("C:\Path\File.txt")
```

若要删除文件，可以使用**Delete**方法：

```
Imports System.IO
File.Delete("C:\Path\File.txt")
```

若要检查文件是否存在，可以使用**Exists**属性，该属性返回一个布尔值，指示文件是否存在：

```
Imports System.IO
If File.Exists("C:\Path\File.txt") Then
    Console.WriteLine("The file exists")
End If
```

可以使用**Copy**方法将文件从一个位置复制到另一个位置：

```
Imports System.IO
File.Copy("C:\Path\File.txt", "C:\Path\NewFile.txt")
```

若要移动或重命名文件，可以使用**Move**方法：

```
Imports System.IO
File.Move("C:\Path\File.txt", "C:\Path\NewFile.txt")
```

若要检索文件属性，可以使用**GetAttributes**方法和**FileAttributes**类：

```
Imports System.IO
Dim attr As FileAttributes = File.GetAttributes("C:\Path\File.txt")
If attr.HasFlag(FileAttributes.ReadOnly)Then
    Console.WriteLine("The file is read-only")
End If
If attr.HasFlag(FileAttributes.Hidden) Then
    Console.WriteLine("The file is hidden")
End If
```

接下来，我们将介绍在VB.NET中处理目录。

## 在VB.NET中处理目录

在VB.NET中，你可以使用**System.IO**命名空间中的**Directory**类来处理目录。**Directory**类提供了用于执行各种目录相关操作的静态方法。

若要创建目录，可以使用**CreateDirectory**方法：

```
Imports System.IO
Directory.CreateDirectory("C:\Path\NewDirectory")
```

若要删除目录，可以使用**Delete**方法。通过第二个参数，你可以指定是否以递归方式删除目录：

```
Imports System.IO
Directory.Delete("C:\Path\Directory", recursive:=True)
```

若要检查目录是否存在，可以使用**Exists**属性，该属性返回一个布尔值，指示目录是否存在：

```
Imports System.IO
If Directory.Exists("C:\Path\Directory") Then
    Console.WriteLine("The directory exists")
End If
```

若要移动或重命名目录，可以使用**Move**方法：

```
Imports System.IO
Directory.Move("C:\Path\OldDir", "C:\Path\NewDir")
```

若要检索目录中的文件或子目录，可以使用`GetFiles`和`GetDirectories`方法：

```
Imports System.IO
Dim files As String() = Directory.GetFiles("C:\Path\Dir")
Dim dirs As String() = Directory.GetDirectories("C:\Path\Dir")
```

`GetFiles`和`GetDirectories`方法返回值集合，我们将在第10章中介绍。

若要检索有关目录的信息，可以使用`GetDirectoryInfo`方法获取`DirectoryInfo`对象。你可以像上一个示例那样使用此`DirectoryInfo`对象来获取文件和目录，但这次是从该对象获取：

```
Imports System.IO
Dim dirInfo As DirectoryInfo =
My.Computer.FileSystem.GetDirectoryInfo("C:\Path\Dir")
Dim files() As FileInfo = dirInfo.GetFiles()
```

你可以使用`GetAttributes`方法检索目录的各种属性，就像之前对文件所做的那样：

```
Imports System.IO
Dim attr As FileAttributes = File.GetAttributes("C:\Path\Dir")
If attr.HasFlag(FileAttributes.ReadOnly) Then
    Console.WriteLine("The directory is read-only")
End If
If attr.HasFlag(FileAttributes.Hidden) Then
    Console.WriteLine("The directory is hidden")
End If
```

接下来，我们将介绍在VBScript、VBA和VB6中处理文件。

## 在早期VB家族成员中处理文件

在早期VB家族成员（VBScript、VBA和VB6）中处理文件和目录，需要使用Windows脚本宿主提供的文件系统对象（FSO）。FSO允许你对文件和目录执行许多操作。

若要使用FSO创建目录，可以使用`CreateFolder`方法：

```
Set myFSO = CreateObject("Scripting.FileSystemObject")
myFSO.CreateFolder("C:\Path\Directory")
```

若要使用FSO删除目录，可以使用`DeleteFolder`方法。第二个参数指定是否以递归方式删除目录：

```
Set myFSO = CreateObject("Scripting.FileSystemObject")
myFSO.DeleteFolder("C:\Path\Directory", True)
```

若要使用FSO检查目录是否存在，可以使用`FolderExists`方法，该方法返回一个布尔值，指示该目录是否存在或


The File class also provides two methods that make writing text to a file in a single line easy. The WriteAllText and AppendAllText methods allow you to send the parameter to overwrite or append the file:

```
Imports System.IO
Dim myFile As String = "C:\Path\File.txt"
File.WriteAllText(myFile, "This text overwrites the file")
File.AppendAllText(myFile, "This is appended text.")
```

In VB.NET, the StreamWriter class can also write text to a file. First, you create a StreamWriter instance, and then use its Write or WriteLine methods to write text:

```
Imports System.IO
Dim myFile As String = "C:\Path\File.txt"
Using writer As New StreamWriter(myFile)
    writer.Write("Here is some text.")
    writer.WriteLine("This text will end the line.")
End Using
```

In earlier family members of VB, we can utilize the FSO as we did earlier. FSO provides the CreateTextFile method that we saw earlier. This method creates a new file or opens an existing file for writing. The CreateTextFile method returns a TextFile object. The Write method of the TextFile object is then used to write the text to the file. Finally, we need to make sure we close the file. The Close method of the TextFile object is used to close the file and release the resources:

```
myFile = "C:\Path\File.txt"
myText = "This text will be written to the file"
Set myFSO = CreateObject("Scripting.FileSystemObject")
Set myTextFile = myFSO.CreateTextFile(myFile, True)
myTextFile.Write myText
MyTextFile.Close
```

The implementation of the previous code snippet will depend on the VB family member. For VBScript, this could be run by placing the code in a script file. For VB and VBA, you would place the code in a larger project.

## Writing text in VB6 and VBA

VB6 and VBA have unique methods that are unavailable in VBScript for output operations. For example, you open a file for a specific mode of operation and give it a file number, which needs to be between 1 and 511. Once the file is opened, you can write to it and close it:

```
Open "C:\path\file.txt" For Output As #1
Write #1, "This is the text."
Close #1
```

Next, we will look at reading data in text format from files.

## Reading text from files

We will continue to utilize the two different methods for VB.NET and older VB family members. For VB.NET, we will use the `File` class from the `System.IO` namespace to read text from files. For VBScript, VBA, and VB6, we will continue to use the FSO we used in the files and directories section.

The `File` class also provides two methods that make writing text to a file in a single line easy. The `ReadAllText` and `ReadAllLines` methods pull all the text from the file into a single variable. The difference between the two methods is that the second method pulls the data line by line into an array, while the first does not separate the input file lines:

```
```vbnet
Imports System.IO
Dim myFile As String = "C:\Path\File.txt"
Dim myText As String = File.ReadAllText(myFile)
```
```

In VB.NET, the `StreamReader` class allows you to read text from a file. First, you create a `StreamReader` instance, and then use its `Read` or `ReadLine` methods to read the text:

```
```vbnet
Imports System.IO
Dim myFile As String = "C:\Path\File.txt"
Dim myLine As String
Using reader As New StreamReader(myFile)
    myLine = reader.ReadLine
End Using
```
```

## Reading text in VB6 and VBA

VB6 and VBA have unique methods that are unavailable in VBScript for input operations. For example, you open a file for a specific mode of operation and give it a file number, which needs to be between 1 and 511. Once the file is opened, you can read from it with the `Input` or `LineInput` statements and close it. `Input` will read to the next delimiter, and `LineInput` will read the text up to the following line:

```
```vb
Dim myValue
Open "C:\path\file.txt" For Input As #1
Input #1, myValue
MsgBox myValue
Close #1
```
```

Next, we will look at writing binary data to files.

## Writing binary data to files

We will utilize the two methods for VB.NET and older VB family members. For VB.NET, we will use the `File` class from the `System.IO` namespace to write binary data to files. For VBA and VB6, we will use the file number operations we saw in the last two sections. VBScript can use `ADODB.stream` to write binary data to files, but we will leave it out of this section due to its complexity.

before doing the put operation, or you can utilize the second parameter to skip forward.

Next, we will look at reading binary data from files.

## Reading binary data from files

We will continue to utilize the two different methods for VB.NET and older VB family members. For VB.NET, we will use the File class from the System.IO namespace to read binary data from files. For VBA and VB6, we will use the file number operations we saw in the previous sections. VBScript can use ADODB.Stream to read binary data from files, but we will leave it out of this section due to its complexity.

The File class also provides a method that makes reading binary data from a file in a single line easy. The ReadAllBytes method allows you to receive an array of bytes representing the data from the file:

```
Imports System.IO
Dim myFile As String = "C:\Path\File.dat"
Dim bData() As Byte
bData = File.ReadAllBytes(myFile)
```

To read binary data from files in VB.NET, you can also use the FileStream class from the System.IO namespace. Here’s an example of how to read binary data from a file with a FileStream object:

```
Imports System.IO
Dim myFile As String = "C:\Path\File.dat"
Dim bData()
Using fs As New FileStream(myFile, FileMode.Open, FileAccess.Read)
    ReDim bData(fs.Length - 1) As Byte
    fs.Read(bData, 0, bData.Length)
End Using
```

This example specifies the file path from which we want to read the binary data with the myFile variable. The FileStream object is created with the FileMode.Open file path to open an existing file and FileAccess.Read to specify read access to the file. Inside the Using block, the bData byte array is created with a length equal to the size of the file. The Read method of the FileStream object is then called to read the binary data from the file into the bData array.

After reading the binary data, we can process it within the Using block. The Using block ensures that the FileStream object is properly closed and disposed of after completing the reading operation.

## Reading binary data in VB6 and VBA

VB6 and VBA have unique methods that are unavailable in VBScript for binary input operations. You can open a file for a specific mode of operation and give it a file number, which needs to be between 1 and 511. Once the file is opened, you can read binary bytes with the **Get** statement:

```
```vb
Dim myByte as Byte
Open "C:\path\file.dat" For Binary Access Read As #1
Get #1,,myByte
Close #1
```
```

In the preceding example, the second parameter represents the **Byte** offset to read from. In the example, this is blank, which means **Byte** is read from the current location.

## Summary

This chapter introduced reading and writing data from files. We looked at working with both text and binary data. Storing data in files and reading that data from files is important, as we want our programs to have the ability to persist the work they have done between executions of the program.

In the next chapter, we will look at how to store many values in a single variable using the many collections available in the Visual Basic language.

## Collections

In programming, collections refer to data structures or containers that are used to store and organize groups of related values or objects. These collections provide various methods and operations that you can use efficiently to manipulate and access the data they contain.

Several common types of collections are used in computer programming, including the following:

- **Arrays**: An array is a fundamental collection type that stores a fixed-size sequence of elements of the same data type. Elements in an array are accessed by their index, which represents their position in the array.
- **Lists**: These are dynamic collections that can grow or shrink in size. They allow for efficient insertion, deletion, and retrieval of elements. Lists can store pieces of different types, often providing additional operations such as sorting and searching.
- **Sets**: These are collections that store unique elements without any particular order. They ensure that each piece appears only once, eliminating duplicates. Sets often provide operations for union, intersection, and difference between sets.
- **Dictionaries**: Also known as maps or associative arrays, dictionaries store key-value pairs. They allow you to efficiently look up values based on their associated keys. Dictionaries provide operations for inserting, deleting, and retrieving values based on their keys.
- **Queues**: These follow a **First-In, First-Out (FIFO)** order, where elements are added at the end and removed from the front. Queues are commonly used when the processing order is essential, such as scheduling tasks or processing messages.
- **Stacks**: These follow a **Last-In, First-Out (LIFO)** order, where elements are added and removed from the same end. Stacks are often used in scenarios such as parsing expressions, managing function calls, or implementing undo/redo functionality.
- **Linked lists**: These contain nodes that hold data and references to the next node in the sequence. They provide efficient insertion and deletion at any position in the list, but accessing elements via an index is less efficient than using arrays.
- **Tuples**: These are ordered collections that can store elements of different types. Unlike arrays, tuples are typically immutable, meaning their elements cannot be modified after creation.
- **Bags or multisets**: These are like sets but allow duplicate elements. They store a collection of pieces without enforcing uniqueness.

There are solutions for each collection type in VB, but we will focus on the core collections needed and used in a typical program.

In this chapter, we’re going to cover the following main topics:

- Working with one-dimensional arrays
- Utilizing multidimensional arrays





This code will output each element in the `myInts` array, but it does so without indexing into the array. The `For Each` loop will copy each value in the sequence into the variable after the `For Each` keywords. In this case, the `num` variable will get each value from the `myInts` array.

Next, we will look at working with multidimensional arrays in **VB**.

## Utilizing multidimensional arrays

Multidimensional arrays allow you to store and organize data in a spreadsheet-like structure with multiple dimensions. You can have arrays with two or more dimensions, such as two-dimensional arrays, three-dimensional arrays, and so on. Here’s an example of how you can declare an array with multiple dimensions:

```
Dim arrayName(size1, size2, ...) As DataType
```

For example, to declare and initialize a two-dimensional array of integers with five rows and 10 columns, you can use the following code:

```
Dim myMatrix(5, 10) As Integer
```

Array elements in multidimensional arrays are accessed using their indices for each dimension. You can retrieve or assign values to array elements as follows:

```
Dim value As Integer = myMatrix(4, 9)
myMatrix(4, 9) = 25
```

The previous code example accesses the element at the fourth row and tenth column. First, it retrieves the value, then assigns a new value to that cell of the multidimensional array.

To get the length or size of each dimension in a multidimensional array, you can use the `GetLength` method while passing in the dimension you want to know the size of:

```
Dim rows As Integer = myMatrix.GetLength(0)
Dim columns As Integer = myMatrix.GetLength(1)
```

You can iterate over the elements of an array using various looping constructs inside other looping constructs, such as a `For` loop, as we learned in *Chapter 5*:

```
For row As Integer = 0 To myMatrix.GetLength(0) - 1
    For col As Integer = 0 To myMatrix.GetLength(1) - 1
        Console.WriteLine(myMatrix(row,col))
    Next
Next
```

Using two loops, the previous code will output each element in the multidimensional `myMatrix` array. The outer loop goes through the first dimension, assigning the index to a variable for the row. The inner `For` loop goes through the second dimension, assigning the index to a variable for the column.

## Handling arrays in early VB family members

There is one difference with arrays in early VB family members from the VB.NET examples that we saw earlier. The difference is in how you find the array’s size or an array’s dimension. You can use the UBound function to get the length of the array or the dimension. A number indicates which dimension’s size is returned. Use 1 for the first dimension, 2 for the second, and so on. If the dimension is omitted, one is assumed:

```
Dim rows As Integer = Ubound(myMatrix)
Dim columns As Integer = Ubound(myMatrix,1)
```

Next, we will look at working with lists in VB.

## When and how to use lists

Lists and arrays are data structures that are used in programming to store and organize collections of elements. However, there are some differences between them in terms of their properties and functionality

- **Lists:**
  - **Dynamic size:** In most programming languages, such as Python, lists are dynamic in size, meaning they can grow or shrink during program execution. You can add or remove elements from a list without specifying their size.
  - **Heterogeneous elements:** Lists can store elements of different data types. For example, a list can contain integers, strings, and objects.
  - **Flexible operations:** Lists provide built-in methods for operations, such as appending elements, inserting elements at specific positions, removing elements, and concatenating lists.
  - **Slower access:** Accessing elements in a list is slower than using arrays because lists use pointers to store the elements’ memory addresses. As a result, accessing an element requires following the pointer to the desired memory location.
  - **Higher memory overhead:** Lists generally require more memory than arrays due to their dynamic nature and additional metadata.
- **Arrays:**
  - **Fixed Size:** Arrays have a fixed size that’s determined at the time of declaration and cannot be changed during runtime.
  - **Homogeneous elements:** Arrays only store elements of the same data type. For example, an integer array can only hold integer values, and a string array can only hold string values.
  - **Efficient access:** Arrays provide efficient element access since elements are stored in contiguous memory locations. Accessing an element requires calculating its memory address using an index, a simple arithmetic operation.
  - **Limited operations:** Arrays typically offer fewer built-in operations compared to lists. They usually support basic operations such as element access, modification, and iteration but may not have built-in methods for dynamic resizing or complex operations.
- **Lower memory overhead**: Arrays generally require less memory than lists because they have a fixed size and do not require additional metadata.

To summarize the differences, lists are more flexible and versatile regarding dynamic size and heterogeneous elements. At the same time, arrays offer efficient element access and lower memory overhead but have a fixed size and homogeneous elements. The choice between lists and arrays depends on the specific requirements of your program and the operations you need to perform on the collection of elements.

In VB.NET, a list is represented by the `List(Of T)` class, which is part of the `System.Collections.Generic` namespace. The `List(Of T)` class provides a dynamic, resizable array-like data structure.

Here’s an example of how to use lists in VB.NET:

```
Imports System.Collections.Generic
Dim nums As New List(Of Integer)()
nums.Add(10)
nums.Add(20)
nums.Add(30)
For Each num As Integer In nums
Console.WriteLine(num)
Next
Console.WriteLine("Count: " & nums.Count)
```

The preceding VB.NET code demonstrates the usage of a list to store and display a collection of integer values. Here’s a breakdown of the code’s functionality:

- 1. The code begins by importing the `System.Collections.Generic` namespace, which contains the `List(Of T)` class for creating and manipulating lists.
- 2. A new list called `nums` is declared and instantiated using the `New List(Of Integer)()` syntax. This list is specifically designed to store integer values.
- 3. Using the `Add` method, the code adds three integer elements (10, 20, and 30) to the `nums` list.
- 4. A `For Each` loop is utilized to iterate over each element in the `nums` list. During each iteration, the current element is assigned to the `num` variable, and its value is then printed to the console using `Console.WriteLine(num)`.
- 5. After the loop completes, the code outputs the count of elements in the `nums` list using the `Count` property. The result is printed to the console using `Console.WriteLine("Count: " & nums.Count)`.

In summary, this VB.NET code creates a list called `nums` and adds three integer values. It then iterates over the list and prints each element’s value. Finally, it outputs the count of elements in the list.

There is also an operation to remove items from the list:

```
Nums.Remove(30)
```

VB6, VBA, and VBScript did not have native support for lists. You can instantiate COM objects in these family members as they support lists but you can also use dictionaries to get similar functionality. We will take a look at dictionaries in the next section.

## Storing data in a dictionary

Dictionaries, or associative arrays or hash maps, are widely used data structures in programming. Dictionaries provide a way to store and retrieve data using key-value pairs. The key is used to store and retrieve the related data. There are also methods to return the keys as a list that can be iterated over.

In VB.NET, dictionaries are represented by the Dictionary(Of TKey, TValue) class, which is part of the System.Collections.Generic namespace. Dictionaries provide a data structure that stores key-value pairs, allowing efficient lookup and retrieval of values based on their associated keys.

Here’s an example of how to use dictionaries in VB.NET:

```
Dim studGrades As New Dictionary(Of String, Integer)()
studGrades.Add("Seamus", 92)
studGrades.Add("Freya", 95)
studGrades.Add("Aspen", 89)
For Each pair As KeyValuePair(Of String, Integer) In studGrades
    Console.WriteLine(pair.Key & " Received " & pair.Value)
Next
Console.WriteLine("Students: " & studGrades.Count)
```

The preceding code snippet is written in VB.NET and represents a program that manages student grades using a dictionary data structure.

Here’s a breakdown of the code:

- Dim studGrades As New Dictionary(Of String, Integer)(): This line declares and initializes a new dictionary instance called studGrades. The dictionary is expected to store key-value pairs, where the keys are of the String type (representing student names) and the values are of the Integer type (representing the grades).
- studGrades.Add("Seamus", 92): This line adds a key-value pair to the studGrades dictionary. The key is "Seamus", and the associated value is 92.
- studGrades.Add("Freya", 95): This line adds another key-value pair to the studGrades dictionary. The key is "Freya", and the associated value is 95.
- studGrades.Add("Aspen", 89): This line adds a key-value pair to the studGrades dictionary. The key is "Aspen", and the associated value is 89.
- For Each pair As KeyValuePair(Of String, Integer) In studGrades: This line initiates a loop that iterates through each key-value pair in the studGrades dictionary. The loop assigns each pair to the variable pair.
- Console.WriteLine(pair.Key & " Received " & pair.Value): This line prints the key (student name), followed by the " Received " string, and then the value (grade) associated with that



键，到控制台。

- **Next：** 此关键字表示由 **For Each** 语句启动的循环的结束。

- **Console.WriteLine("Students: " & studGrades.Count)**：此行将 "Students: " 字符串以及 **studGrades** 字典中的项数（键值对的数量）打印到控制台。

在 **VBScript**、**VBA** 和 **VB6** 中，没有像 **VB.NET** 中那样的内置字典数据结构。你可以使用 **Scripting.Dictionary** 对象来实现类似的功能，该对象作为 **Windows Script Host (WSH)** 对象模型的一部分提供。

要在 VBScript 中使用字典，你必须创建 **Scripting.Dictionary** 对象的一个实例，然后使用其方法和属性来操作和访问数据。下面是一个示例：

```vbscript
Dim studGrades
Set studGrades = CreateObject("Scripting.Dictionary")
studGrades.Add "Seamus", 92
studGrades.Add "Freya", 95
studGrades.Add "Aspen", 89
WScript.Echo "Seamus received " & studGrades("Seamus")
WScript.Echo "Freya received " & studGrades("Freya")
WScript.Echo "Aspen received " & studGrades("Aspen")
WScript.Echo "Students: " & studGrades.Count
```

在上面的示例中，我们使用 **CreateObject** 函数创建了一个新的 **Scripting.Dictionary** 对象实例。然后，我们使用 **Add** 方法向字典添加键值对，其中键是学生姓名，值是他们对应的成绩。

要访问字典中的值，我们使用了括号内的键，类似于数组索引。在这种情况下，**studGrades("Seamus")** 检索与 **"Seamus"** 键关联的成绩。

**Count** 属性返回字典中项（键值对）的数量。

请注意，VBScript 中的 **Scripting.Dictionary** 对象不如 **VB.NET** 中的字典强大或功能丰富。它的方法有限，并且不支持强类型键或值。

## 总结

本章介绍了 **VB** 系列中的集合。我们学习了如何使用数组、列表以及更高级的数据结构——字典。这些集合允许我们在单个变量中存储多个值。在下一章中，我们将基于所学内容构建一个更全面的示例项目。
