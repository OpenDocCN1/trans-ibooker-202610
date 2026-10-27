

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

## Part 3: Object-Oriented Visual Basic

在本书的第三部分，我们将学习构建对象以实现更多代码重用。我们将学习面向对象编程的三大要素 – 封装、继承和多态 – 以及它们在 Visual Basic 中的使用方式。

本部分包含以下章节：

- *[Chapter 12](#)*, *Object-Oriented Programming*
- *[Chapter 13](#)*, *Inheritance*
- *[Chapter 14](#)*, *Polymorphism*
- *[Chapter 15](#)*, *Interfaces*
- *[Chapter 16](#)*, *Project Part III*

## Object-Oriented Programming

**Object-oriented programming (OOP)** 是一种将代码和数据组织到称为对象的可重用结构中的编程范式。在我们之前的代码片段中，我们可以将代码组织到过程或函数中以实现代码重用。对象允许我们重用既可执行又可存储信息的代码。在 OOP 中，对象是类的实例，类充当创建具有相似属性和行为的对象的蓝图或模板。

OOP 的基本概念如下：

- **Classes:** 类是一个蓝图，定义了该类对象的属性（属性）和行为（方法）。它描述了对象的结构和行为，但本身并不代表任何特定实例。
- **Objects:** 对象是类的实例。它由类蓝图创建并具有属性的值。对象可以通过调用其方法相互交互。
- **Encapsulation:** 封装是指将数据（属性）和操作该数据的方法捆绑到称为对象的单一单元中。封装隐藏对象的内部细节，并通过定义良好的接口提供对其功能的访问。
- **Inheritance:** 继承允许类从其他类继承属性和行为。它使类能够形成层次结构，其中子类（派生类）可以继承并扩展超类（基类）的属性和方法。
- **Polymorphism:** 多态意味着对象能够采取不同的形式或行为。在 OOP 的上下文中，多态允许其他类的对象被视为公共超类的对象。多态允许你编写更通用的代码，可与不同类型的对象一起使用。
- **Abstraction:** 抽象涉及创建复杂系统的简化表示的概念。在 OOP 中，抽象允许你专注于对象的本质特征，同时隐藏实现细节。抽象类和接口用于在许多编程语言中定义抽象。

OOP 提供了多个好处，包括代码可重用性、模块化和更易于维护。它允许更好的代码组织，促进代码可读性，并支持大规模软件系统的开发。

支持 OOP 的流行编程语言包括 **Visual Basic (VB)**、Java、C++、Python、C#、Ruby 等。每种语言可能都有自己的语法和用于实现 OOP 概念的特性，但基本原理保持不变。

在本章中，我们将涵盖以下主要主题：

- Classes and fields

