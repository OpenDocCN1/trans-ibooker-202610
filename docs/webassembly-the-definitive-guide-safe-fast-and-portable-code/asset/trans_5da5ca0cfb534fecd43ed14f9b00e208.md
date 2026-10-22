

```
Console.WriteLine($"You are {howOld.Invoke(store, 2021, 2000)}");
    }
}
}
```

执行代码显然会产生你所期望的结果。

```
brian@tweezer ~/g/w/s/wasm time-dotnet> dotnet run
You are 21
```

当然，我们对在主机与 WebAssembly 模块之间边界上想要执行的其他操作也有很好的支持。在示例 13-4 中，我们有一个从 .NET 主机导入的函数被调用的例子。

## 示例 13-4. 调用从主机环境传入的函数。

```
(module
  (type $voidfunc (func))
  (import "hello" "world" (func $hello.world (type $voidfunc)))
  (func $exec
    call $hello.world
  )
  (export "exec" (func $exec))
)
```

设置模块并共享要调用的函数的代码在示例 13-5 中突出显示。请注意，我们可以使用 C# 闭包作为存储在 Store 实例中的函数的基础。我们从模块中检索导出的 exec 函数并调用它，这反过来又调用了我们提供的 hello.world 命名空间函数。

## 示例 13-5. 我们的主机环境提供要调用的 C# 闭包

```
using System;
using Wasmtime;

namespace wasm time_dotnet
{
    class Program
    {
        static void Main(string[] args)
        {
            using var engine = new Engine();
```
```
using var module = Module.FromTextFile(engine, "hello.wat");
using var linker = new Linker(engine);
using var store = new Store(engine);

linker.Define(
    "hello",
    "world",
    Function.FromCallback(store,
        () => Console.WriteLine("I like soccer and shishkabobs."))
    );

var instance = linker.Instantiate(store, module);
var exec = instance.GetFunction(store, "exec");
exec.Invoke(store);
}
```

运行 dotnet 命令行将显示你所期望的结果。

```
brian@tweezer ~/g/w/s/wasmtime-dotnet> dotnet run
I like soccer and shishkabobs.
```

在脚注中链接的 GitHub 仓库中还有大量其他示例，包括其他 Wasi 集成的示例。也有使用 .NET 与 Wasmer 运行时相似的示例可用。主要观点是 WebAssembly 可以轻松地从 .NET 环境调用并与其交互。下一个用例有点偏离，但仍然是这些技术日益增长的组合。

## Blazor

微软对 WebAssembly 有另一种设想，这与他们重新致力于为所有主要平台的开发者提供工具和框架的兴趣相一致。Blazor 框架⁶ 允许开发者使用 C# 编写交互式组件和应用程序来面向 Web。开发可以在 Windows、macOS 和 Linux 上的各种工具上完成。

Blazor 使用 Razor 语法，这是一种 HTML 和 C# 的混合标记语言。如果你觉得这听起来有点像足球和串烤肉，我同意，但我们将在下面看到一些工作示例。主要好处是它允许开发者使用现有的工具和 .NET 库构建用户界面，而不必依赖 JavaScript 用户界面框架。虽然这些在 JavaScript 开发者中非常流行，但与更传统的面向对象用户界面体验相比，学习曲线相当陡峭。

使用该框架编写的应用程序可以选择部署在服务器上或在浏览器中。两种模式都允许用户界面在浏览器中呈现，但在第一种情况下，主要逻辑在服务器上执行。用户界面更新的通信通过 SignalR⁷ 连接管理。这是一种远程过程调用 RPC 方法，允许服务器通过 WebSockets、服务器发送事件或长轮询技术在浏览器中调用 JavaScript 中的行为。这不是我们将重点关注的用例，但它与我想就此用例提出 larger point 相关。我将在下面回到它。此部署模型在图 13-1 中可视化。

![](img/16b7990eb0dbf589db177bdbd2f4a189_282_0.png)

图 13-1. Blazor Server 部署模型

更相关的场景是称为 Blazor WebAssembly 的浏览器内部署模型。在此场景中，C# 和 Razor 文件被编译为 .NET 程序集，然后下载到浏览器。

生成的产物是一个在浏览器中运行的单页应用程序 SPA，可以对本地显示的内容进行更改。显然，为了使其工作，.NET 运行时也必须在浏览器中可用，因此它也必须被下载。本地用户界面更新能够通过 JavaScript 互操作直接影响 DOM。此部署模型如图 13-2 所示。

![](img/16b7990eb0dbf589db177bdbd2f4a189_283_0.png)

图 13-2. Blazor WebAssembly 部署模型

它可能不清楚这是如何工作的，但你不必再往前看，前文提到的 Mono 平台就足够了。它已被编译为 WebAssembly，因此在其之上运行的代码产物也应该在它支持的任何环境中工作。.NET 运行时的大小对于手机和嵌入式设备等小型设备来说可能是一个潜在问题，但总体而言，这是一个你会不频繁支付的代价，因为它只是一个依赖的网络资源，可以对其设置缓存控制，因此不会成为持续的带宽负担。

我们将在下面构建一个示例应用程序，但现在，请考虑图 13-3 所示的 Blazor WebAssembly 演示站点。这显示了一个简单的有状态计数器和一个用于操作它的按钮，以及左侧的一些可路由组件，用于在 SPA 中显示其他面板。

![](img/16b7990eb0dbf589db177bdbd2f4a189_284_0.png)

图 13-3. Blazor WebAssembly 演示网站

为了了解这可能如何工作，请考虑图 13-4 所示的细节。Mono 平台显示为 mono.wasm。它由一个名为 mono.js 的伴随 JavaScript 文件初始化，该文件看起来像我们在全书中看到的各种引导 JavaScript。在图像底部你会看到关于此引导过程的控制台日志详细信息。即使许多 .NET 程序集的大小为几十到几百千字节，你也会注意到它们都来自之前的会话缓存，因此无需重新获取。

![](img/16b7990eb0dbf589db177bdbd2f4a189_285_0.png)

图 13-4. Blazor 演示网站周围的网络流量

两种部署策略各有利弊。Blazor Server 模型不会给客户端带来下载大型二进制运行时的负担，但后端与前端用户界面组件之间的交互存在延迟问题。在缓慢和不稳定的网络或远程客户端上，这可能存在问题。Blazor WebAssembly 模型通常无法访问完整的 .NET 平台，以便使运行时下载保持在可管理的大小。对于目前而言，调试也是 Blazor WebAssembly 的一个问题，但这显然是微软应该能够解决的问题，鉴于他们致力于为开发者提供良好工具的态度。

现在，让我们生成 Blazor WebAssembly 演示脚手架。幸运的是，我们不需要在之前的示例中已经使用过的东西之外安装任何东西。然而，我们将引用一个名为 blazorwasm 的新模板，它将为我们设置此部署模型。如果你想尝试创建服务器托管模型，你将使用 blazorsserver 作为你的模板名称。

```
brian@tweezer ~/g/w/s> dotnet new blazorwasm -o blazor-web
The template "Blazor WebAssembly App" was created successfully.
This template contains technologies from parties other than Microsoft,
see https://aka.ms/aspnetcore/5.0-third-party-notices for details.

Processing post-creation actions...
Running 'dotnet restore' on /Users/brian/src/blazor-web/blazor-web.csproj...
    Determining projects to restore...
    Restored /Users/brian/src/blazor-web/blazor-web.csproj (in 4.83 sec).
Restore succeeded.
```

此时，我们有一个可用的工作应用程序。如果我们调查目录结构，我们会发现我们确实站在巨人的肩膀上。幕后有大量的代码在运行，以使一个看似简单的应用程序正常工作。下面的 tree 命令产生了 700 行文件和目录。当然，典型的 JavaScript 框架也会引入大量的依赖。你可以自行决定权衡是否值得，但显然许多组织会得出结论认为它是值得的。

我下面大量删除了多余的行，只是为了专注于你关心的核心文件，但如果你感觉冒险并且安装了 tree，不妨试试。

剩下的是一个相当直接的项目。以 .razor 结尾的文件是组件文件。目录结构中散布着几个配置文件。几个组件具有自定义的层叠样式表 CSS 文件来定义其样式。

```
brian@tweezer ~/g/w/s/c/blazor-web> tree .
├── App.razor
```
