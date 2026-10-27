## 项目 第四部分

在我们的第四个项目中，我们将使用我们所学的关于 HTTP 请求-响应模型以及变量作用域和并发的技能，构建一个服务器端成绩平均工具，该工具将一次从客户端接收一个成绩。此外，它还会发送回迄今为止收到的所有成绩的平均值。

在本章中，我们将涵盖以下主要主题：

- 在 Classic ASP 中开发服务器端成绩平均工具
- 在 ASP.NET 中开发服务器端成绩平均工具

### 技术要求

请完成使用第 1 章中的 VB 家族成员安装代码的步骤。本章的所有示例代码都可在以下 GitHub 仓库获得：https://github.com/PacktPublishing/Learn-Visual-Basics-Quick-Start-Guide-/tree/main/ProjectPartIV。

### 在 Classic ASP 中构建项目

以下是接受客户端 Web 浏览器中的成绩并以平均成绩作为响应的 Classic ASP 程序。该程序将在 Web 浏览器中接受成绩并将它们发送到服务器，服务器将计算新的平均值并返回它：

```
<FORM>
<INPUT name="grade"/>
<INPUT type="submit" value="Add Grade"/>
</FORM>
<%
if Request("grade").Count > 0 Then
    Session("Total") = Session("Total") + CInt(Request("grade"))
    Session("Cnt") = Session("Cnt") + 1
    response.write("Grade Average: ")
    response.write(Session("Total") / Session("Cnt"))
End If
%>
```

代码的第一部分定义了一个 HTML 表单，允许用户输入成绩值。它包含一个名为 `grade` 的文本输入字段和一个标记为 Add Grade 的提交按钮。

代码的下一部分使用 Classic ASP 处理表单提交并计算平均成绩。

`if` 语句使用 `Request("grade").Count` 条件检查是否已提交名为 `grade` 的数据。如果存在数据，则表示表单已被提交。

在 `if` 块内部，我们有以下内容：

- **Session("Total")** 是一个会话变量，用于跟踪成绩的总和。它将提交的成绩值（`Request("grade")`）添加到当前值。
- **Session("Cnt")** 是另一个会话变量，用于跟踪成绩的数量。它递增 1 以计算输入的成绩数量。
- **response.write** 用于将文本输出到网页。在这种情况下，它显示 **Grade Average**，后跟计算出的平均值，即总和除以计数。

总之，此代码创建了一个简单的 Web 表单，允许用户输入成绩。当表单提交时，Classic ASP 代码通过使用会话变量维护成绩的运行总和和输入的成绩计数来计算并显示平均成绩。

#### 使用记事本编码 Classic ASP 评分工具

按照以下步骤编码和执行在此项目中开发的计算器的 VBScript：

1. 启动记事本。
2. 输入上一节代码示例中的所有代码。
3. 将文件保存为 **ClassicASPSSGrades.asp** 到根 Web 目录（这通常是 `c:\inetpub\wwwroot`）。
4. 打开 Web 浏览器。
5. 输入 `http://127.0.0.1/ClassicASPSSGrades.asp`。

下一节将处理 **ASP.NET** 中的服务器端成绩平均工具示例。

### 在 ASP.NET 中构建项目

下面是服务器端成绩平均工具的 **ASP.NET** 和 **VB.NET** 代码。第一段代码是 ASPX 代码：

```aspx
<%@ Page Language="vb" AutoEventWireup="false" CodeBehind="ASPNETSSGrades.aspx.vb" Inherits="WebApplication8.WebForm1" %>
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <title></title>
</head>
<body>
    <form id="form1" runat="server">
        <div>
            <asp:TextBox ID="tbGrade" runat="server"></asp:TextBox>
            <asp:Button ID="Button1" runat="server" Text="Add Grade" />
        </div>
        <asp:Label ID="lblAverage" runat="server"></asp:Label>
    </form>
</body>
</html>
```

前述代码块的第一行是页面顶部的指令，为 ASP.NET 运行时提供指令。它指定页面的语言（Visual Basic），关闭自动事件绑定（我们稍后会讨论），并指向代码隐藏文件（`ASP.NETSSGrades.aspx.vb`）。

代码的下一部分包含网页的 HTML 标记。

页面以通常的 HTML 结构开始，并包含一个 ID 为 `form1` 且在服务器上运行的 `<form>` 元素。

在表单内部，有一个包含两个 ASP.NET 控件的 `<div>` 元素：

- 一个 ID 为 `tbGrade` 的 `<asp:TextBox>` 控件。这是用户可以输入成绩的文本输入。
- 一个 ID 为 `Button1` 且文本为 Add Grade 的 `<asp:Button>` 控件。这是用户可以单击以执行操作（例如添加成绩）的按钮。

在 `<div>` 元素下方，有一个 ID 为 `lblAverage` 的 `<asp:Label>` 控件。这里将显示计算出的平均成绩。

这些 HTML 元素上的 `runat="server"` 属性表示它们是 ASP.NET 服务器控件，这意味着它们可以通过服务器端代码进行操作和交互。

以下是代码隐藏文件 `ASP.NETSSGrades.aspx.vb` 的代码：

```vb
Public Class WebForm1
    Inherits System.Web.UI.Page
    Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
    End Sub
    Protected Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click
        Session("Total") += CInt(tbGrade.Text)
        Session("Cnt") += 1
        lblAverage.Text = "Grade Average: " & Session("Total") / Session("Cnt")
    End Sub
End Class
```

前述代码的开头声明了一个名为 `WebForm1` 的类，它继承自 `System.Web.UI.Page` 类。这表明 `WebForm1` 是一个 ASP.NET 页面，可以利用基类提供的属性和方法。

以 `Protected Sub Page_Load` 开始的代码部分是页面 Load 事件的事件处理程序。Load 事件在页面加载时触发。该事件通常用于初始化页面相关的数据或控件。在这种情况下，



该事件处理程序为空，因此页面加载时不包含任何要执行的特定代码。

代码中从 `Protected Sub Button1_Click` 开始的部分是 ID 为 `Button1` 的控件的 `click` 事件的事件处理程序（如 `Handles Button1.Click` 部分所指定）。当用户单击该按钮时，会触发此事件。事件处理程序中的代码执行以下操作：

- 它使用 `CInt` 函数将 ID 为 `tbGrade` 的控件中输入的文字转换为整数，并将其加到存储在 `Total` 会话变量中的值上。
- 它将存储在 `Cnt` 会话变量中的值递增 `1`。
- 它通过将存储在 `Total` 会话变量中的总和除以存储在 `Cnt` 会话变量中的计数来计算平均成绩。
- 然后，计算出的平均值被赋给 ID 为 `lblAverage` 的控件的 `Text` 属性，该控件会在网页上显示计算出的平均值。

该代码使用会话变量来跨请求和交互存储并维护成绩的累计总和与计数。当用户单击 `Button1` 按钮时，计算出的平均值会动态显示在网页上。

## 使用 Visual Studio 编写 ASP.NET 服务器端成绩平均值计算器

按照以下步骤编写并执行一个显示 `Hello World` 的 ASP.NET 网页：

- 1. 启动 **Microsoft Visual Studio 2019**。
- 2. 选择 **创建新项目**。
- 3. 选择 **ASP.NET Web 应用程序**，然后单击 **下一步**。
- 4. 接受 **默认项目名称** 和 **解决方案名称**。
- 5. 如果你希望代码位于其他文件夹，请更改位置。
- 6. 接受默认的 **框架** 选项。
- 7. 单击 **创建项目**。
- 8. 选择 **Web 窗体**。
- 9. 设计器加载后，导航到 **解决方案资源管理器**，然后右键单击该解决方案。
- 10. 从弹出菜单中选择 **添加项**。
- 11. 选择 **Visual Basic Web 窗体**。
- 12. 选择 **源代码**。
- 13. 将 ASPX 代码粘贴到 **FORM** 标记内。
- 14. 右键单击 ASPX 源代码，然后从弹出菜单中选择 **查看代码**。
- 15. 输入上一节中位于 `Button1_click` 过程中的 VB.NET 代码示例。
- 16. 若要运行程序，请单击代码上方工具栏上的绿色三角形。

试着深入理解 Classic ASP 和 ASP.NET 版本代码之间的相似之处。如你所见，ASP.NET 将 HTML 与 VB 分开，以使维护更容易。随着项目规模的增长，这一点变得非常重要。

## 总结

在本章中，我们运用了前几章学到的关于 HTTP 请求-响应模型、变量作用域和并发的技能，为 **Classic ASP** 和 **ASP.NET** 构建了一个服务器端学生成绩平均值计算工具。在下一章中，我们将总结已涵盖的内容，思考你可以学习的其他 VB 主题，以及你可以开发的项目，以展示你在本书中学到的技能，从而结束本书。
