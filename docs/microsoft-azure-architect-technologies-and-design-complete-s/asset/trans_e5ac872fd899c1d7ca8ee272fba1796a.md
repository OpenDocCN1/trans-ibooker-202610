

+   5. 在本地工作站打开一个 CMD 窗口并输入以下 curl 命令作为单行。（此处命令行被拆分以提高可读性。）

```
```bash
curl https://<functionName>.azurewebsites.net/api/HttpTrigger1?code=xcEFmx...
-d "{\"name\":\"Benjamin\"}"
```
```

输出将与图 4.55 所示类似。

![](img/95043b3df926bcdadadb082b49262037_366_0.png)

**图 4.55** 使用 curl 消费 HTTP 触发的 Azure Function

+   6. 为确认来自包的运行功能，点击函数名称添加一个应用设置。点击 Configuration 链接并点击 + New Application Setting 链接。输入 **WEBSITE_RUN_FROM_PACKAGE** 作为名称，输入 **1** 作为值，点击 OK 按钮，最后点击 Save 按钮。

你可以通过在浏览器中 IDE 右侧选择 Test 项目然后按下 Run 按钮，从门户内部测试 Azure Function。

我将指出在创建 Azure Function 过程中所做决策的一些要点。首先，并非所有运行时堆栈都支持在门户中开发。例如，Python 和 Java 都要求你在本地开发然后部署；因此练习选择了 .NET Core，因为示例稍微简单一些。你也选择不启用 Application Insights。这是一个有价值的工具，而且由于这只是一个示例，因此未启用。如果你在开发 Azure Function 来运行某些实时工作负载，那么 Application Insights 绝对是推荐的。Application Insights 将在第 9 章中更详细地讨论，但请记住，Microsoft（即 FaaS 提供）不会记录你的应用程序错误。云托管提供商主要关注平台。你作为应用程序的所有者需要在代码中编写异常处理程序，而你将这些异常写入的位置就是 Application Insights。最后，虽然练习是通过门户创建的 Azure Function，但该场景更多是为了让你上手并了解一些功能。对于更复杂的场景，建议使用 Visual Studio、Visual Studio Code、IntelliJ、Eclipse 或其他开源 IDE 在本地开发和测试。

## 运行时版本

运行时是在内存中支持代码的集合，有助于管理和执行在其内部运行的自定义应用程序。运行时是什么之前已经提到过，因此此处不需要更深入的介绍。从 Azure Functions 角度来看，运行时有三个版本：版本 1、版本 2 和版本 3。版本 1 和版本 2 之间的差异相当显著，而且如果要创建新函数，务必选择最新版本。Azure Functions 的版本 1 针对 .NET Framework，该框架不支持跨平台执行。这意味着你受限于 Windows 以及完整的 .NET Framework 库。这不一定是坏事；它只是不是最新的东西。版本 2 针对 .NET Core 2.x 且跨平台。.NET Core 已被证明性能更好且占用更小。此外，在版本 1 中，可以在同一个 Function 应用中包含用 C# 编写的函数和用 JavaScript 编写的函数。这种情况在版本 2 中不再受支持。Function 应用内的所有函数必须使用相同的运行时堆栈。

要查看你的 Azure Function 目标运行时，从门户中的 Overview 刀片点击 Function App Setting。当刀片呈现时，你会看到与图 4.56 类似的内容。

图 4.56 Azure Function 运行时版本的视图

![](img/95043b3df926bcdadadb082b49262037_367_0.png)

此时，Azure Function 运行时版本 3 处于预览状态；然而，它正快速向全球可用（GA）推进。版本 3 针对 .NET Core 3.x。请注意版本 ~1、~2 和 ~3 前面的波浪号。这通知平台当发布更新的运行时版本时，运行的代码将自动针对该新版本。如图 4.56 所示，有一个特定的运行时版本 2.0.12858.0。如果将来有更新的版本你的代码无法支持，那么你可以使用通过门户 Configuration 刀片可访问的 FUNCTIONS_WORKER_RUNTIME 应用设置参数将函数固定到特定运行时版本。

## 支持的编程语言

所选的运行时版本很重要，因为它将决定 Azure Function 运行的操作系统以及你可以用其编写代码的语言。如前所述，版本 1 针对完整的 .NET Framework 版本，该版本不是跨平台的，因此只能在 Windows 操作系统上运行。然而，版本 2

以及任何未来版本都针对 .NET Core，该核心是跨平台的，因此可以在 Windows 或 Linux 上运行。

从语言角度来看，C#、JavaScript（Node）和 F# 在 Azure Function 运行时所有受支持的版本中都受支持。Java、PowerShell、Python 和 TypeScript 在版本 2 及更高版本中受支持。随着时间的推移，很可能正在进行支持更多语言的工作。请参阅表 4.15，了解 Azure Function 运行时版本支持的语言概览。

### 表 4.15 Azure Function 支持的语言

| Language | 1.x | 2.x | 3.x |
|---|---|---|---|
| C# | .NET Framework 4.7 | .NET Core 2.2 | .NET Core 3.x |
| JavaScript | Node 6 | Node 8 and 10 | Node 8 and 10 |
| F# | .NET Framework 4.7 | .NET Core 2.2 | .NET Core 3.x |
| Java | X | Java 8 | Java 8 |
| PowerShell | X | PowerShell Core 6 | PowerShell Core 6 |
| Python | X | Python 3.6 | Python 3.6 |
| TypeScript | X | ✓ | ✓ |

截至撰写本章时，版本 3 处于预览状态，且 3.x 列中的所有语言也都被视为预览。此外，在版本 1 的上下文中，还有许多其他语言被视为“实验性”。我使用过去时是因为这些语言将保持“实验性”，永远不会成为 Azure Functions v1 的完全受支持的开发语言。这些语言是 Bash、Batch 和 PHP。

以下哪些运行时堆栈可用于在 Linux 上运行 Azure Function？

-   A. JavaScript
-   B. .NET Core
-   C. Python
-   D. .NET Framework

答案是 A、B 和 C，因为 .NET Framework 不是跨平台的，因此无法在 Linux 操作系统上运行。

每个服务（安全、订购、物流等）都有一组可执行代码，这些代码可能重叠、重用或组合逻辑并共享库。对单体应用程序任何部分的更改都会对在多个层中托管的所有服务产生影响。

从 Azure Function 角度来看，你可能会认识到与微服务术语的一些相似之处，即 Azure Function 执行的工作单元将比在这些多层上运行的 IT 解决方案要小得多。你可以考虑为给定 Function 应用中的每个服务实现一个 Function。该场景是有效的，只要 Functions 的数量可管理并且可以在沙箱中运行。然而，没有编排器可以运行大量可能相互依赖的 Azure Functions，而且你只能扩展 Function 应用而非特定的 Functions。这就是 Service Fabric 和微服务填补的空白。Service Fabric 是一个编排器，有助于高效执行和管理大量小型隔离程序。例如，参见图 4.58，其中不是用绑定到 VM 的专用层或在 Function 应用内运行这些功能，每个服务都可以独立扩展和管理。

## 图 4.58 微服务架构

![](img/95043b3df926bcdadadb082b49262037_370_0.png)

独立扩展/管理微服务是一个有趣的概念。在单体代码库中，你有许多方法执行许多不同的事情，它不会花费

