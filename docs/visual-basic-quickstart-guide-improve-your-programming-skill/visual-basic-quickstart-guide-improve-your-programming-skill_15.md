## 结论

我们在本书中涵盖了许多不同的 **Visual Basic (VB)** 主题。我想通过总结所学内容、建议你可以围绕 VB 学习的其他相关领域，并给出一些可以做的项目建议来结束本书，以便向潜在雇主展示你的新技能。

在本章中，我们将介绍以下主要主题：

- 总结全部内容
- 进一步阅读主题
- 建议项目

### 总结全部内容

在整本书中，我们探讨了 VB 在许多不同环境中的使用，包括使用 Classic VB 和 VB.NET 控制台进行 Windows 桌面开发。我们还研究了在 **Classic ASP** 和 **ASP.NET** 中将 VB 用作服务器端编程语言。**Visual Basic for Applications (VBA)** 也被用来将 VB 嵌入 Microsoft Office 产品中，我们还探讨了在脚本中使用 **VBScript** 的方法。

在本节中，我们将简要回顾本书中涵盖的主要主题。

在 VB 中，变量声明、类型、作用域和赋值是处理数据和管理程序逻辑的基本概念。以下是这些概念的概述。

### 变量声明

在 VB 中，你使用 `Dim` 关键字（即 **Dimension** 的缩写）声明变量。变量声明会为存储数据保留内存空间，并将其与名称关联。声明变量的基本语法如下：

```vb
Dim variableName As DataType
```

### 数据类型

VB 支持多种数据类型来表示不同种类的值。一些常见的数据类型包括：

- **Integer**：表示整数。
- **Double** 或 **Single**：表示浮点数。
- **String**：表示文本。
- **Boolean**：表示真或假值。
- **Date**：表示日期和时间。
- **Object**：表示类的实例。

### 作用域

变量的作用域指的是变量在代码中可访问和使用的位置。VB 中有不同级别的作用域：

- **过程级作用域**：只有过程（函数或子例程）内声明的变量可访问。
- **模块级作用域**：在模块级别（任何过程之外）声明的变量可被该模块内的所有过程访问。
- **全局作用域**：在模块中使用 **Public** 关键字声明的变量可从项目中的任何模块或窗体访问。

### 赋值

赋值涉及将值存储到变量中。你可以使用赋值运算符 (=) 为变量赋值。示例如下：

```vb
age = 25
name = "John"
```

### 迭代

迭代（也称为循环）是一种基本的编程概念，它允许你重复执行一段代码。在 VB 中，你可以使用各种结构来实现迭代。以下是一些标准的迭代方法：

- **For...Next 循环**：当你预先知道迭代次数时，可使用 **For...Next** 循环。它遍历由起始值、结束值和可选步长值定义的值范围：

  ```vb
  For counter As Integer = startValue To endValue Step stepValue
  Next
  ```

- **While 循环**：**While** 循环只要指定条件保持为真，就会重复执行一个代码块：

  ```vb
  While condition
      ' Code to be executed in each iteration
  End While
  ```

- **Do...Loop 循环**：**Do...Loop** 循环与 **While** 循环类似，但条件在末尾检查。这意味着循环的代码块始终至少执行一次：

  ```vb
  Do
      ' Code to be executed in each iteration
  Loop While condition
  ```

- **For Each 循环**：**For Each** 循环用于遍历集合中的元素，例如数组或集合类。

- **Sub 过程**：Sub 过程执行任务且不返回值：

  ```vb
  ' Code to perform a specific task
  End Sub
  ```

- **Function 过程**：Function 过程（或函数）与 Sub 类似，但会返回值。当你需要计算或推导出结果时，可以使用它们：

  ```vb
  Function FunctionName(parameters) As DataType
      ' Code to calculate and return a value
  End Function
  ```

### 集合

在 VB 中，数据结构允许你存储和管理相同或不同数据类型的多个元素。集合为处理数据组提供了便捷的方式，并提供了各种添加、移除和访问元素的方法。VB 提供了多个内置集合类，可用于完成不同任务。由于集合类型因 VB 家族成员而异，我们在此不做总结，但你应该重新查看[第 10 章](#)进行回顾。

### 文件处理

VB 中的文件处理涉及读取和写入计算机文件系统上的文件。每个 VB 家族成员都提供了各种用于处理文件的类和方法，使你可以创建、打开、读取和写入文件。你可以重新查看[第 9 章](#)进行回顾。

### 对象封装

对象封装（也称为封装或信息隐藏）是**面向对象编程（OOP）**的一项基本原则，它强调将数据（属性）和操作这些数据的方法（函数或过程）捆绑到一个称为对象的单元中。封装有助于控制对对象内部状态的访问，并促进模块化和抽象化。请重新查看[第 12 章](#)，了解如何在不同 VB 家族成员中封装变量。

### 继承

继承是 OOP 中的一个基本概念，允许你基于现有类（称为基类或父类）创建新类（称为子类或派生类）。继承使你可以重用和扩展现有类的功能，促进代码重用和层次化。在 VB 中，你可以使用 **Inherits** 关键字或接口继承来实现继承。




接口。请回顾*第 13 章*至*第 15 章*，了解不同 VB 家族成员中继承是如何实现的。

## 多态

多态是面向对象编程（OOP）中的一个核心概念，它允许将不同类的对象视为标准基类的对象。它使你能够编写更通用、更灵活的代码，以统一的方式处理各种对象。多态通过方法重写和方法重载来实现。

在 VB 中，多态的实现主要依靠方法重写和接口实现。请回顾*第 14 章*，了解如何在不同 VB 家族成员中使用多态。

## Web 应用 HTTP 模型与并发

在 Web 应用中，客户端 Web 浏览器与服务器之间的通信使用**超文本传输协议（HTTP）**。该模型涉及客户端向服务器发送请求，服务器返回数据作为响应。请回顾*第 17 章*和*第 18 章*，了解如何在经典 ASP 和 ASP.NET 应用中使用 VB。

接下来，让我们看看本书范围之外的一些可能有趣的 VB 主题。

## 延伸阅读主题

在本书中，我们介绍了频繁使用的 VB 核心编程主题，但你可能需要修改使用其他技术的代码。我建议你熟悉以下我们未涉及的主题。

### VB6 窗体开发

**VB6** 曾广泛用于开发 Windows 应用程序，尤其是使用窗体的**图形用户界面（GUI）**应用程序。VB6 窗体开发涉及通过可视化设计窗体来创建用户界面，并添加代码来实现功能。以下是 VB6 窗体开发过程的一般概述：

- **创建新项目：**打开 VB6 IDE 并创建一个新项目。你可以从各种项目模板中进行选择，包括 Standard EXE，来创建独立应用程序。

### COM 对象创建

在 VB.NET 中创建组件对象模型（COM）对象涉及创建符合 COM 规范的类，使它们能够被支持 COM 的其他语言和应用程序（如 VB6 和 VBA）使用。COM 对象可用于多种场景，从扩展现有软件的功能到创建可重用组件。以下是在 VB.NET 中创建 COM 对象的步骤概述：

- **创建新的类库项目：**打开 Visual Studio，并使用 VB.NET 创建一个新的类库项目。

- **定义 COM 类：**定义希望作为 COM 对象公开的类。此类应使用指示其为 COM 类的特性进行标记。最基本的特性是 [ClassInterface]、[Guid] 和 [ComVisible]：

```
Imports System.Runtime.InteropServices
<ClassInterface(ClassInterfaceType.AutoDual)>
<Guid("YOUR-GUID-HERE")>
<ComVisible(True)>
Public Class MyCOMClass
' Define properties, methods, and events here.
End Class
```

将“YOUR-GUID-HERE”替换为你类的实际 GUID。你可以使用 Visual Studio 的“Create GUID”工具生成 GUID。

- **定义属性、方法和事件：**在 COM 类内部，定义希望向 COM 客户端公开的属性、方法和事件。

- **生成项目：**生成项目以创建类库程序集。

- **注册程序集：**要使用 COM 对象，需要使用 Windows 命令行工具 Regasm.exe 注册程序集。以管理员身份打开命令提示符，然后运行以下命令：

```
Regasm.exe /codebase YourAssembly.dll
```

将 YourAssembly.dll 替换为程序集的实际名称。

- **使用 COM 对象：**注册后，COM 对象即可被其他支持 COM 的应用程序使用。你可以在 VB6 和 C# 等应用程序，甚至 VBScript 等脚本语言中创建 COM 类的实例。以下是在 VBScript 中使用它的示例：

```
Dim obj
Set obj = CreateObject("YourNamespace.YourCOMClass")
obj.YourMethod()
Set obj = Nothing
```

接下来，让我们看看你可以在 VB 中着手的一些项目，以向潜在雇主展示你的新技能。

## 建议项目

我建议你通过完成一个更大的项目来继续学习，用这个项目向潜在雇主展示你的新技能。对你成功最重要的是，你能被这个创意项目所激励。尽量选择一个对你有用的想法。这里有几个想法：

- 尝试开发一个处理文件的命令行工具。文件处理对于娱乐和业务来说都是非常重要的自动化任务。例如，成千上万的志愿者修改旧游戏主机上的游戏以使用更新的数据。一个例子是每年更新阵容的体育游戏。你可以尝试开发一个程序来自动化这一过程。一定要循序渐进，不要试图一次做太多。最好是该过程也可以手动更新文件来完成，而你只是编写一个工具，用代码完成这些手动步骤。

- 尝试开发一个比本书中简单示例更复杂的 Web 应用。有很多优秀的 HTML 书籍可以帮助你增长知识。它可以是一个交互式游戏（如棋类游戏），也可以是一个帮助你管理生活的实用工具。

- 尝试开发一个在 Microsoft Windows 启动时可以运行的启动程序。如果你每天都会在机器上做一些操作，比如备份或清理文件，看看你能否通过代码完成这些操作。

这些只是激发你创造力的一些想法。重要的是，你要把新获得的思维能力运用到你关心的项目中。

## 小结

本章结束了我们对 VB 及微软多年来创建的不同家族成员的探讨。VB 对今天人们使用计算机和编写程序的方式产生了巨大影响。我要感谢你努力学习本书所呈现的内容。在提升技术技能方面继续加油！

## 索引

*由于本电子书版本没有固定页码，以下页码基于本书印刷版，以超链接形式提供，仅供参考。*

### 符号

| 主题 | 页码 |
|---|---|
| 7-Zip | 8 |
| &nbsp;&nbsp;&nbsp;&nbsp;URL | 8 |
| & 运算符 | 75 |

### A

| 主题 | 页码 |
|---|---|
| Abs 函数 | 53 |
| 抽象类 | 141, 142 |
| 抽象 | 125, 145 |
| 访问修饰符 | 127, 128 |
| Active Server Pages (ASP) | 177 |
| &nbsp;&nbsp;&nbsp;&nbsp;应用程序作用域 | 179 |
| &nbsp;&nbsp;&nbsp;&nbsp;Cookie | 177, 181, 182 |
| &nbsp;&nbsp;&nbsp;&nbsp;查询字符串/隐藏表单域 | 178 |
| &nbsp;&nbsp;&nbsp;&nbsp;请求作用域 | 181 |
| &nbsp;&nbsp;&nbsp;&nbsp;会话对象 | 177 |
| &nbsp;&nbsp;&nbsp;&nbsp;会话作用域 | 178 |
| ActiveX 对象 | 127 |
| &nbsp;&nbsp;&nbsp;&nbsp;应用程序作用域 | 179 |
| &nbsp;&nbsp;&nbsp;&nbsp;在 ASP 中使用应用程序作用域 |  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;应用程序关闭 | 180 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;访问数据 | 180 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据初始化 | 179 |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;共享更新 | 180 |
| 参数 | 53 |
| 数组 | 29, 34, 95 |
| &nbsp;&nbsp;&nbsp;&nbsp;示例 | 34 |
| &nbsp;&nbsp;&nbsp;&nbsp;在早期 VB 家族成员中的处理 | 98 |
| &nbsp;&nbsp;&nbsp;&nbsp;与列表的比较 | 98, 99 |
| ASP 编译器 | 13 |
| ASP.NET | 14, 24, 85, 189 |
| &nbsp;&nbsp;&nbsp;&nbsp;替代安装方式 | 15 |
| &nbsp;&nbsp;&nbsp;&nbsp;Hello, World! | 14 |
| &nbsp;&nbsp;&nbsp;&nbsp;使用 Visual Studio 编写 Hello, World! | 16 |
| &nbsp;&nbsp;&nbsp;&nbsp;使用输入和输出进行编程 | 24 |
| &nbsp;&nbsp;&nbsp;&nbsp;安装 | 15 |
| &nbsp;&nbsp;&nbsp;&nbsp;使用 ASP.NET 编程 | 14 |
| &nbsp;&nbsp;&nbsp;&nbsp;构建项目 | 68–70, 120, 160–162, 185–187 |
| &nbsp;&nbsp;&nbsp;&nbsp;标签 | 24 |
| ASP.NET 计算器 |  |
| &nbsp;&nbsp;&nbsp;&nbsp;使用 Visual Studio 编码 | 71, 122, 162 |
| Linux 上的 ASP.NET |  |
| &nbsp;&nbsp;&nbsp;&nbsp;安装链接 | 15 |
| ASP.NET Request 对象 | 169 |
| &nbsp;&nbsp;&nbsp;&nbsp;Cookies 集合 | 170 |
| &nbsp;&nbsp;&nbsp;&nbsp;表单集合 | 169 |
| &nbsp;&nbsp;&nbsp;&nbsp;标头集合 | 170 |
| &nbsp;&nbsp;&nbsp;&nbsp;HTTP 方法 | 170 |
| &nbsp;&nbsp;&nbsp;&nbsp;InputStream | 170 |
| &nbsp;&nbsp;&nbsp;&nbsp;路径 | 170 |
| &nbsp;&nbsp;&nbsp;&nbsp;查询字符串 | 170 |
| &nbsp;&nbsp;&nbsp;&nbsp;使用 | 170, 171 |
| ASP.NET Response 对象 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;AddHeader 方法 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;BinaryWrite 方法 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;Clear 方法 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;ContentType 属性 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;Cookies 集合 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;示例 | 173, 174 |
| &nbsp;&nbsp;&nbsp;&nbsp;输出缓存 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;Redirect 方法 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;Response.Cache 属性 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;WriteFile 方法 | 173 |
| &nbsp;&nbsp;&nbsp;&nbsp;Write 方法 | 173 |
| ASP.NET 服务端成绩平均计算器 |  |
| &nbsp;&nbsp;&nbsp;&nbsp;使用 Visual Studio 编码 | 187 |
| ASP.NET VB.NET | 160 |
| 赋值 | 190 |
| 关联数组 | 95, 100 |

### B

| 主题 | 页码 |
|---|---|
| B4X | 9 |
| &nbsp;&nbsp;&nbsp;&nbsp;URL | 9 |
| 袋/多重集 | 96 |
| 基类 | 193 |
| BASIC | 45 |
| Basic for Android (B4A) | 9 |
| Basic for Arduino (B4R) | 9 |
| Basic for iOS (B4i) | 9 |
| Basic for Java (B4J) | 9 |
| 变量命名最佳实践 | 33 |
| 二进制数据 |  |
| &nbsp;&nbsp;&nbsp;&nbsp;从文件读取 | 92, 93 |
| &nbsp;&nbsp;&nbsp;&nbsp;在 VB6 中读取 | 93 |
| &nbsp;&nbsp;&nbsp;&nbsp;在 VBA 中读取 | 93 |
| &nbsp;&nbsp;&nbsp;&nbsp;在 VB6 中写入 | 92 |
| &nbsp;&nbsp;&nbsp;&nbsp;在 VBA 中写入 | 92 |
| &nbsp;&nbsp;&nbsp;&nbsp;写入文件 | 91, 92 |




布尔数据类型 29, 30
ByRef 参数 56
ByVal 参数 56
C
C# 142
Case 语句 41
字符数据类型 29, 30
在 OOP 中使用接口的特性与优势
  抽象 145
  契约义务 146
  接口隔离原则（ISP）146
  多重继承 146
  多态性 145
  关注点分离 145
Cint 函数 53
类 125, 126
  声明 126
  实例化 129, 130
类数据类型 30
经典 ASP（Active Server Pages）12, 22, 189
  Hello, World! 12
  使用记事本编写 Hello, World! 代码 13, 14
  输入与输出，使用编程方式 22
  在 Windows 上安装 12
  编程 12
  构建项目 66, 67, 118, 119, 183, 184
经典 ASP（Active Server Pages）的替代安装方式 13
  ASP 编译器 13
  CodeWall 13
  SmartAssembly 13

经典 ASP 计算器
使用记事本编写代码 68

经典 ASP 评分工具
使用记事本编写代码 120, 184

经典 ASP Request 对象 168, 169
cookies 集合 168
form 集合 168
HTTP 方法 168
路径 168
查询字符串 168, 169
ServerVariables 集合 168

经典 ASP Response 对象 172, 173
AddHeader 方法 172
BinaryWrite 方法 172
Clear 方法 172
ContentType 属性 172
cookies 集合 172
方法和属性 172
Redirect 方法 172
Write 方法 172
WriteText 方法 172

代码
在函数中模块化 53, 54
在过程中模块化 55

CodeWall 13

集合 95, 192
数组 95
包/多重集 96
字典 95
链表 95
列表 95
队列 95
集合 95
栈 95
元组 96
逗号分隔值（CSV）文件 103
编译时（静态）多态性 139
组件对象模型（COM）对象
  程序集，注册 196
  创建类库项目 195
  定义 COM 类 196
  使用 COM 对象 196
  创建 195
  定义事件 196
  定义方法 196
  构建项目 196
  定义属性 196
并发 193
条件语句 191
Console 类 17
Continue 语句 50, 51
契约义务 146
cookies 181, 182
CorelDRAW 10
CScript 21
CSS 24
自定义函数 54, 55
自定义过程 55

## D

数据格式化 75

数据类型
  数组
  布尔
  字符
  类
  双精度
  枚举
  浮点
  整数
  指针
  选择
  字符串
  结构体
  无类型

数据类型，VB
  布尔
  字符
  日期
  十进制
  双精度
  整数
  长整数
  对象
  单精度
  字符串

日期和时间数据
  组件提取
  日期和时间数学运算
  日期和时间值，格式化
  修改

数值格式化 82
创建值 80, 81

日期数据类型 30
Date 函数 53
十进制数据类型 30
决策分支 39, 191
Case 语句 41
GoTo 语句 44, 45
If 语句 39, 40
使用条件语句实现 191, 192
三元运算符 42
VB.NET AndAlso 运算符 43, 44
VB.NET OrElse 运算符 42
派生类 193
字典 95, 100
存储数据 100-102
在 VB.NET 中使用 100
Dimension 189
目录
在旧版 VB 系列成员中管理 88
在 VB.NET 中管理 86, 87
使用 85
Do 循环 49, 50
双精度数据类型 29, 30

## E

ElseIf 语句 40
Else 语句 40
封装 4, 125, 193
枚举类型 30
Excel
在其中编写 VBA 计算器
在其中编写 VBA 评分工具
从其中编写 VBA Hello, World! 代码
异常处理
Exit 语句
显式变量声明
与隐式变量声明对比

## F

字段
文件处理
文件
  读取二进制数据
  写入二进制数据
  在旧版 VB 系列成员中管理
  在 VB.NET 中管理
  读取文本
  写入文本
  使用
文件系统对象（FSO）
先进先出（FIFO）
浮点数据类型
For 循环
Format 函数
FORTRAN
函数过程
函数
  在函数中模块化代码
函数，VB
  Abs
  Cint
  Date
  Format
  InStr
  Int
  Lcase
  Left
  Len
  Mid
  MsgBox
  Now
  Replace
  Right
  Rnd
  Time
  Trim
  Ucase

## G

全局级作用域 32, 190
GoTo 语句 44, 45
图形用户界面（GUI）194

## H

哈希映射
Hello, World!
  在 ASP.NET 中 14, 16
  在经典 ASP 中 12, 13
  在 VBA 中 10
  在 VBScript 中 11
  在 Visual Basic 经典版中 7, 9
  在 Visual Basic.NET Windows 窗体中 5, 6
HTML 22-24

Lcase 函数 54
Left 函数 54
Len 函数 54
LibreOffice 10
链表 95
列表 95, 99
  演示用法 100
  使用 99
  与数组对比 98, 99
长数据类型 30
循环 190

## M

映射 95
数学运算，VB
  加法 78
  除法 78
  模除 79
  乘法 78
  减法 78
方法重载 128
方法 127
  声明 128, 129
Microsoft Internet Information Services（IIS）12
Microsoft SQL Server 12
Mid 函数 54, 76
模型-视图-控制器（MVC）14
模块级作用域 32, 190
Mono 6
  下载链接 6
MsgBox 函数 54

多维数组 34
使用 97, 98
多重继承 146
MySQL 12

## N

.NET 框架 195
.NET 软件开发工具包（SDK）6
下载链接 6

记事本
使用记事本编写经典 ASP 计算器代码 68
使用记事本编写经典 ASP 评分工具代码 120, 184
用于编写经典 ASP Hello, World! 代码 13, 14
用于编写 Hello, World! VBScript 代码 11
使用记事本编写 VBScript 计算器代码 66
使用记事本编写 VBScript 评分工具代码 118

数值数据
数学运算 78
修改 78
舍入 79
截断 79, 80
类型转换 80

## O

对象 125, 193
对象数据类型 30
对象封装 193
面向对象编程（OOP）4, 125, 133, 139, 145, 193
抽象 125
优势 4
好处 126
类 125
封装 125
继承 125
对象 125
多态性 125
原则 4
  支持的编程语言 126
一维数组
  使用 96, 97
一对一"有一个"关系 130
Optional 参数 56
Option Explicit 语句 31
Oracle 12
Overridable 关键字 141

## P

ParamArray 参数 56
参数 56
  函数中的参数 56
  过程中的参数 56
参数，VB
  ByRef 56
  ByVal 56
  Optional 56
  ParamArray 56
父类 193
PerlScript 12
指针数据类型 29
多态性 4, 125, 139, 145, 193
  编译时（静态）多态性 139
  在 Visual Basic（VB）中演示 140, 141
  运行时（动态）多态性 139
后置条件循环 49
过程级作用域 32, 190
过程模块化 192
过程 53, 192
  在过程中模块化代码 55
项目
  在 ASP.NET 中构建 68-70, 120, 185-187
  在经典 ASP 中构建 66, 67, 118, 119, 183, 184
  在 VB6 中构建 61, 62, 107-111
  在 VBA 中构建 63, 112-114
  在 VB.NET 控制台中构建 59-61, 104, 105
  在 VBScript 中构建 64, 115-117
属性 127

## Q

队列 95

## R

RAD Basic 9
  URL 9
Replace 函数 54
请求作用域 181
Return 语句 57
Right 函数 54
Rnd 函数 54
Round 函数 79
运行时（动态）多态性 139

## S

变量作用域 190
  赋值 190
  全局作用域 190
  迭代 190, 191




- 模块级作用域 [190]
- 过程级作用域 [190]
- 密封类 [142, 143]
- Select Case 语句 [192]
- 哨兵 [50]
- 关注点分离 [145]
- 会话作用域 [178]
- 会话作用域（ASP 中）
  - 访问数据 [179]
  - 存储数据 [178]
  - 会话结束 [179]
  - 会话开始 [178]
- 集合 [95]
- 银级兼容性 [9]
- 单精度数据类型 [30]
- SmartAssembly [13]
- SQL Server [14]
- 堆栈 [95]
- 标准接口 [148, 149]
- StdIn 属性 [22]
- StdOut 属性 [21]
- String.Concat 方法 [76]
- 字符串数据
  - 大小写转换 [77]
  - 连接 [75, 76]
  - 修改 [75]
  - 子字符串提取 [76]
  - 子字符串替换 [76]
- 字符串数据类型 [29, 30]
- 结构体数据类型 [30]

子类 193

子过程 128, 192

子过程 192

Substring 方法 76

## T

三元运算符 42

文本
从文件读取 90, 91
在 VB6 中读取 91
在 VBA 中读取 91
在 VB6 中写入 90
在 VBA 中写入 90
写入文件 89, 90

Time 函数 54

Trim 函数 54

截断函数 79, 80

元组 96

TwinBasic 9

URL 9

## U

Ucase 函数 54

用户界面（UI）框架 4

## V

变量声明 29, 189

变量命名
最佳实践 33

变量 29

变量作用域 32
全局级作用域 32
模块级作用域 32
过程级作用域 [32]
VB6 与 VB.NET
数据类型差异 [36]
VB6 窗体开发 [194]
构建应用程序 [194]
部署应用程序 [194]
分发应用程序 [194]
添加控件 [194]
调试 [194]
事件处理 [194]
设计窗体 [194]
创建项目 [194]
VB 6 评分工具
编码 [111]
VB6Installer
参考链接 [9]
VBA 与 VB6
数据类型差异 [35, 36]
VBA 计算器
在 Excel 中编码 [64]
VBA 评分工具
在 Excel 中编码 [115]
VB.NET [85, 100-102]
管理目录 [86, 87]
管理文件 [85, 86]
VB.NET AndAlso 运算符 [43, 44]
VB.NET 控制台
构建项目 [59-61, 104, 105, 151-159]
VB.NET 控制台评分工具
编码 [107]
VB.NET 继承 [133, 135]
VB.NET OrElse 运算符 [42]
VB.NET WinForms [195]
构建应用程序 [195]
部署应用程序 [195]
分发应用程序 [195]
添加控件 [195]
数据绑定 [195]
调试 [195]
事件处理 [195]
设计窗体 [195]
创建项目 [195]
VBScript [21, 88, 100, 101, 189]
Hello, World! [11]
使用记事本编写 Hello, World! [11]
输入与输出编程 [21]
安装 [11]
编程 [11]
在 [64, 115–117] 中构建项目
VBScript 与 VB6
数据类型差异 [35]
VBScript 计算器
使用记事本编码 [66]
VBScript 评分工具
使用记事本编码 [118]
虚拟机（VM）[8]
Visual Basic 6 计算器
编码 [62, 63]
Visual Basic Classic (VB6) [7, 19, 88, 100, 101]
读取二进制数据 [93]
写入二进制数据 [92]
下载链接 [8]
Hello, World! 程序编码 [7], [9]
Hello, World! 程序运行 [9]
输入与输出编程 [19]
安装 [8]
编程 [7]
在 [61], [62], [107–111] 中构建项目
读取文本 [91]
写入文本 [90]

Visual Basic Classic (VB6) 的替代安装方式
B4X [9]
RAD Basic [9]
TwinBasic [9]
VB6Installer [9]
WINE [9]

Visual Basic for Applications (VBA) [20], [88], [100], [101], [189]
读取二进制数据 [93]
写入二进制数据 [92]
Hello, World! [10]
从 Excel 编写 Hello, World! [10]
输入与输出编程 [20]
安装 [10]
编程 [9], [10]
在 [63], [112–114] 中构建项目
读取文本 [91]
写入文本 [90]

Visual Basic.NET
输入与输出编程 [17]
在 Windows 机器上安装 [5]

Visual Basic.NET 的替代安装方式
Mono 6
.NET 软件开发工具包（SDK）6

Visual Basic.NET 控制台计算器
编码 61

Visual Basic.NET 控制台评分工具
编码 160

Visual Basic.NET Windows 窗体 4
特性 4
Hello, World! 编码 5, 6
Hello, World! 示例 5
Hello, World! 执行 6
编程 4, 5

Visual Basic (VB) 140
演示多态 140, 141

Visual Studio
ASP.NET 计算器编码 71, 122, 162
ASP.NET 服务器端成绩平均器编码 187
安装链接 5
用于编写 ASP.NET Hello, World! 16

Visual Studio 旧版本
下载链接 15

void 数据类型 30

## W

Web 应用 HTTP 模型 193

While 循环 48, 49

Windows
在 Windows 上安装经典 Active Server Pages (ASP) 12

Windows Communication Foundation (WCF) 14

Windows Management Instrumentation (WMI) 11

Windows Script Host (WSH) [21, 88, 101]

Windows Workflow Foundation (WF) [14]

WINE [9]

WordPerfect [10]

WScript [21]

## .NET MAUI 跨平台应用程序开发

利用一流的跨平台 UI 框架在多个平台上构建原生应用。

ROGER YE

<packt>

第 1 版

## .NET MAUI 跨平台应用程序开发

Roger Ye

ISBN: 978-1-80056-922-5

- 探索 .NET 6 中可用于移动和桌面应用开发的最新特性
- 了解如何使用 .NET MAUI 和 Blazor 构建跨平台应用
- 使用 .NET MAUI Essentials 实现设备特定功能
- 集成第三方库并添加你自己的设备特定功能
- 了解如何使用 xUnit.net 进行 .NET 类单元测试，以及使用 bUnit 进行 Razor 组件单元测试
- 在移动端和桌面的不同应用商店中部署应用

## 面向 C# 开发者的 .NET MAUI

构建跨平台移动和桌面应用程序

JESSE LIBERTY | RODRIGO JUAREZ

序言作者：Maddy Montaquila (Leger)，微软 .NET MAUI 高级产品经理

第 1 版

<packt>

## 面向 C# 开发者的 .NET MAUI

Jesse Liberty, Rodrigo Juarez

ISBN: 978-1-83763-169-8

- 探索使用 Visual Studio 创建 .NET MAUI 应用的基础知识
- 理解 XAML 是构建用户界面的关键工具
- 使用布局和控件获取并显示数据
- 了解 MVVM 模式以创建健壮的应用
- 掌握存储和检索持久数据的技能
- 使用单元测试确保应用稳定可靠

## Packt 正在寻找像你一样的作者

如果你有兴趣成为 Packt 的作者，请访问 authors.packtpub.com 并立即申请。我们已与成千上万的开发者和技术专业人士（就像你一样）合作，帮助他们与全球技术社区分享见解。你可以进行一般申请，申请我们正在招募作者的具体热门主题，或提交你自己的创意。

## 分享你的想法

现在你已经读完了 *Visual Basic 快速入门指南*，我们很乐意听到你的想法！如果你从亚马逊购买了这本书，请点击此处直接进入这本书的亚马逊评论页面分享你的反馈，或者在你购买本书的网站上留下评论。

你的评论对我们和技术社区都很重要，将帮助我们确保提供优质的内容。

## 下载本书的免费 PDF 副本

感谢购买本书！

你喜欢随时随地阅读，但又无法随身携带纸质书吗？

你的电子书购买是否与你选择的设备不兼容？

别担心，现在购买每本 Packt 图书，你都可以免费获得该书的无 DRM PDF 版本。

随时随地，在任何设备上阅读。搜索、复制并粘贴你最喜欢的技术书籍中的代码，直接用于你的应用程序。

福利还不止这些，你还可以独家获得折扣、新闻通讯和每天发送到收件箱的精彩免费内容。

按照以下简单步骤即可获得这些福利：

1. 扫描二维码或访问下面的链接



![](img/421317ea489d4f3f077fe0c3dcdfc6fe_219_0.png)

https://packt.link/free-ebook/9781805125310

- 2. 提交您的购买证明
- 3. 就这样！我们会直接将免费 PDF 及其他福利发送到您的邮箱

