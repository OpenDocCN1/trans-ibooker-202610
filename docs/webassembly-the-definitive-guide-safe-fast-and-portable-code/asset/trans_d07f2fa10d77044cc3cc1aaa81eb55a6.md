

8 wasm-feature-detect 库的 GitHub 仓库在这里：
https://github.com/GoogleChromeLabs/wasm-feature-detect

9 wabt.js 是我们在本书早期介绍的 WebAssembly Binary Toolkit (WABT) 功能的移植。它的 GitHub 仓库在这里：
https://github.com/AssemblyScript/wabt.js

10 你可以目前在这里跟踪主要提案及其所处的阶段：
https://github.com/WebAssembly/proposals

11 尽管它经常被归因于奥托·冯·俾斯麦，但证据表明说法律像香肠一样的是约翰·戈德弗雷·萨克斯。如果你喜欢其中任何一种，你就不应该观看它们是如何制作的。

## 第 13 章. WebAssembly 与 .NET

## 早期发布读者的注意事项

使用 Early Release 电子书，您可以在这些标题的正式发布之前很长时间就利用这些技术——作者在写作时提供的原始和未编辑的内容，以便您可以在这些技术正式发布之前利用它们。

这将是最终书籍的第 13 章。

如果您对如何改进本书中的内容和/或示例有任何意见，或者如果您注意到本章中缺少材料，请联系作者 wasmbook@bosatsu.net。

> 我认为桌上足球¹ 是一种足球和烤肉串的结合。
> ——米奇·赫德伯格

在我整个职业生涯中，我与 .NET 保持着一种尊重的冷漠关系。我对它没有什么意见，我只是几乎从未需要过它，并且花很少的时间在 Windows 特定的工作上，而在很多年里它就是这样。

在 2000 年代初期，我确实帮助与一些同事启动了一个基于 .NET 的项目，用于可视化模拟建筑物攻击的场景。想法是，通过使用爆炸的真实物理模型，我们可以将可视化与基于各种保护措施的可能结果结合起来。安全规划者可以考虑物理屏障的不同位置，并对涉及装有炸药的车辆的设施进行测试。如果他们只能靠近到如此程度，对建筑物或刚进入入口的人会造成什么样的影响²？Windows 是目标平台，显然我们需要一些计算速度，所以我们为核心库选择了托管 C++ 基础，为应用程序选择了 C#。我没有在这个项目上待很久，但最终它是一个相当成功的项目。

除此之外，我做过我需要用 Java、C++、JavaScript、Rust、Python 和其他语言和环境做的事情。我认识很多喜欢 .NET 和其周围工具的人，但它对我的职业生涯来说从来没有太重要过。

话虽如此，我一直对像 Mono³ 这样的计划很感兴趣，所以我会 somewhat 关注那个世界正在发生的事情。加上，我的很多朋友都是该领域的专家和倡导者，所以他们也会让我知道那里发生了什么。其中一个令人兴奋的转变是，微软的开发工具和 .NET 框架现在是跨平台开发的真正解决方案。

当我开始了解一些正在获得动力的 .NET 和 WebAssembly 计划时，我仍然感到惊讶。它们最初感觉像是足球和烤肉串的结合，奇怪而过时的搭配。然而，我越想越觉得，这些技术的整合中存在一些引人注目且后果重大的方面，值得探索。

## .NET 和 Wasmtime

正如您可能已经开始意识到的那样，WebAssembly 的用例远不止 MVP 带来的直接方法。我们将从一个不完全是 .NET 特定的集成开始，类似于我们在前几章中看到的。Wasmtime 平台通过其 NuGet 包⁴ 支持 .NET 集成。此示例支持使用 WebAssembly 模块、库和插件扩展 .NET 应用程序的想法。

在尝试以下操作之前，您需要安装 .NET Core SDK。请参阅附录 A 以获取帮助您完成此操作的说明。一旦工具可用，我们将创建一个新目录

并初始化一个项目。dotnet 命令行工具能够基于现有模板创建结构。其中最简单的一个是控制台模板，所以我们将使用它。

```
brian@tweezer ~/g/w/s> mkdir wasmtime-dotnet
brian@tweezer ~/g/w/s> cd wasmtime-dotnet
brian@tweezer ~/g/w/s/wasmtime-dotnet> dotnet new console
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on /Users/brian/src/wasmtime-dotnet/wasmtime-dotnet.csproj...
    Determining projects to restore...
    Restored /Users/brian/src/wasmtime-dotnet/wasmtime-dotnet.csproj (in 58 ms).
Restore succeeded.
```

一旦项目结构初始化，我们将需要添加对 Wasmtime NuGet 包的依赖。截至本文撰写时，版本 0.28.0-preview1 是最新版本。您可能想要或需要使用更新的版本。

```
brian@tweezer ~/g/w/s/wasmtime-dotnet> dotnet add package ↩
  --version 0.28.0-preview1 wasmtime
    Determining projects to restore...
    Writing
/var/folders/mn/4kd_fxdj3lxbp ljfhjyp5sw40000gn/T/tmp5A8L0B.tmp
info : Adding PackageReference for package 'wasmtime' into
project '/Users/brian/src/wasmtime-dotnet/wasmtime-dotnet.csproj'.
info : Restoring packages for /Users/brian/src/wasmtime-dotnet/wasmtime-dotnet.csproj...
info :   GET https://api.nuget.org/v3-flatcontainer/wasmtime/index.json
info :   OK https://api.nuget.org/v3-flatcontainer/wasmtime/index.json 234ms
info :   GET https://api.nuget.org/v3-flatcontainer/wasmtime/0.28.0-preview1/wasmtime.0.28.0-preview1.nupkg
info :   OK https://api.nuget.org/v3-flatcontainer/wasmtime/0.28.0-preview1/wasmtime.0.28.0-preview1.nupkg 235ms
info :   GET https://api.nuget.org/v3-flatcontainer/microsoft.csharp/index.json
info :  OK https://api.nuget.org/v3-flatcontainer/microsoft.csharp/index.json 210ms
info :  GET https://api.nuget.org/v3-flatcontainer/microsoft.csharp/4.6.0/microsoft.csharp.4.6.0.nupkg
info :  OK https://api.nuget.org/v3-flatcontainer/microsoft.csharp/4.6.0/microsoft.csharp.4.6.0.nupkg 20ms
info : Installed Microsoft.CSharp 4.6.0 from
https://api.nuget.org/v3/index.json with content hash
kxn3M2rnAGy5N5DgcIwcE8QTePWU/XiYcQVzn9HqTls2NKluVzVSmVWRjK7OUPWbl
jCXuZxHyhEz9kPRI QeXow==.
info : Installed Wasmtime 0.28.0-preview1 from
https://api.nuget.org/v3/index.json with content hash
diVfo3IYN8ZBRwY30dlWme7vFC2NelIeqBCNiaK/FIGVLoa6IHoBfpsDqBm9gjcNY
8wDsRDo/u4xRquSCG+2dg==.
info : Package 'wasmtime' is compatible with all the specified
frameworks in project '/Users/brian/src/wasmtime-dotnet/wasmtime-
dotnet.csproj'.
info : PackageReference for package 'wasmtime' version '0.28.0-
preview1' added to file '/Users/brian/src/wasmtime-
dotnet/wasmtime-dotnet.csproj'.
info : Committing restore...
info : Writing assets file to disk. Path:
/Users/brian/src/wasmtime-dotnet/obj/project.assets.json
log  : Restored /Users/brian/src/wasmtime-dotnet/wasmtime-
dotnet.csproj (in 2.45 sec).
```

此时，您的项目中应该生成了一个名为 Program.cs 的控制台应用程序，它应该看起来像示例 13-1。

### 示例 13-1. 生成的 C# 控制台应用程序

```
using System;

namespace wasmtime_dotnet
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

通过执行以下命令仔细检查一切是否正确设置。

```
brian@tweezer ~/g/w/s/wasmtime-dotnet> dotnet run
Hello World!
```

如果您看到了我们通常友好的问候，我们就可以继续了。为了保持简单，我们将只使用我们现有的 howold 函数以保持一致性。供参考，请确保示例 13-2 中显示的模块位于您的项目目录中名为 hello.wat 的文件中（例如，如果您遵循了我上面的命令，则为 wasmtime-dotnet）。

### 示例 13-2. 我们的年龄计算 Wasm 模块

```
(module
  (func $how_old (param $year_now i32) (param $year_born i32)
  (result i32)
    get_local $year_now
    get_local $year_born
    i32.sub)
  (export "how_old" (func $how_old))
)
```

不出所料，Wasmtime 类将与我们在 Rust 中看到的名称类似，尽管我们现在将使用 C# 作为集成的基础。如果您将 Program.cs 文件更改为示例 13-3 中看到的内容，我们就可以在 Wasi 支持⁵ 下恢复工作。

### 示例 13-3. 使用 Wasmtime 在 C# 中执行 WebAssembly

```
using System;
using Wasmtime;

namespace wasmtime_dotnet
{
    class Program
    {
        static void Main(string[] args)
        {
            using var engine = new Engine();
            using var module = Module.FromTextFile(engine, "hello.wat");
            using var linker = new Linker(engine);
            using var store = new Store(engine);

            var instance = linker.Instantiate(store, module);
            var howOld = instance.GetFunction(store, "how_old");
```

