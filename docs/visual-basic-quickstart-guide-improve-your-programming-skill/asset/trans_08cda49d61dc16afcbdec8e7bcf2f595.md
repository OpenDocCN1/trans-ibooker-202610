

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

