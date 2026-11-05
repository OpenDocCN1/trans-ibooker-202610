
![](img/c63b00dca7192cc58cfb9e1f1e64f483_795_0.png)

好实践：你现在可以在其他 Blazor Server 项目中重用 Blazor 组件。然而，由于它依赖于完整的 ASP.NET Core 工作负载，你不能在 Blazor WebAssembly 项目中使用该类库。创建适用于两种托管模型的 Blazor 组件库超出了本书的范围。

## 与 JavaScript 的互操作

默认情况下，Blazor 组件无法访问浏览器的功能，如本地存储、地理位置和媒体捕获，或任何 JavaScript 库，如 React 或 Vue。如果需要与它们交互，可以使用 JavaScript 互操作。

让我们看一个使用浏览器窗口的 alert 框和本地存储的示例，本地存储可以无限期地为每个访客持久保存多达 5 MB 的数据：

1. 在 Northwind.BlazorServer 项目中，在 wwwroot 文件夹中，添加一个名为 scripts 的文件夹。
2. 在 scripts 文件夹中，添加一个名为 interop.js 的文件。
3. 修改其内容，如以下代码所示：
4. 在 Pages 文件夹中，在 _Layout.cshtml 中，在添加 Blazor Server 支持的 script 元素之后，添加一个引用你刚刚创建的 JavaScript 文件的 script 元素，如以下代码所示：

```
function messageBox(message) {
  window.alert(message);
}

function setColorInStorage() {
  if (typeof (Storage) !== "undefined") {
    localStorage.setItem("color",
      document.getElementById("colorBox").value);
  }
}

function getColorFromStorage() {
  if (typeof (Storage) !== "undefined") {
    document.getElementById("colorBox").value =
      localStorage.getItem("color");
  }
}
```

```
<script src="scripts/interop.js"></script>
```

5. 在 Pages 文件夹中，在 Index.razor 中，删除两个 Customers 组件实例，然后添加一个按钮和一个代码块，该代码块使用 Blazor JavaScript 运行时依赖服务调用 JavaScript 函数，如以下代码所示：

```
<button type="button" class="btn btn-info" @onclick="AlertBrowser">
    Poke the browser</button>

<hr />

<input id="colorBox" />

<button type="button" class="btn btn-info" @onclick="SetColor">
    Set Color</button>

<button type="button" class="btn btn-info" @onclick="GetColor">
    Get Color</button>

@code {
    [Inject]
    public IJSRuntime JSRuntime { get; set; } = null!;

    public async Task AlertBrowser()
    {
        await JSRuntime.InvokeVoidAsync(
            "messageBox", "Blazor poking the browser");
    }

    public async Task SetColor()
    {
        await JSRuntime.InvokeVoidAsync("setColorInStorage");
    }

    public async Task GetColor()
    {
        await JSRuntime.InvokeVoidAsync("getColorFromStorage");
    }
}
```

6. 启动 Northwind.BlazorServer 项目。
7. 启动 Chrome 并导航到 https://localhost:5001/。
8. 在主页上，在文本框中输入 red，然后单击 Set Color 按钮。
9. 显示开发者工具，选择 Application 选项卡，展开 Local Storage，选择 https://localhost:5001，并注意键值对 color-red，如图 17.13 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_797_0.png)

图 17.13：使用 JavaScript 互操作在浏览器本地存储中存储颜色

10. 关闭 Chrome 并关闭 Web 服务器。
11. 启动 Northwind.BlazorServer 项目。
12. 启动 Chrome 并导航到 https://localhost:5001/。
13. 在主页上，单击 Get Color 按钮并注意文本框中显示的值 red，该值是从本地存储中在访客会话之间检索的。
14. 关闭 Chrome 并关闭 Web 服务器。

## Blazor 组件库

有很多 Blazor 组件库。付费组件库来自 Telerik、DevExpress 和 Syncfusion 等公司。开源 Blazor 组件库包括以下内容：

- Radzen Blazor 组件： https://blazor.radzen.com/
- 精彩开源 Blazor 项目： https://awesomeopensource.com/projects/blazor

## 练习和探索

通过回答一些问题来测试你的知识和理解，进行一些实践操作，并通过更深入的研究探索本章主题。

### 练习 17.1 – 测试你的知识

回答以下问题：

1. Blazor 的两种主要托管模型是什么，它们有何不同？
2. 在 Blazor Server 网站项目中，与 ASP.NET Core MVC 网站项目相比，Startup 类需要什么额外的配置？
3. Blazor 的好处之一是能够使用 C# 和 .NET 而不是 JavaScript 实现客户端组件。Blazor 组件需要任何 JavaScript 吗？
4. 在 Blazor 项目中，App.razor 文件做什么？
5. 使用 <NavLink> 组件的好处是什么？
6. 如何将值传递给组件？
7. 使用 <EditForm> 组件的好处是什么？
8. 如何在设置参数时执行某些语句？
9. 如何在组件出现时执行某些语句？
10. Blazor Server 和 Blazor WebAssembly 项目的 Program 类之间有哪些两个关键区别？

### 练习 17.2 – 通过创建乘法表组件进行练习

创建一个基于名为 Number 的参数渲染乘法表的组件，然后以两种方式测试你的组件。

首先，通过将你的组件实例添加到 Index.razor 文件中，如以下标记所示：

```
<timestable Number="6" />
```

其次，通过在浏览器地址栏中输入路径，如以下链接所示：

https://localhost:5001/timestable/6

### 练习 17.3 – 通过创建国家导航项进行练习

向 CustomersController 类添加一个 action 方法以返回国家名称列表。

在共享的 NavMenu 组件中，调用客户的 Web 服务以获取国家名称列表并循环遍历它们，为每个国家创建菜单项。

### 练习 17.4 – 探索主题

使用以下页面上的链接了解本章涵盖主题的更多详细信息：

https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-17---building-user-interfaces-using-blazor

## 摘要

在本章中，你学习了如何构建托管于 Server 和 WebAssembly 的 Blazor 组件。你看到了两种托管模型之间的一些关键差异，例如如何使用依赖服务管理数据。

## 尾声

我希望这本书与市场上其他书籍不同。我希望你觉得它读起来节奏明快、趣味十足，每个主题都包含实用的动手演练。

本尾声包含以下简短部分：

- 你在 C# 和 .NET 学习之旅中的下一步
- .NET MAUI 延迟
- 下一版将于 2022 年 11 月发布
- 祝好运！

### 你在 C# 和 .NET 学习之旅中的下一步

对于你想了解更多但本书篇幅有限无法包含的主题，我希望 GitHub 仓库中的笔记、良好实践提示和链接能为你指明正确的方向：

```
https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md
```

### 使用设计指南打磨你的技能

现在你已经学习了使用 C# 和 .NET 开发的基础知识，你可以通过学习更详细的设计指南来提高代码质量。

早在 .NET Framework 早期，Microsoft 就发布了一本涵盖 .NET 开发各方面的良好实践书籍。那些建议在现代 .NET 开发中仍然非常适用。

以下主题涵盖：

- 命名指南
- 类型设计指南
- 成员设计指南
- 为可扩展性而设计
- 异常设计指南
- 使用指南
- 常见设计模式

为了使指导尽可能易于遵循，建议仅使用 **Do**、**Consider**、**Avoid** 和 **Do not** 等术语进行标记。

Microsoft 已在以下链接提供该书的摘录：

```
https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/
```

我强烈建议你查看所有指南并将其应用到你的代码中。

### 进一步提升学习的书籍

如果你正在寻找我出版社出版的涵盖相关主题的其他书籍，有很多可供选择。我推荐 Harrison Ferrone 的 *Learning C# by Developing Games with Unity 2021* 作为学习 C# 的有趣补充。

还有许多书籍将 C# 和 .NET 提升到更高层次，如图 18.1 所示：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_802_0.png)

图 18.1：Packt 出版的进一步提升你的 C# 和 .NET 学习书籍
