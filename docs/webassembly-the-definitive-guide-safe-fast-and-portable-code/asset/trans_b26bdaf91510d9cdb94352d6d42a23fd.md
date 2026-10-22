

上一条命令生成的 JavaScript 文件。看看那个文件列表！它超过 120 千字节！它有超过 2,000 行代码。如果你查看它，你可能会很快迷失方向。这就是为什么我在前几章中不想从那里开始的原因。

## 示例 6-2. 与我们之前看到的截然不同的 HTML 文件。

```
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <link rel="stylesheet" href="bootstrap.min.css">
        <title>Hello, World!</title>
    </head>
    <body>
        <div class="container">
            <h1>Hello, World!</h1>
        </div>
        <script src="hello.js"></script>
    </body>
</html>
```

然而，如果我们通过 HTTP 提供此目录，打开浏览器并打开 JavaScript 控制台，你会看到与图 6-1 非常相似的内容。

![](img/16b7990eb0dbf589db177bdbd2f4a189_127_0.png)

图 6-1. 你被告知不可能的 Hello, World。

如果你在文件 hello.wasm 上使用 wasm-objdump 命令，你会注意到有一个导出的 main() 函数。生成的代码已经超出了我们能够展示整个文件的能力，因此我将只突出显示导出部分。

```
...
Export[13]:
  - memory[0] -> "memory"
  - func[3] <__wasm_call_ctors> -> "__wasm_call_ctors"
  - func[5] <main> -> "main"
  - func[6] <__errno_location> -> "__errno_location"
  - func[50] <fflush> -> "fflush"
  - func[47] <stackSave> -> "stackSave"
  - func[48] <stackRestore> -> "stackRestore"
  - func[49] <stackAlloc> -> "stackAlloc"
  - func[44] <emscripten_stack_init> -> "emscripten_stack_init"
  - func[45] <emscripten_stack_get_free> -> "emscripten_stack_get_free"
  - func[46] <emscripten_stack_get_end> -> "emscripten_stack_get_end"
  - table[0] -> "__indirect_function_table"
```

```
- func[53] <dynCall_jiji> -> "dynCall_jiji"
...
```

你会看到这里有相当多的生成的脚手架来使这一切工作。细节相当复杂，但如果你想追踪它，我建议使用 wasm2wat 生成相应的 Wat 文件。从那里，追踪 main() 函数（上面编号为 5）。你会看到类似示例 6-3 的内容。

## 示例 6-3. Wat 中的 main 方法。

```
...
(func (;5;) (type 5) (param i32 i32) (result i32)
  (local i32)
  call 4
  local.set 2
  local.get 2
  return)
...
```

最终你会回到生成的 JavaScript 文件。在其中有一个名为 fd_write 的函数，如示例 6-4 所示。这被添加到一个名为 wasi_snapshot_preview1 的命名空间中。正如名称所暗示的，这是一个预览，我们将在后面的讨论中介绍，但主要目的是 Emscripten 工具链正在生成代码来解决我们在前几章中看到的一些低级麻烦。我们将在第 10 章中 Rust 生态系统中发现类似的工具。

## 示例 6-4. printf 解决方案的一部分。

```
...
function _fd_write(fd, iov, iovcnt, pnum) {
  // hack to support printf in SYSCALLS_REQUIRE_FILESYSTEM=0
  var num = 0;
  for (var i = 0; i < iovcnt; i++) {
    var ptr = HEAP32[(((iov)+(i*8))>>2)];
    var len = HEAP32[(((iov)+(i*8 + 4))>>2)];
    for (var j = 0; j < len; j++) {
      SYSCALLS.printChar(fd, HEAPU8[ptr+j]);
    }
    num += len;
  }
  HEAP32[((pnum)>>2)] = num
  return 0;
```

}
...

完全没有必要深入细节来了解所有这些是如何工作的。重要的是你要理解，我们实际上并不是以典型的标准库意义上调用 printf()，而是该函数已被重写为调用生成的代码。在浏览器中，它会将字符路由到与开发者工具关联的 JavaScript 控制台。在 Node.js 环境中，它会被路由到底层系统控制台。在这个阶段重要的是，我们的遗留应用程序不需要更改即可在此新环境中运行，但我们也不必担心直接运行原生 C 和 C++ 的可怕前景。我们在可移植性、安全性和性能之间取得了关键平衡，这正是 WebAssembly 的全部意义所在。

生成的代码在 JavaScript 中有一个 Module 对象，它定义了我们的 WebAssembly 代码将占据的运行时环境。JavaScript 文件顶部有注释描述了该对象及其作为两个世界之间接口的作用。为了保持可管理性，我们将专注于其中的一个更小部分。

我们可用的选项之一是使用编译器指令来打开或抑制某些生成的行为。例如，我们可能不希望我们的 C 程序在 JavaScript 代码加载时立即运行。如果你尝试在不使用 INVOKE_RUN=0 指令的情况下编译，你会看到与上面相同的典型问候。在下面的片段中，注意当代码在 Node.js 中加载时，命令行上没有打印任何内容。

```
brian@tweezer ~/g/w/s/ch06> emcc hello.c -o hello.js -s INVOKE_RUN=0
brian@tweezer ~/g/w/s/ch06> node hello.js
brian@tweezer ~/g/w/s/ch06>
```

显然，如果你抑制了自动执行，你将希望能够指示应用程序何时可以执行。这可以用另一个指令完成。

```
brian@tweezer ~/g/w/s/ch06> emcc hello.c -o hello.js ↵
-s INVOKE_RUN=0 -s EXTRA_EXPORTED_RUNTIME_METHODS="
['callMain']"
```

在示例 6-5 中，你可以看到我们在按钮点击时调用 main() 函数。

## 示例 6-5. 延迟的 main() 方法调用

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="bootstrap.min.css">
    <title>Hello, World!</title>
  </head>
  <body>
    <div class="container">
      <h1>Hello, World!</h1>
      <button id="press">Press Me</button>
    </div>
    <script src="hello.js"></script>
    <script>
      var button = document.getElementById("press");
      button.onclick = function() {
        try {
          Module.callMain();
        } catch(re) {
        }
      };
    </script>
  </body>
</html>
```

在图 6-2 中，友好的消息在按钮被按下时打印到控制台。Firefox 不会显示每个相同的消息，但它显示我已在右侧按下了按钮七次。你的浏览器可能在每次调用时显示一条打印消息。

![](img/16b7990eb0dbf589db177bdbd2f4a189_131_0.png)

图 6-2. 按下按钮触发的 Hello, World

## 移植第三方代码

我们现在将深入研究将一些现有代码带入浏览器。这段代码从未打算在网络上运行，并且做了你通常不期望在浏览器中工作的事情，例如写入文件系统。不要担心，我们不会破坏浏览器安全模型，但你会看到这段 C++ 代码如何基本未经修改即可运行。

Emscripten 有大量的选项可以将第三方代码移植到 WebAssembly。有效地有 cc、make 和 configure 的即插即用替代品，这通常使移植过程变得微不足道。实际上，你可能需要处理遇到的各种问题，但你可能会惊讶于过程的简易性。项目的网站³ 有很好的文档来帮助你。我的最爱介绍这个主题的资料是 Robert Aboukhalil 的精彩 Level Up With WebAssembly⁴ 资料。他带你逐步移植几个不同的开源项目到 WebAssembly 以在浏览器中运行

它们包括 Tetris、Pong 和 Pacman 等游戏。与其尝试重现他已经出色完成的工作，我将专注于一个相对简单且干净的项目。

我花了一些时间寻找合适的候选代码。我想要一些有分量但不过于庞大的东西。最终我找到了 Arash Partow 在其网站 https://www.partow.net 上收集的优雅、干净、许可合适且有用的 C++ 代码。如果你去那里，你会发现很多有趣的材料。我最初打算使用计算几何库，但决定 Bitmap 库更适合像这样的书。

首先，从 http://www.partow.net/programming/bitmap/index.html 获取代码。一旦你下载 Zip 文件并解压缩，你会看到三个文件。Makefile 是一个老式的 Unix 构建文件，其中包含组装相关软件的说明。我们稍后会探索该过程。bitmap_image.hpp 文件是主要库，而 bitmap_test.cpp 是一个全面的测试集合，生成大量有趣的 Windows 位图图像。此代码不需要任何平台特定的库。

```
brian@tweezer ~/g/w/s/c/bitmap> ls -alF
total 536
drwxr-xr-x@  5 brian  staff       160 Dec 31  1999 ./
drwxr-xr-x  11 brian  staff       352 Mar  6 13:56 ../
-rw-r--r--@  1 brian  staff       770 Dec 31  1999 Makefile
-rw-r--r--@  1 brian  staff    247721 Dec 31  1999 bitmap_image.hpp
-rw-r--r--@  1 brian  staff     20479 Dec 31  1999 bitmap_test.cpp
```

我已为节省空间删除了示例 6-6 中的一些注释和许可细节。剩下的内容是构建测试应用 bitmap_test 的规则结构。Makefile 通过建立目标、依赖项以及构建目标的规则来工作。作为惯例，通常有一个 all 规则，它指定上面提到的目标文件名。它依赖于 .cpp 和 .hpp 文件。如果任一文件被修改，我们的可执行文件需要重建。为此，make 工具将执行文件

