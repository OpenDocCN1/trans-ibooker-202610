## Appendix A. Installing WebAssembly Tools

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at wasmbook@bosatsu.net.

> Technology is nothing. What’s important is that you have a faith in people, that they’re basically good and smart, and if you give them tools, they’ll do wonderful things with them.
> 
> —Steve Jobs

It is unsurprising given all of the languages, tools and frameworks that we discuss in this book that there is a fair amount to install. This appendix will not be comprehensive, but will try to point you in the right direction to getting everything going. Some of the tools are easier to install on Linux or macOS, but most should work on Windows too.

#### Installing WebAssembly Binary Toolkit (WABT)

The WebAssembly Binary Toolkit (WABT) provides a suite of tools for converting things too and from the various formats we have discussed as well as several others. It includes tools for dumping out details about the modules, validating their structures and more.

There are quite good instructions on the GitHub repo for building on all three major operating systems so there is no point in replicating that here. The repo is located here:

https://github.com/WebAssembly/wabt

One thing I did want to point out was that some of the tools are also available online. It shouldn’t be too surprising that WebAssembly tools might also run in the browser. If you want to try converting formats without installing the tools, you can try them out here:

https://webassembly.github.io/wabt/demo/

Additionally, there is a spin off project called Wabt.js that allows you to use much of the functionality from the toolkit in the browser. It’s GitHub repository is here:

https://github.com/AssemblyScript/wabt.js/

#### Installing LLVM

LLVM looks like it would stand for something but it doesn’t. It’s just the name for an exceptionally cool modular compiler architecture that serves as the basis for languages such as Rust, Swift, Julia and many more.

LLVM is able to emit platform-specific codes even for different platforms if you install the correct tooling. It is also useful for experimenting with optimizations, running the intermediate form in a virtual machine and producing WebAssembly.

I highly recommend you install LLVM through one of the installers rather than building it from scratch as it takes forever and consumes an obscene amount of disk space. Depending on your OS, you may already have a version installed. The macOS toolchain is LLVM-based, but that version does not yet interoperate with WebAssembly.

The main website is here:
https://llvm.org

There are installers for most major operating systems so you should not have difficulty finding one that will work.

#### Installing Emscripten

The Emscripten toolchain is a set of tools that wrap the LLVM tools. Originally it emitted asm.js, but now it supports WebAssembly directly. In addition to assisting with compiling existing C and C++ code, it has drop in replacements for other command line tools for building software such as Make and configure.

With its macros and compiler directives, it is fairly straightforward to simplify communicating between JavaScript host environments in the browser or Node.js. It has support for widely-used dependencies like the standard library and OpenGL.

The getting started guide has good instructions for multiple operating systems so your best bet will be to investigate this site:
https://emscripten.org/docs/getting_started/index.html

#### Installing Wasm3

Wasm3 bills itself as “The fastest WebAssembly interpreter, and the most universal runtime.”

The GitHub repo is here:
https://github.com/wasm3/wasm3

Given the variety of platforms it runs on and languages it works with, I am not going to challenge them on that. It currently runs on:

- Linux, Windows, macOS, FreeBSD, Android, iOS
- OpenWrt, Yocto, Buildroot (network equipment)
- Raspberry Pi and other Single Board Computers
- A variety of microcontrollers
- Most modern Browsers

It is also doing a good job of tracking the various new proposals.

It is easy enough to build from source, but there are several installers documented here:

https://github.com/wasm3/wasm3/blob/main/docs/Installation.md

I also encourage you to check out the helpful cookbook here:

https://github.com/wasm3/wasm3/blob/main/docs/Cookbook.md

#### Installing WasmTime

Wasmtime was originally a Mozilla project but is maintained by the Bytecode Alliance these days. They refer to it as “a standalone wasm-only optimizing runtime for WebAssembly and WASI”.

It is one of the most up-to-date runtimes for the various proposals and has an extensive set of programmatic libraries as you have seen throughout the book.

There is quite extensive documentation here:

https://docs.wasmtime.dev

You can find installation instructions here:

https://docs.wasmtime.dev/cli-install.html

#### Installing Wasmer

Wasmer was one of the first non-browser and non-Node.js WebAssembly runtimes I encountered. It pre-dated WASI but quickly added support.





Wasmer 的主网站是：

https://wasmer.io

除了作为独立运行时之外，它还支持以下语言的集成：

- Rust
- C 和 C++
- JavaScript
- Go
- Python
- PHP
- Ruby

它还维护着 WebAssembly 包管理器（WAPM）、WebAssembly.sh 以及 Wasienv 工具包。

一个非常适合入门并安装运行时的好地方是他们优秀的文档，链接如下：

https://docs.wasmer.io/ecosystem/wasmer/getting-started

### 安装 Rust 工具

Rust 在 WebAssembly 生态系统中显然占有重要地位，但它本身作为一种安全、快速且采用率迅速增长的编程语言也大有可言。

主网站是：

https://rust-lang.org

你通常会想要使用 rustup 工具来管理你的 Rust 工具链。他们支持 nightly、beta 和 stable 版本。根据你自己的舒适程度或对最新功能的渴望，你可以快速轻松地在已安装的各个通道之间切换。随时安装多个通道也很简单。如果你有兴趣进行交叉编译，也可以安装其他架构的后端。

首先安装此处记录的 rustup 工具链：

https://www.rust-lang.org/tools/install

要直接生成 WebAssembly，你必须安装后端。你可以选择任意通道，但要安装 nightly WebAssembly 后端，你可以这样做：

```
~/s/r/wasm> rustup target add wasm32-unknown-unknown --toolchain nightly
```

如果没有将其设为默认工具链，在编译 Rust 时使用后端，你可以这样做：

```
~/s/r/wasm> rustc +nightly --target wasm32-unknown-unknown \
  -O --crate-type=cdylib add.rs -o add.wasm
```

要生成 WASI 代码，你需要安装 WASI 后端：

```
~/s/r/wasm> rustup target add wasm32-wasi --toolchain nightly
```

如果它不是默认工具链，使用它：

```
~/s/r/wasm> rustc hello.rs --target wasm32-wasi
```

请注意，常规 Rust 原生后端、常规 WebAssembly 和 WASI 后端是不同的，具体取决于你想要的目标。

如果你想在 Rust 和 JavaScript 之间无缝交互，你可能需要安装 wasm-bindgen。

该工具的优秀介绍在这里：

https://rustwasm.github.io/wasm-bindgen/

安装了 Rust 工具后，你应该可以用以下命令安装它：

```
~/s/rust> cargo install -f wasm-bindgen-cli
```

### 安装 Dotnet 工具

正如你在书中所看到的，.NET 已成为一个强大的 WebAssembly 环境，特别是现在它已跨平台。

好消息是安装起来非常简单。主要操作系统的说明位于此处：

https://dotnet.microsoft.com/download

安装完成后，你应该能够运行我们在书中介绍的命令行示例。

### 安装 AssemblyScript

AssemblyScript 正在 WebAssembly 世界中崭露头角。它在过去和未来之间取得了很好的平衡。你不必学习 C 或 C++ 代码，可以留在相对熟悉的语言空间中，同时仍然生成更高性能的软件。

主网站在这里：

https://www.assemblyscript.org

安装说明在这里：

https://www.assemblyscript.org/quick-start.html

### 安装 IPFS

星际文件系统（IPFS）与 WebAssembly 并无直接关系，但作为去中心化领域的关键参与者，它们有几个交集的地方。我在书中强调了一个例子。

如果你想了解更多关于该项目的信息，网站在这里：

https://ipfs.io

安装选项有多种，详细说明在这里：
https://ipfs.io/#install

### 安装 TinyGo

正如我在上一章所指出的，TinyGo 正在成为 Go 与 WebAssembly 集成的一个不错的起点，同时它还支持微控制器和其他嵌入式系统。

主网站在这里：
https://tinygo.org

在各种操作系统或 Docker 上安装的说明可在以下位置获得：
https://tinygo.org/getting-started/install/

### 安装 Artichoke

Artichoke 是将 Ruby 带入 WebAssembly 派对的一个引人注目的开端。还处于早期阶段，但正如我提到的，他们正在寻找贡献者。

你可以在这里找到安装说明：
https://www.artichokeruby.org/install/

### 安装 SwiftWasm

SwiftWasm 也处于早期阶段，但对于这种日益有趣的、已摆脱 macOS 起源的语言来说，它显示出很大的潜力。各种安装选项可在以下位置获得：
https://book.swiftwasm.org/getting-started/setup.html

### 安装 Zig 和 Grain

Zig 和 Grain 都是引人注目的新语言。它们是我非常感兴趣深入研究的语言。虽然它们的使用范围还远不广泛，但兴趣正在增长。它们拥有强大的新兴 WebAssembly 策略这一事实可能会迅速放大它们的影响，因为在许多情况下你不需要新的运行时工具。

尽管它们是分开且无关的，我将它们捆绑在一起，因为我认为它们发挥相似的作用并且处于相似的阶段。

我鼓励你进一步深入研究两者。

Zig 可在此处获得：

https://ziglang.org/download/

Grain 可在此处获得：

https://grain-lang.org/docs/getting_grain
