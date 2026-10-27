## 项目第二部分

在我们的第二个项目中，我们将使用迄今学到的技能来构建一个简单的学生成绩工具，该工具将从文件中读取学生成绩，并为每个 VB 系列成员汇总这些成绩。评分工具应读取**逗号分隔值（CSV）**文件，其中学生姓名和成绩以逗号分隔。评分工具应利用我们在前几章学到的字典和列表数据结构。然后，该评分工具将显示每个学生的摘要。

在本章中，我们将介绍以下主要主题：

- 在 VB.NET 控制台中开发评分工具
- 在 VB 6 中开发评分工具
- 在 VBA 中开发评分工具
- 在 VBScript 中开发评分工具
- 在 Classic ASP 中开发评分工具
- 在 ASP.NET 中开发评分工具

### 技术要求

本章的所有示例代码可在以下 GitHub 仓库中获取：https://github.com/PacktPublishing/Learn-Visual-Basics-Quick-Start-Guide/-/tree/main/ProjectPartII。

### 在 VB.NET 控制台中构建项目

这个简单的控制台应用程序是用 VB.NET 编程语言编写的。此代码片段演示了如何读取包含学生姓名和成绩的 .csv 文件，并将它们存储在带有成绩列表的字典中。让我们分解一下代码：

1. 以下几行导入了文件操作（**System.IO**）和集合操作（**System.Collections.Generic**）所需的命名空间：

   ```vbnet
   Imports System.IO
   Imports System.Collections.Generic
   ```

2. 代码被封装在一个名为 **MainModule** 的模块中；入口点是 **Main** 子程序。**Main** 子程序是程序开始执行的地方：

   ```vbnet
   Module MainModule
       Sub Main()
           ' Main code goes here
       End Sub
   End Module
   ```

3. filePath 变量存储 CSV 文件的路径：

   ```vbnet
   Dim filePath As String = "students.csv"
   ```

4. 以下行初始化了一个名为 studentGrades 的字典。字典的键是表示学生姓名的字符串，值是表示成绩的字符串列表：

   ```vbnet
   Dim studentGrades As New Dictionary(Of String, List(Of String))()
   ```

5. 下一段代码使用 StreamReader 读取由 filePath 指定的 CSV 文件内容。Using 语句确保读取器在使用后正确释放。While 循环会一直持续到到达文件末尾：

   ```vbnet
   Using reader As New StreamReader(filePath)
       While Not reader.EndOfStream
           ' Read and process each line
       End While
   End Using
   ```

6. 以下几行使用 ReadLine() 从 CSV 文件中读取一行，并使用 Split() 方法将其拆分为字段。这些字段预期以逗号分隔：

   ```vbnet
   Dim line As String = reader.ReadLine()
   Dim fields As String() = line.Split(","c)
   ```

   以下条件检查该行是否恰好包含两个字段。如果是，则继续处理学生姓名和成绩：

   ```vbnet
   If fields.Length = 2 Then
       ' Process the student name and grade
   End If
   ```

   这几行从字段数组中提取学生姓名和成绩。Trim() 方法用于删除任何前导或尾随空白：

   ```vbnet
   Dim studentName As String = fields(0).Trim()
   Dim grade As String = fields(1).Trim()
   ```

7. 此代码检查 studentName 是否已作为键存在于 studentGrades 字典中。如果不存在，则向字典添加一个新条目，以 studentName 为键，并以空列表作为初始值：

   ```vbnet
   If Not studentGrades.ContainsKey(studentName) Then
       studentGrades.Add(studentName, New List(Of String)())
   End If
   ```

8. 此行将 grade 添加到 studentGrades 字典中与 studentName 键关联的成绩列表中：

   ```vbnet
   studentGrades(studentName).Add(grade)
   ```

   这个 For Each 循环遍历 studentGrades 字典的 Keys 集合中的每个 studentName 实例。它允许你处理每个学生的姓名和关联的成绩：

   ```vbnet
   For Each studentName As String In studentGrades.Keys
       ' Process each student name and grades
   Next
   ```

   在循环内，此代码检索当前 studentName 的成绩列表，并使用 String.Join() 将它们连接成逗号分隔的字符串。然后使用 Console.WriteLine() 将学生姓名及其成绩打印到控制台：

   ```vbnet
   Dim grades As List(Of String) = studentGrades(studentName)
   Dim gradeList As String = String.Join(", ", grades)
   Console.WriteLine(studentName & ": " & gradeList)
   ```

9. 此行等待用户输入，有效地暂停控制台窗口，以便在程序退出前查看输出：

   ```vbnet
   Console.ReadLine()
   ```

以下是读取 CSV 文件、处理数据并将其存储在带有成绩列表的字典中的完整代码。然后将学生姓名及其对应的成绩输出到控制台：

```vbnet
Imports System.IO
Imports System.Collections.Generic

Module MainModule
    Sub Main()
        Dim filePath As String = "students.csv"
        Dim studentGrades As New Dictionary(Of String, List(Of String))()

        Using reader As New StreamReader(filePath)
            While Not reader.EndOfStream
                Dim line As String = reader.ReadLine()
                Dim fields As String() = line.Split(","c)
                If fields.Length = 2 Then
                    Dim studentName As String = fields(0).Trim()
                    Dim grade As String = fields(1).Trim()

                    If Not studentGrades.ContainsKey(studentName) Then
                        studentGrades.Add(studentName, New List(Of String)())
                    End If

                    studentGrades(studentName).Add(grade)
                End If
            End While
        End Using

        For Each studentName As String In studentGrades.Keys
            Dim grades As List(Of String) = studentGrades(studentName)
            Dim gradeList As String = String.Join(", ", grades)
            Console.WriteLine(studentName & ": " & gradeList)
        Next

        Console.ReadLine()
    End Sub
End Module
```

### 编写 VB.NET 控制台评分工具

以下步骤向您展示如何编写和执行 VB.NET Windows Forms 版本的 Hello World：

1. 启动 Microsoft Visual Studio。





+   2. 选择 **Create New Project**。
+   3. 选择 **Console (.NET Framework)** 并点击 **Next**。
+   4. 接受 **Default Project Name** 和 **Solution Name**。
+   5. 如需将代码放在不同文件夹中，请更改位置。
+   6. 接受默认的 **Framework** 选项。
+   7. 点击 **Create Project**。
+   8. 输入前一节的代码示例。
+   9. 要运行程序，请点击代码上方工具栏上的绿色三角形。

## NOTE

> *请确保 `students.csv` 文件与可执行代码在同一文件夹中；否则，它会抛出 `System.IO.FileNotFoundException` 异常。*

我们下一节将讲解 VB 6 中的计算器示例。

## Building the project in VB6

我们现在将为成绩工具程序构建 **VB 6** 代码。此代码读取学生和成绩的 CSV 文件并将它们存储在字典中。它使用 `Scripting.Dictionary` 对象创建字典，并使用 `Collection` 对象存储每个学生的成绩。

以下是完整的代码，它读取 CSV 文件，将学生姓名和成绩存储在字典中，然后打印它们及其对应的成绩：

```
Option Explicit
Dim studentGrades As Object
Sub ReadCSVFile()
    Dim filePath As String
    Dim fileNum As Integer
    Dim line As String
    Dim fields() As String
    Dim studentName As String
    Dim grade As String
    filePath = "students.csv"
    Set studentGrades = CreateObject("Scripting.Dictionary")
    fileNum = FreeFile
    Open filePath For Input As fileNum
    Do Until EOF(fileNum)
        Line Input #fileNum, line
        fields = Split(line, ",")
        If UBound(fields) = 1 Then
            studentName = Trim(fields(0))
            grade = Trim(fields(1))
            If Not studentGrades.Exists(studentName) Then
                studentGrades.Add studentName, New Collection
            End If
            studentGrades(studentName).Add grade
        End If
    Loop
    Close fileNum
    Dim student As Variant
    Dim gradeList as String
```

+   16. `fields = Split(line, ",")`: 这使用逗号分隔符将行拆分为字段数组。
+   
+   17. `If UBound(fields) = 1 Then`: 这检查字段数是否等于 2。如果为真，则表示有两个字段：`studentName` 和 `grade`。
+   
+   18. `studentName = Trim(fields(0))`: 这将第一个字段（学生姓名）的修剪值分配给 `studentName` 变量。
+   
+   19. `grade = Trim(fields(1))`: 这将第二个字段（`grade`）的修剪值分配给 `grade` 变量。
+   
+   20. `If Not studentGrades.Exists(studentName) Then`: 这检查 `studentGrades` 字典是否尚未包含当前学生姓名的条目。
+   
+   21. `studentGrades.Add studentName, New Collection`: 这向 `studentGrades` 字典添加新条目，以学生的姓名作为键，以新的 `Collection` 对象作为值。`Collection` 对象将存储学生的成绩。
+   
+   22. `studentGrades(studentName).Add grade`: 这将当前成绩添加到与 `studentGrades` 字典中学生姓名关联的 `Collection` 对象。
+   
+   23. `Loop`: 这是循环的结束。它继续从文件读取行，直到到达文件末尾。
+   
+   24. `Close fileNum`: 这关闭 CSV 文件。
+   
+   25. `Dim student As Variant`: 这声明一个名为 `student` 的变量，用于遍历 `studentGrades` 字典中的键（学生姓名）。
+   
+   26. `For Each student In studentGrades.Keys`: 这开始一个循环，遍历 `studentGrades` 字典中每个学生的姓名。
+   
+   27. `studentName = student`: 这将当前学生姓名分配给 `studentName` 变量。
+   
+   28. `gradeList = GetGradeList(studentName)`: 这调用 `GetGradeList` 函数以检索与当前学生姓名关联的成绩。
+   
+   29. `Debug.Print studentName & ": " & gradeList`: 这将学生姓名和对应的成绩列表打印到调试输出。
+   
+   30. `Next student`: 这是遍历字典中学生姓名的循环的结束。
+   
+   31. `End Sub`: 这是 `ReadCSVFile` 子程序的结束。
+   
+   32. `Function GetGradeList(ByVal studentName As String) As String`: 这是一个函数，它接受学生姓名作为输入并返回对应的成绩列表作为字符串。
+   
+   33. `Dim gradeList As String`: 这声明一个名为 `gradeList` 的变量，用于将成绩列表存储为字符串。
+   
+   34. `Dim grades As Collection`: 这声明一个名为 `grades` 的变量，用于存储与学生姓名关联的 `Collection` 对象。
+   
+   35. `Set grades = studentGrades.Item(studentName)`: 这将 `studentGrades` 字典中与学生姓名关联的 `Collection` 对象分配给 `grades` 变量。
+   
+   36. `Dim grade As Variant`: 这声明一个名为 `grade` 的变量，用于遍历 `grades` Collection 对象中的成绩。
+   
+   37. `For Each grade In grades`: 这开始一个循环，遍历 `grades` Collection 对象中的每个成绩。

38. **gradeList = gradeList & grade & ", "**: 这将当前成绩追加到 **gradeList** 字符串，以逗号和空格分隔。

39. **If Len(gradeList) > 0 Then**: 这检查 **gradeList** 字符串是否不为空。

40. **gradeList = Left(gradeList, Len(gradeList) - 2)**: 这使用 **Left** 函数从 **gradeList** 字符串中删除尾部的逗号和空格。

41. **GetGradeList = gradeList**: 这将 **gradeList** 字符串分配为函数的返回值。

## Coding the VB 6 grading tool

以下步骤允许你编码并执行 VB 6 版本的成绩工具：

+   1. 启动 **Microsoft Visual Basic 6**。
+   2. 选择 **Standard EXE** 作为项目类型。
+   3. 设计器加载后，双击代表窗体的矩形。
+   4. 输入前一节的代码示例。
+   5. 要运行程序，请点击代码上方工具栏上的绿色三角形。

在下一节中，我们将讲解 VBA 中的成绩工具示例。

## Building the project in VBA

接下来是一个 **Visual Basic for Applications (VBA)** 应用程序，它定义了一个名为 **ReadCSVFile** 的子程序，该子程序读取包含学生姓名和成绩的 CSV 文件。让我们分解代码：

+   1. **Sub ReadCSVFile()**: 这一行开始定义 **ReadCSVFile** 子程序。它表示它不返回任何值。
+   2. 这些行声明子程序中使用的多个变量：
+      `Dim fso As Object    Dim inputFile As Object    Dim studentGrades As Object    Dim line As String    Dim fields() As String    Dim studentName As String    Dim grade As String    Dim grades() As String    Dim gradeList As String`
+      1. **fso**、**inputFile** 和 **studentGrades** 声明为通用 **Object** 类型，并将在以后分配给特定对象。
+      2. **line**、**fields()**、**studentName**、**grade**、**grades()** 和 **gradeList** 声明为特定数据类型（**String** 和 **String** 数组），用于在子程序执行期间存储不同信息。

这些行创建对象并将它们设置为来自 **Scripting** 库的特定对象：

```
Set fso = CreateObject("Scripting.FileSystemObject")
Set inputFile = fso.OpenTextFile("students.csv", 1, False)
```

```
Set studentGrades = CreateObject("Scripting.Dictionary")
```

3. fso 被设置为使用 CreateObject 函数创建的 FileSystemObject 对象。它提供对文件系统操作的访问。

4. inputFile 被设置为由 fso 的 OpenTextFile 方法打开的文本文件。它以读取模式（1）打开 students.csv 文件；False 表示它应为非 Unicode 文件。

5. studentGrades 被设置为使用 CreateObject 函数创建的 Dictionary 对象。它将存储学生的姓名作为键，其成绩作为值。

3. Do Until 循环迭代直到到达输入文件的末尾。它使用 inputFile 的 ReadLine 方法读取文件的每一行并将其分配给 line 变量。然后，它使用 Split 函数以逗号（,）为分隔符将 line 拆分为 fields，并将它们存储在 fields 数组中：

```
Do Until inputFile.AtEndOfStream        line = inputFile.ReadLine
    fields = Split(line, ",")        ...    Loop
```

4. If 语句检查 fields 数组的长度是否为 2。如果是，它假设该行包含学生的姓名和成绩。它在修剪首尾空格后，使用 Trim 函数将第一个字段（学生姓名）分配给 studentName 变量，将第二个字段（成绩）分配给 grade 变量：

```
If UBound(fields) = 1 Then
    studentName = Trim(fields(0))
    grade = Trim(fields(1))
    ...
End If
```

5. If-Else 语句检查 studentName 是否已作为键存在于 studentGrades 字典中。如果不存在，它向字典添加新条目，以 studentName 作为键，以仅包含 grade 的数组作为值。如果 studentName 已存在，它从字典中检索现有的成绩数组，使用 ReDim Preserve 将其调整大小以容纳新成绩，然后将新成绩分配给 grades 数组的最后一个位置。最后，它使用更新后的 grades 数组更新字典条目：

```
If Not studentGrades.Exists(studentName) Then
    studentGrades.Add studentName, Array(grade)
Else
    grades = studentGrades(studentName)
    ReDim Preserve grades(UBound(grades) + 1)
    grades(UBound(grades)) = grade
    studentGrades(studentName) = grades
End If
```

6. 下一行关闭输入文件：

```
inputFile.Close
```

7. For Each 循环遍历 studentGrades 字典的 Keys 集合中的每个 studentName。对于每个学生姓名，它从字典中检索对应的 grades 数组，使用 Join 函数将成绩连接成以逗号分隔的字符串，然后使用 Debug.Print 输出学生姓名和成绩列表：

```
For Each studentName In studentGrades.Keys
    grades = studentGrades(studentName)
    gradeList = Join(grades, ", ")
    Debug.Print studentName & ": " & gradeList
Next studentName
```





总体而言，这段代码读取 CSV 文件，将学生姓名和成绩存储在字典中，然后使用 `Debug.Print` 打印学生姓名及其相应的成绩。循环会一直继续，直到用户输入 **x** 退出，此时变量 `sentinel` 被设为 `True`，循环结束：

```
Option Explicit
Sub ReadCSVFile()
    Dim fso As Object
    Dim inputFile As Object
    Dim studentGrades As Object
    Dim line As String
    Dim fields() As String
    Dim studentName As String
    Dim grade As String
    Dim grades() As String
    Dim gradeList As String
    Set fso = CreateObject("Scripting.FileSystemObject")
    Set inputFile = fso.OpenTextFile("students.csv", 1, False)
    Set studentGrades = CreateObject("Scripting.Dictionary")
    Do Until inputFile.AtEndOfStream
        line = inputFile.ReadLine
        fields = Split(line, ",")
        If UBound(fields) = 1 Then
            studentName = Trim(fields(0))
            grade = Trim(fields(1))
            If Not studentGrades.Exists(studentName) Then
                studentGrades.Add studentName, Array(grade)
            Else
                grades = studentGrades(studentName)
                ReDim Preserve grades(UBound(grades) + 1)
                grades(UBound(grades)) = grade
                studentGrades(studentName) = grades
            End If
        End If
    Loop
    inputFile.Close
    For Each studentName In studentGrades.Keys
        grades = studentGrades(studentName)
        gradeList = Join(grades, ", ")
        Debug.Print studentName & ": " & gradeList
    Next studentName
End Sub
```

## 在 Excel 中编写 VBA 评分工具

以下步骤可帮助你编写并执行一个显示 **Hello World** 的 VBA 宏：

- 1. 启动 **Microsoft Excel**。
- 2. 显示 **View** 菜单。
- 3. 点击 **Macros** 按钮并从下拉菜单中选择 **View Macros**。
- 4. 输入 **test** 作为宏的名称。
- 5. 点击 **Create** 按钮。
- 6. 当设计器加载时，双击代表你窗体的矩形区域。
- 7. 输入前一节中的代码示例。
- 8. 点击代码上方工具栏上的绿色三角形运行你的宏。

在下一节中，我们将处理 VBScript 中的评分工具示例。

## 在 VBScript 中构建项目

以下 **VBScript** 程序读取包含学生姓名和成绩的 CSV 文件。它将数据存储在字典中，每个学生的姓名关联着其成绩列表。让我们逐步分解代码：

- 1. **Set fso = CreateObject("Scripting.FileSystemObject")**：此行创建 **FileSystemObject** 对象的实例，该对象提供对文件系统的访问。它允许我们与文件交互，例如打开和读取文件。
- 2. **Set inputFile = fso.OpenTextFile("students.csv", 1, False)**：此行使用 **FileSystemObject** 的 **OpenTextFile** 方法以读取模式（1）打开 **students.csv** 文件。**False** 参数表示文件应作为 Unicode 文件打开。
- 3. **Dim studentGrades**：此行声明一个名为 **studentGrades** 的变量但未为其赋值。它稍后将用于存储学生姓名和成绩。
- 4. **Set studentGrades = CreateObject("Scripting.Dictionary")**：此行使用 **CreateObject** 函数创建 **Dictionary** 对象的实例。**Dictionary** 对象允许你存储键值对。
- 5. **Do Until inputFile.AtEndOfStream**：这开始一个循环，直到到达输入文件的末尾。
- 6. **line = inputFile.ReadLine**：这从输入文件读取下一行并将其分配给 **line** 变量。
- 7. **fields = Split(line, ",")**：此行使用逗号作为分隔符将 **line** 变量拆分为 **fields** 数组。
- 8. **If UBound(fields) = 1 Then**：此条件检查 **fields** 数组的长度是否为 2。如果是，则假定该行包含学生的姓名和成绩。
- 9. **studentName = Trim(fields(0)) and grade = Trim(fields(1))**：这些行将第一个字段（学生姓名）分配给 **studentName** 变量，将第二个字段（成绩）分配给 **grade** 变量。**Trim** 函数用于从值中删除前导和尾随空格。
- 10. **If Not studentGrades.Exists(studentName) Then**：此条件检查 **studentName** 是否已作为键存在于 **studentGrades** 字典中。
- 11. **studentGrades.Add studentName, Array(grade)**：如果学生姓名不存在于字典中，则添加一个新条目，以 **studentName** 作为键，以仅包含 **grade** 的数组作为值。
- 12. **Else**：如果学生姓名已存在于字典中，则执行此代码块。
- 13. **grades = studentGrades(studentName)**：此行从字典中检索与 **studentName** 关联的成绩列表。
- 14. **ReDim Preserve grades(UBound(grades) + 1)**：此行调整 **grades** 数组的大小以容纳新成绩。**Preserve** 关键字确保在调整大小过程中现有成绩不会丢失。
- 15. **grades(UBound(grades)) = grade**：此行将新成绩变量分配给 grades 数组的最后一个位置。
- 16. **studentGrades(studentName) = grades**：这使用更新后的 grades 数组更新 studentName 的字典条目。
- 17. **Loop**：这是循环的结束。它会一直继续，直到到达输入文件的末尾。
- 18. **inputFile.Close**：此行关闭输入文件。
- 19. **For Each studentName In studentGrades**：此循环遍历 studentGrades 字典中的每个学生姓名。
- 20. **grades = studentGrades(studentName)**：此行检索与当前 studentName 关联的成绩列表。
- 21. **gradeList = Join(grades, ", ")**：此行将 grades 数组元素连接成一个字符串，以逗号和空格分隔。
- 22. **WScript.Echo studentName & ": " & gradeList**：此行将学生姓名及其对应的成绩列表输出到控制台。

总体而言，这段代码读取 CSV 文件，将学生姓名和成绩存储在字典中，然后打印学生姓名及其相应的成绩：

```
Set fso = CreateObject("Scripting.FileSystemObject")
Set inputFile = fso.OpenTextFile("students.csv", 1, False)
Dim studentGrades
Set studentGrades = CreateObject("Scripting.Dictionary")
Do Until inputFile.AtEndOfStream
  line = inputFile.ReadLine
  fields = Split(line, ",")
  If UBound(fields) = 1 Then
    studentName = Trim(fields(0))
    grade = Trim(fields(1))
    If Not studentGrades.Exists(studentName) Then
      studentGrades.Add studentName, Array(grade)
    Else
      grades = studentGrades(studentName)
      ReDim Preserve grades(UBound(grades) + 1)
      grades(UBound(grades)) = grade
      studentGrades(studentName) = grades
    End If
  End If
Loop
inputFile.Close
For Each studentName In studentGrades
  grades = studentGrades(studentName)
  gradeList = Join(grades, ", ")
  WScript.Echo studentName & ": " & gradeList
Next
```

## 使用记事本编写 VBScript 评分工具

以下步骤展示了如何编写并执行之前的 VBScript，该脚本运行一个计算器程序：

- 1. 启动记事本。
- 2. 输入前一节中的代码。
- 3. 将文件保存为 **VBScriptGradesFromFile.vbs** 到你的根文件夹。
- 4. 运行 **CMD** 进入命令提示符。
- 5. 输入 **cscript VBScriptGradesFromFile.vbs** 并按下 *Enter* 键。

## NOTE

*如前所述，请确保 students.csv 文件位于你运行的同一目录中。*

在下一节中，我们将处理 Classic ASP 中的评分工具示例。

## 在 Classic ASP 中构建项目

以下是一个 Classic ASP 程序，它读取学生和成绩的 CSV 文件，然后将数据存储在字典中，每个学生对应一个成绩列表。以下是代码的分解：

- 1. 代码以 `<%` 开始并以 ` %>` 结束，表示它嵌入在 ASP 代码块中。
- 2. `fso`、`inputFile`、`studentGrades`、`line`、`fields`、`studentName`、`grade`、`grades` 和 `gradeList` 变量使用 `Dim` 语句声明，以在处理期间保存各种数据。
- 3. `Server.CreateObject` 方法创建 `Scripting.FileSystemObject` 和 `Scripting.Dictionary` 对象的实例（`fso` 和 `studentGrades`）。
- 4. `Server.MapPath` 方法用于获取 `students.csv` 文件相对于服务器文件系统的物理路径。
- 5. `fso` 对象的 `OpenTextFile` 方法被调用以打开 `students.csv` 文件进行读取。
- 6. 使用 `Do Until` 语句启动一个循环，以读取输入文件中的每一行，直到到达文件末尾（`inputFile.AtEndOfStream`）。
- 7. 从文件中读取的每一行使用 `Split` 函数以逗号（,）作为分隔符拆分为字段。
- 8. `UBound` 函数检查 `fields` 数组是否恰好有两个元素。如果是，则从 `fields` 数组中提取学生姓名和成绩并存储在相应的变量中。
- 9. 在条件块内部，代码使用 `Exists` 方法检查学生姓名是否已存在于 `studentGrades` 字典中。如果不存在，则向字典中添加一个新条目，以学生姓名作为键，以包含成绩的数组作为值。
- 10. 如果学生姓名已存在于字典中，则使用 `studentName` 作为键检索现有的 `grades` 数组。使用 `ReDim Preserve` 语句调整 `grades` 数组的大小以容纳新成绩，并将新成绩添加到数组末尾。最后，将更新后的 `grades` 数组存储回 `studentGrades` 字典中。
- 11. 处理完所有行后，使用 `Close` 语句关闭输入文件。
- 12. 代码随后使用 `For Each` 语句进入一个循环，遍历 `studentGrades` 字典中的每个学生姓名。
- 13. 在循环内部，从字典中检索当前学生姓名对应的成绩。`Join` 函数将成绩连接成一个逗号分隔的字符串。





- 14. 学生姓名和成绩列表使用 `Response.Write` 语句写入响应。`<br>` 标签被追加以在每个学生的信息之间创建换行。

完整的代码读取 CSV 文件，将学生成绩数据组织到字典中，并以格式化方式输出学生姓名及其对应的成绩：

```
<%
Dim fso, inputFile, studentGrades, line, fields, studentName, grade, grades, gradeList
Set fso = Server.CreateObject("Scripting.FileSystemObject")
Set inputFile = fso.OpenTextFile(Server.MapPath("students.csv"), 1, False)
Set studentGrades = Server.CreateObject("Scripting.Dictionary")
Do Until inputFile.AtEndOfStream
    line = inputFile.ReadLine
    fields = Split(line, ",")
    If UBound(fields) = 1 Then
        studentName = Trim(fields(0))
        grade = Trim(fields(1))
        If Not studentGrades.Exists(studentName) Then
            studentGrades.Add studentName, Array(grade)
        Else
            grades = studentGrades(studentName)
            ReDim Preserve grades(UBound(grades) + 1)
            grades(UBound(grades)) = grade
            studentGrades(studentName) = grades
        End If
    End If
Loop
inputFile.Close
For Each studentName In studentGrades.Keys
    grades = studentGrades(studentName)
    gradeList = Join(grades, ", ")
    Response.Write studentName & ": " & gradeList & "<br>"
Next
%>
```

## 使用记事本编写 Classic ASP 评分工具

以下步骤展示了如何编写和执行一个 VBScript，使其显示我们在本项目中开发的计算器：

- 1. 启动记事本。
- 2. 输入前面部分中的所有示例代码。
- 3. 将文件保存为 ClassicASPGradingTool.asp 到根 Web 目录 – 通常是 c:\inetpub\wwwroot。
- 4. 将 students.csv 文件放入同一文件夹。
- 5. 打开一个 Web 浏览器。
- 6. 输入 http://127.0.0.1/ClassicASPGradingTool.asp。

在下一节中，我们将处理 ASP.NET 中的评分工具示例。

## 在 ASP.NET 中构建项目

下面是评分工具的 ASP.NET 代码。此代码与为 VB.NET 控制台应用程序提供的代码非常相似。在此示例中，无需更改 ASPX 代码，因为输出是通过 `response.write` 方法处理的：

```
Imports System.IO
Public Class ASPNETGradingTool
    Inherits System.Web.UI.Page
    Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
        Dim filePath As String = "students.csv"
        Dim studentGrades As New Dictionary(Of String, List(Of String))()
        Using reader As New StreamReader(filePath)
            While Not reader.EndOfStream
                Dim line As String = reader.ReadLine()
                Dim fields As String() = line.Split(","c)
                If fields.Length = 2 Then
                    Dim studentName As String = fields(0).Trim()
                    Dim grade As String = fields(1).Trim()
                    If Not studentGrades.ContainsKey(studentName) Then
                        studentGrades.Add(studentName, New List(Of String)())
                    End If
                    studentGrades(studentName).Add(grade)
                End If
            End While
        End Using
        For Each studentName As String In studentGrades.Keys
            Dim grades As List(Of String) = studentGrades(studentName)
            Dim gradeList As String = String.Join(", ", grades)
            Response.Write(studentName & ": " & gradeList & "</br>")
        Next
    End Sub
End Class
```

## 使用 Visual Studio 编写 ASP.NET 计算器

以下步骤将允许你编写和执行一个 ASP.NET 网页以显示 `hello world`：

- 1. 启动 **Microsoft Visual Studio 2019**。
- 2. 选择 **Create New Project**。
- 3. 选择 **ASP.NET Web Application** 并点击 Next。
- 4. 接受 **Default Project Name** 和 **Solution Name**。
- 5. 如果你希望代码位于不同文件夹，请更改位置。
- 6. 接受默认的 **Framework** 选项。
- 7. 点击 **Create Project**。
- 8. 选择 **Web Forms**。
- 9. 当设计器加载时，导航到 **Solution Explorer** 并右键单击解决方案。
- 10. 从弹出菜单中选择 **Add Item**。
- 11. 选择 **Web Form Visual Basic**。
- 12. 右键单击 ASPX 源并从弹出菜单中选择 **View Code**。
- 13. 输入前面部分代码示例中位于 `Page_load` 过程中的 VB.NET 代码。
- 14. 要运行程序，请点击代码上方工具栏上的绿色三角形。

## 总结

在本章中，我们利用迄今为止获得的技能为我们 VB 家族的每个成员构建了一个学生评分工具。该项目采用了我们在 *Chapter 7* 中看到的项目的技能，并增加了集合和文件输入的使用。下一章将探讨在利用 **object-oriented programming (OOP)** 范式的同时如何组织我们的代码。
