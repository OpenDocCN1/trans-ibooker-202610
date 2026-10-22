

# 第11章. WebAssembly 服务接口 (WASI)

# 致早期发布读者的一则说明

使用 Early Release 电子书，您可以在这些标题的正式发布之前很久就利用这些技术——作者在撰写时提供的原始且未经编辑的内容。

这将是最终书籍的第11章。

如果您对如何改进本书的内容和/或示例有任何意见，或者如果您注意到本章中缺少材料，请联系作者 wasmbook@bosatsu.net。

> 我并不生活在这个世界上是为了满足你的期望，你也不生活在这个世界上是为了满足我的期望。
> 
> ——布鲁斯·李

由于 WebAssembly 的安全性和安全性目标，作为后果，在其他平台上更容易实现的一些事情在 WebAssembly 中变得不必要地困难。在像 C、C++ 或 Rust 这样的语言中，从文件系统读取、向控制台写入以及在内存中操作字符串都是简单的活动。预计操作系统会允许具有足够权限的用户执行这些操作。没有明确的、上下文相关的边界。

不幸的是，这也是大多数现代网络威胁背后的问题，例如网络钓鱼攻击、权限提升、供应链攻击等等。如果攻击者能够说服特权用户运行不可信的代码，他通常可以窃取本不属于他的其他资源的访问权限。沙箱环境存在是为了防止这种情况，但它们通常很慢、笨重且给开发人员带来负担。WebAssembly 希望解决这个问题，并且在许多方面确实如此。从根本上说，WebAssembly 模块无法访问其托管环境未提供的任何内容。

到目前为止，我们看到的 MVP 和工具在很大程度上是关于使代码可移植的。现在我们将学习如何使应用程序可移植。解决方案，事实证明，从根本上说，是否能够满足期望。这不仅仅是语言的问题，也是应用程序编程接口 (API) 可用性、运行时环境配置、安全限制等等的问题。因此，就像布鲁斯·李的引言一样，我们的托管环境并不总是存在于运行的代码的期望中。我们仍然可以掌控。

# WebAssembly 系统接口 (WASI)

到目前为止您看到的酷炫演示和代码示例，特别是那些涉及图形、声音、视频等的内容，在很大程度上之所以可能，是因为它们在浏览器中运行。这是一种旨在成为通用客户端的软件，是其他代码的宿主。通过将任意程序发送到客户端执行，可以扩展 Web。它内置了安全限制，但如今它实际上是一个功能丰富的编程环境，充满了诸如 WebGL 用于 3D 图形、加速视频、WebRTC 用于协作等 API。Emscripten 通过浏览器运行时环境可用的内容提供了 POSIX 和 OpenGL 等标准 API 的实现，以便标准 C/C++ 应用程序只需编译即可继续工作。

我们已经看到了一种为模块实例提供函数以通过它们导入的对象调用的机制。例如，记住传递一个打印到控制台的 JavaScript 函数，而不是依赖标准 C 库来提供对 printf() 的访问。这是一种从根本上不令人满意的方法。每个需要打印到控制台的 WebAssembly 模块都应该只调用一个函数，最好是它最初编写时使用的那个，每当需要时。这关乎安全且高效地满足期望。

然而，由于安全问题，布鲁斯·李引言的另一部分是相关的。仅仅因为一个程序想要读取或写入文件系统并不意味着它就可以这么做。我们业务关键的桌面计算机并不存在于这个世界上是为了满足恶意软件的期望。我们希望控制应用程序在何种上下文中访问资源，如果有的话。

关于期望的另一个问题是，浏览器之外的运行时环境是 WebAssembly 潜在执行表面的重要组成部分。其中许多 API 在 Node.js 或 Deno 等环境中并不以相同的方式可用。没有兼容的 Web IDL¹ 定义的接口来公开对音频和视频播放、3D 图形等的访问。确实有其他类似的 API 可用，但它们与浏览器内部的内容并不直接兼容，应用程序必须重写才能使用它们。

通常，POSIX 函数将在不同的操作系统中一致定义，并映射到较低级别的内核函数或特定于平台的 API，例如 Win32²。这些是可重用功能的关键部分，我们希望它们是快速且稳定的实现。由应用程序开发人员提供的任意 JavaScript 包装函数可能不够快或稳定。许多这些浏览器外部 API 也没有沙箱浏览器环境相同的<|message|> 安全保证，因此它们在那里也不可直接使用。

这些是 WebAssembly 系统接口 (WASI) 试图解决的众多问题之一。这是一项艰巨的任务，到目前为止，这项工作正在扩展并变得相当复杂，但基础相当简单，所以我们将从那里开始。

最终，我们希望有一个功能生态系统，程序被允许访问时可以期望其可用。我们希望功能能够跨语言边界工作，这意味着我们需要方法来引用诸如字符串、列表和数组等高级结构。我们希望实现受保护但快速。我们希望某种类型安全，并且不必在我们编写的代码中依赖太多功能测试。我们还希望诸如垃圾回收和异常处理等语言特性能够从支持它们的语言中获得，而不会给不支持它们的语言带来负担。为了满足这些要求，WebAssembly 平台需要扩展。在第12章中，我将介绍其中一些主要扩展，但现在我希望保持在相对较高的层面。

让我们再一次重温第10章中我们著名的示例的 Rust 版本，如示例11-1所示。我们已经确定问题在于 println! 宏在浏览器中不可用。C 版本中的 printf() 函数也是如此。我们已经看到了各种绕过方法，但这些解决方案仍然令人困扰。

# 示例 11-1. Rust “Hello, World!”

```
fn main() {
    println!("Hello, World!");
}
```

出于稍后会变得清晰的原因，我将重建这个基本 Rust 程序的示例脚手架并运行它。cargo new 命令将建立项目的目录结构并填充一个简单的应用程序，其效果与上面的示例基本相同。

```
brian@tweezer ~/g/w/s/ch11> cargo new --bin hello-world
    Created binary (application) `hello-world` package
brian@tweezer ~/g/w/s/ch11> cd hello-world/
brian@tweezer ~/g/w/s/c/hello-world> cargo build --release
    Finished release [optimized] target(s) in 0.03s
brian@tweezer ~/g/w/s/c/hello-world> cargo run --release
    Finished release [optimized] target(s) in 0.01s
    Running `target/release/hello-world`
Hello, world!
```

cargo build 工具通过在 src/main.rs 中放置一个友好的程序为我们创建了项目脚手架。我们构建了此应用程序的优化发布版本，然后运行了相关的原生可执行文件。记住，Rust 基于 LLVM，因此默认后端将是您安装工具链的任何操作系统。

我们在第10章中看到，我们可以使用 WebAssembly 后端从 Rust 生成模块，但由于涉及字符串、内存管理、与操作系统交互等所有限制，它无法工作。

```
brian@tweezer ~/g/w/s/c/hello-world> cargo build ↵
  --target wasm32-unknown-unknown --release
    Compiling hello-world v0.1.0 (/Users/brian/git-personal/webassembly-the-definitive-guide/src/ch11/hello-world)
    Finished release [optimized] target(s) in 0.89s
brian@tweezer ~/g/w/s/c/hello-world> ls -laF target/wasm32-unknown-unknown/release/
total 3008
drwxr-xr-x  10 brian  staff   320 Jun 27 16:02 ./
drwxr-xr-x@  5 brian  staff   160 Jun 27 16:02 ../
drwxr-xr-x   2 brian  staff    64 Jun 27 16:02 build/
drwxr-xr-x   4 brian  staff   128 Jun 27 16:02 deps/
drwxr-xr-x   2 brian  staff    64 Jun 27 16:02 examples/
-rw-r--r--   1 brian  staff   228 Jun 27 16:02 hello-world.d
-rwxr-xr-x   2 brian  staff 1534049 Jun 27 16:02 hello-world.wasm*
drwxr-xr-x   2 brian  staff    64 Jun 27 16:02 incremental/
brian@tweezer ~/g/w/s/c/hello-world > wasm3 target/wasm32-unknown-unknown/release/hello-world.wasm
Error: [Fatal] repl_call: function lookup failed
Error: function lookup failed ('_start')
```

一个根本性问题是，基本模块结构未定义与可执行程序相同类型的应用程序二进制接口 (ABI)³。它没有预期的初始化函数名称。开发人员认为 main() 是起点，但通常这是由非常精简的 C 运行时环境从名为 "start" 的方法调用的。

另一个问题是，没有直接的方法从 WebAssembly 模块写入控制台或读写文件。这似乎是 WebAssembly 的一个相当大的障碍，但我鼓励你不要就此放弃希望！如果我们使用不同的后端会怎样？而不是 wasm32-unknown-unknown，我们将使用 wasm32-wasi。

