## 项目第三部分

在我们的第三个项目中，我们将运用本书*第3部分*所学到的技能，构建一个简单的学生成绩工具，用于从文件读取学生成绩。根据评估类别，此项目版本将支持每行中的不同数据。我们将使用继承，使每个成绩都是一个可聚合的通用对象，并针对每种评估类型有一个专门的版本。成绩工具应读取一个**逗号分隔值（CSV）**文件，其中包含学生姓名和成绩，以逗号分隔。每行的第三列将是评估类型。我们将支持三种评估类型：Test、Quiz和HW。对于每个Test成绩，第四个字段将是参加测试所用的时间。对于每个Quiz，我们将在第四个字段中获取参加次数。对于HW，我们将存储提交日期。成绩工具应利用我们最近几章学到的字典。然后，面向对象成绩工具将按成绩类别显示每个学生的摘要。我们将项目范围限制为**VB.NET**和**ASP.NET**，因为较旧的**Visual Basic**家族成员不支持继承。

在本章中，我们将涵盖以下主要主题：

- 在VB.NET控制台中开发面向对象成绩工具
- 在ASP.NET中开发面向对象成绩工具

## 技术要求

本章的所有示例代码可在以下GitHub仓库中获取：https://github.com/PacktPublishing/Learn-Visual-Basics-Quick-Start-Guide/-/tree/main/ProjectPartIII

## 在VB.NET控制台中构建项目

这个简单的控制台应用程序是用**VB.NET**编程语言编写的。我们将从探索必须为项目开发的不同类开始。以下代码定义了一个名为`clsGrade`的**VB.NET**类，它封装了一个与评分或打分相关的概念。它提供了一个私有字段`piScore`来存储数值分数，以及一个公共属性`Score`，允许获取和设置该分数的值。

以下是对后续代码的逐项说明：

- `Public Class clsGrade`：此行声明了一个名为`clsGrade`的公共类。
- `Private piScore As Integer`：此行声明了一个名为`piScore`的`Integer`类型私有字段，用于存储数值分数。




- **Public Property Score As Integer**: 这行定义了一个名为 Score 的公共属性。属性允许在封装实现细节的同时访问或修改私有字段。
- **Get**: 这个块定义了 Score 属性的 getter。当访问该属性时，这个块会被执行并返回 piScore 私有字段的值。
- **Set(value As Integer)**: 这个块定义了 Score 属性的 setter。当给属性赋值时，这个块会被执行并将提供的值赋给 piScore 私有字段。

在此代码中使用属性允许你以受控的方式与 piScore 字段交互。属性提供了一层抽象，允许你在不直接访问底层字段的情况下获取和设置分数。这对于维护数据完整性、应用验证或在值被访问或修改时执行额外操作非常有用。下面是代码示例：

```
Public Class clsGrade
    Private piScore As Integer
    Public Property Score As Integer
        Get
            Return piScore
        End Get
        Set(value As Integer)
            piScore = value
        End Set
    End Property
End Class
```

下一节代码定义了一个名为 clsTestGrade 的 VB.NET 类，它扩展或继承自另一个名为 clsGrade 的类。clsTestGrade 类表示一个测验成绩，并且除了分数之外，还增加了与测试时长相关的功能。

以下是代码的分解，其遵循此分解和说明：

- **Public Class clsTestGrade**: 这行声明了一个名为 clsTestGrade 的类，旨在表示一个测验成绩。
- **Inherits clsGrade**: 这行表示 clsTestGrade 类继承自另一个名为 clsGrade 的类。这意味着 clsTestGrade 继承了 clsGrade 类的成员（字段、属性、方法等）和行为。
- **Private piDuration As Integer**: 这行声明了一个名为 piDuration 的私有字段，类型为 Integer，用于存储测试时长。
- **Public Sub New(piScore As Integer, piDuration As Integer)**: 这是 clsTestGrade 类的构造函数。它接受两个参数：piScore 和 piDuration。当创建 clsTestGrade 的实例时，会调用此构造函数来初始化其属性。
- **Me.Score = piScore**: 这行将 clsTestGrade 实例的 Score 属性设置为 piScore 参数的值。Score 属性继承自 clsGrade 类。
- **Me.piDuration = piDuration**: 这行将 clsTestGrade 实例的 piDuration 字段设置为 piDuration 参数的值。
- **Public Property Duration As Integer**: 这行定义了一个名为 Duration 的公共属性。此属性允许获取和设置测试的时长。
- **Get**: 这个块定义了 **Duration** 属性的 getter。当访问该属性时，这个块会被执行并返回 **piDuration** 私有字段的值。
- **Set(value As Integer)**: 这个块定义了 **Duration** 属性的 setter。当给属性赋值时，这个块会被执行并将提供的值赋给 **piDuration** 私有字段。

该类旨在处理测验成绩并包含测试时长的额外属性。继承自 **clsGrade** 类提供了与成绩相关的通用属性或方法，而 **clsTestGrade** 则扩展了它以包含与测验评分和时长相关的特定行为。下面是代码：

```
Public Class clsTestGrade
    Inherits clsGrade
    Private piDuration As Integer
    Public Sub New(piScore As Integer, piDuration As Integer)
        Me.Score = piScore
        Me.piDuration = piDuration
    End Sub
    Public Property Duration As Integer
        Get
            Return piDuration
        End Get
        Set(value As Integer)
            piDuration = value
        End Set
    End Property
End Class
```

在下一个代码段中，我们定义了一个名为 **clsQuizGrade** 的 **VB.NET** 类，它扩展或继承自另一个名为 **clsGrade** 的类。**clsQuizGrade** 类表示一个测验成绩，并包含有关测验尝试次数和分数的信息。

以下是代码的分解，其遵循此分解和说明：

- **Public Class clsQuizGrade**: 这行声明了一个名为 **clsQuizGrade** 的类，旨在表示一个测验成绩。
- **Inherits clsGrade**: 这行表示 **clsQuizGrade** 类继承自另一个名为 **clsGrade** 的类。**clsGrade** 类包含与评分相关的通用属性或方法，而 **clsQuizGrade** 则扩展了它以包含与测验评分相关的特定行为。
- **Private piAttempts As Integer**: 这行声明了一个名为 **piAttempts** 的私有字段，类型为 **Integer**，用于存储测验的尝试次数。
- **Public Sub New(piScore As Integer, piAttempts As Integer)**: 这是 **clsQuizGrade** 类的构造函数。它接受两个参数：**piScore** 和 **piAttempts**。当创建 **clsQuizGrade** 的实例时，会调用此构造函数来初始化其属性。
- **Me.Score = piScore**: 这行将 **clsQuizGrade** 实例的 **Score** 属性设置为 **piScore** 参数的值。**Score** 属性继承自 **clsGrade** 类。
- **Me.piAttempts = piAttempts**: 这行将 **clsQuizGrade** 实例的 **piAttempts** 字段设置为 **piAttempts** 参数的值。
- **Public Property Attempts As Integer**: 这行定义了一个名为 **Attempts** 的公共属性。此属性允许获取和设置测验的尝试次数。
- **Get**: 这个块定义了 **Attempts** 属性的 getter。当访问该属性时，这个块会被执行并返回 **piAttempts** 私有字段的值。
- **Set(value As Integer)**: 这个块定义了 **Attempts** 属性的 setter。当给属性赋值时，这个块会被执行并将提供的值赋给 **piAttempts** 私有字段。

该类旨在处理测验成绩并包含测验尝试次数的额外属性。继承自 **clsGrade** 类提供了标准的评分相关功能，而 **clsQuizGrade** 则扩展了它以包含测验特定的行为：

```
Public Class clsQuizGrade
    Inherits clsGrade
    Private piAttempts As Integer
    Public Sub New(piScore As Integer, piAttempts As Integer)
        Me.Score = piScore
        Me.piAttempts = piAttempts
    End Sub
    Public Property Attempts As Integer
        Get
            Return piAttempts
        End Get
        Set(value As Integer)
            piAttempts = value
        End Set
    End Property
End Class
```

以下代码定义了一个名为 **clsHWGrade** 的 VB.NET 类，它继承自另一个名为 **clsGrade** 的类。**clsHWGrade** 类表示一个作业成绩，并包含有关提交日期和分数的信息。

以下是代码的分解：

- **Public Class clsHWGrade**: 这行声明了一个名为 **clsHWGrade** 的类，旨在表示一个作业成绩。
- **Inherits clsGrade**: 这行表示 **clsHWGrade** 类继承自另一个名为 **clsGrade** 的类。这表明 **clsGrade** 类可能包含与评分相关的属性或方法，而 **clsHWGrade** 则扩展了它以包含作业特定的行为。
- **Private pdSubmitted As Date**: 这行声明了一个名为 **pdSubmitted** 的私有字段，类型为 **Date**，用于存储作业提交日期。
- **Public Sub New(piScore As Integer, pdSubmitted As Date)**: 这是 **clsHWGrade** 类的构造函数。它接受两个参数：**piScore** 和 **pdSubmitted**。当创建 **clsHWGrade** 的实例时，会调用此构造函数来初始化其属性。
- **Me.Score = piScore**: 这行将 **clsHWGrade** 实例的 **Score** 属性设置为 **piScore** 参数的值。**Score** 属性继承自 **clsGrade** 类。
- **Me.pdSubmitted = pdSubmitted**: 这行将 **clsHWGrade** 实例的 **pdSubmitted** 字段设置为 **pdSubmitted** 参数的值。




- **Public Property Submitted As Date**：此行定义了一个名为 **Submitted** 的公共属性。该属性允许获取和设置作业的提交日期。
- **Get**：此块定义了 **Submitted** 属性的 getter。当访问该属性时，将执行此块并返回 **pdSubmitted** 私有字段的值。
- **Set(value As Date)**：此块定义了 **Submitted** 属性的 setter。当为该属性赋值时，将执行此块并将提供的值赋给 **pdSubmitted** 私有字段。

该类设计用于处理作业成绩，并包含一个额外的属性来记录作业提交日期。继承自 `clsGrade` 类提供了通用的评分相关功能，而 `clsHWGrade` 扩展了它，以包含作业特有的行为。代码如下：

```vbnet
Public Class clsHWGrade
    Inherits clsGrade
    Private pdSubmitted As Date
    Public Sub New(piScore As Integer, pdSubmitted As Date)
        Me.Score = piScore
        Me.pdSubmitted = pdSubmitted
    End Sub
    Public Property Submitted As Date
        Get
            Return pdSubmitted
        End Get
        Set(value As Date)
            pdSubmitted = value
        End Set
    End Property
End Class
```

以下代码从包含学生成绩的 CSV 文件中读取数据，并计算每个学生作业（测试、测验和家庭作业）的平均分。它使用字典和列表来组织和处理数据。以下是代码的详细说明：

- **Imports System.IO** 导入 **System.IO** 命名空间，该命名空间提供了用于输入输出操作（包括文件读取）的类。
- **Module Module1** 定义了一个名为 **Module1** 的 VB.NET 模块。
- **Sub Main()** 是程序的入口点，执行从这里开始。
- **Dim filePath As String = "..\\students.csv"** 定义了包含学生成绩的 CSV 文件的相对路径。
- **Dim studentGrades As New Dictionary(Of String, List(Of clsGrade))()** 初始化一个名为 **studentGrades** 的字典，该字典将学生姓名（键）与成绩对象列表（值）关联起来。
- **Using reader As New StreamReader(filePath)** 开始一个使用 **StreamReader** 读取 CSV 文件的代码块。
- **While Not reader.EndOfStream** 循环遍历 CSV 文件中的每一行。
- **Dim line As String = reader.ReadLine()** 从 CSV 文件中读取一行。
- **Dim fields As String() = line.Split(",")** 使用逗号作为分隔符，将行拆分为字符串数组。
- **If fields.Length = 4 Then** 检查该行是否包含四个字段。
- 在 **If** 块内部，代码从 **fields** 数组中提取学生姓名、分数、类别和附加信息。
- 根据类别，创建 **clsTestGrade**、**clsQuizGrade** 或 **clsHWGrade** 的新实例，并将其添加到 **studentGrades** 字典中相应学生的列表中。
- **Using** 块内的循环持续进行，直到处理完 CSV 文件的所有行。
- 读取 CSV 文件后，代码计算并显示每个学生和每个类别的平均分。
- 外层的 **For Each** 循环遍历 **studentGrades** 字典中的每个学生。
- 在循环内部，它使用单独的变量计算每个类别（**Test**、**Quiz** 和 **HW**）的总分和计数。
- 然后计算每个类别的平均分，并处理计数为零的情况。
- 最后，它打印出学生姓名以及计算得到的测试、测验和家庭作业的平均分。
- **Console.ReadLine()** 等待用户按下 *Enter* 键后再退出程序。

总之，此代码从 CSV 文件中读取学生成绩数据，将其组织在字典中，计算不同作业类别的平均分，并显示每个学生的结果：

```vbnet
Imports System.IO
Module Module1
    Sub Main()
        Dim filePath As String = "..\..\students.csv"
        Dim studentGrades As New Dictionary(Of String, List(Of Grade))()
        Using reader As New StreamReader(filePath)
            While Not reader.EndOfStream
                Dim line As String = reader.ReadLine()
                Dim fields As String() = line.Split(",")
                If fields.Length = 4 Then
                    Dim studentName As String = fields(0).Trim()
                    If Not studentGrades.ContainsKey(studentName) Then
                        studentGrades.Add(studentName, New List(Of Grade)())
                    End If
                    Dim score As String = fields(1).Trim()
                    Dim category As String = fields(2).Trim()
                    Select Case category
                        Case "Test"
                            studentGrades(studentName).Add(New TestGrade(score, fields(3).Trim()))
                        Case "Quiz"
                            studentGrades(studentName).Add(New QuizGrade(score, fields(3).Trim()))
                        Case "HW"
                            studentGrades(studentName).Add(New HWGrade(score, fields(3).Trim()))
                    End Select
                End If
            End While
        End Using
        For Each studentName As String In studentGrades.Keys
            Dim testtotal, testcnt, quiztotal, quizcnt, hwtotal, hwcnt As Integer
            Dim grades As List(Of Grade) = studentGrades(studentName)
            For Each eagrade As Grade In grades
                If TypeOf eagrade Is TestGrade Then
                    testtotal += eagrade.Score
                    testcnt += 1
                ElseIf TypeOf eagrade Is QuizGrade Then
                    quiztotal += eagrade.Score
                    quizcnt += 1
                Else
                    hwtotal += eagrade.Score
                    hwcnt += 1
                End If
            Next
            Dim testavg, quizavg, hwavg As Double
            If testcnt > 0 Then
                testavg = testtotal / testcnt
            End If
            If quizcnt > 0 Then
                quizavg = quiztotal / quizcnt
            End If
            If hwcnt > 0 Then
                hwavg = hwtotal / hwcnt
            End If
            Console.WriteLine(studentName & ": Test Avg:" & testavg.ToString("N1") & " Quiz Avg:" & quizavg.ToString("N1") & " HW Avg:" & hwavg.ToString("N1"))
        Next
        Console.ReadLine()
    End Sub
End Module
```

接下来，让我们将此代码放入 IDE 中试一试。

## 编写 Visual Basic.NET 控制台评分工具

1. 启动 **Microsoft Visual Studio**。
2. 选择 **创建新项目**。
3. 选择 **控制台（.NET Framework）**，然后单击 **下一步**。
4. 接受默认的项目名称和解决方案名称。
5. 如果您希望将代码放在其他文件夹中，可以更改位置。
6. 接受默认框架。
7. 单击 **创建项目**。
8. 在主模块中输入上一节中的代码示例。
9. 为前面描述的每个类创建类文件，文件名与类名相同，扩展名为 **vb**。
10. 要运行程序，请单击代码上方工具栏上的绿色三角形。

下一节将介绍 ASP.NET 中的面向对象评分工具。

## 在 ASP.NET 中构建项目

以下是面向对象评分工具的 **ASP.NET VB.NET** 代码。该代码与之前为 **VB.NET** 控制台应用程序提供的代码非常相似。在此示例中，无需更改 ASPX 代码，因为输出通过 `response.write` 方法处理：

```vbnet
Imports System.IO
Public Class ASPNETOOGradingTool
    Inherits System.Web.UI.Page
    Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
        Dim filePath As String = "students.csv"
        Dim studentGrades As New Dictionary(Of String, List(Of clsGrade))()
        Using reader As New StreamReader(filePath)
            While Not reader.EndOfStream
                Dim line As String = reader.ReadLine()
                Dim fields As String() = line.Split(",")
                If fields.Length = 4 Then
                    Dim studentName As String = fields(0).Trim()
                    If Not studentGrades.ContainsKey(studentName) Then
                        studentGrades.Add(studentName, New List(Of Grade)())
                    End If
                    Dim score As String = fields(1).Trim()
                    Dim category As String = fields(2).Trim()
                    Select Case category
                        Case "Test"
                            studentGrades(studentName).Add(New clsTestGrade(score, fields(3).Trim()))
                        Case "Quiz"
                            studentGrades(studentName).Add(New clsQuizGrade(score, fields(3).Trim()))
                        Case "HW"
                            studentGrades(studentName).Add(New clsHWGrade(score, fields(3).Trim()))
                    End Select
                End If
            End While
        End Using
        For Each studentName As String In studentGrades.Keys
            Dim testtotal, testcnt, quiztotal, quizcnt, hwtotal, hwcnt As Integer
            Dim grades As List(Of clsGrade) = studentGrades(studentName)
            For Each egrade As clsGrade In grades
                If TypeOf egrade Is clsTestGrade Then
                    testtotal += egrade.Score
                    testcnt += 1
                ElseIf TypeOf egrade Is clsQuizGrade Then
                    quiztotal += egrade.Score
                    quizcnt += 1
                Else
                    hwtotal += egrade.Score
                    hwcnt += 1
                End If
            Next
            Dim testavg, quizavg, hwavg As Double
            If testcnt > 0 Then
```




of inheritance and polymorphism. The next chapter will look at the Request and Response model used in modern web application architectures.
