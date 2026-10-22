## 第 14 章. 使用 AssemblyScript 和 WebAssembly

### 早期发布读者的说明

使用 Early Release 电子书，你可以在这些书籍的正式发布之前很久就利用这些技术——作者在撰写时提供的原始未编辑内容——从而利用这些技术。

这将是最终书籍的第 14 章。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系作者 wasmbook@bosatsu.net。

> 今晚有些奇怪的事情正在发生
> 有些事情正在发生，感觉不太对劲
> Joey 很紧张，灯光很亮
> 有些事情正在发生，感觉不太对劲
> 
> —Wire

我越来越难以做出大众能理解的文化引用。你阅读本书的许多人可能不会认出这首歌，但引用经典仍然很重要。对于那些足够有文化¹ 能认出上述歌曲是 R.E.M. 专辑 “Document”² 上的歌曲的你们，可能会惊讶地得知这是一首翻唱。原曲由乐队 Wire 演唱。如果你听原版，你的反应可能会像歌曲所说那样，觉得有些奇怪正在发生。它既熟悉又不熟悉。

在本章中，我将介绍一种名为 AssemblyScript 的语言，这可能会引发同样的反应。它是一个基于 Binaryen 的编译器，将 TypeScript 的子集编译为 WebAssembly。它显然与 TypeScript 非常相似（而 TypeScript 又与 JavaScript 相似），但仔细检查后，这里也有些奇怪的事情在发生。尽管如此，它为 WebAssembly 提供了一种不同的用例，并展示了该平台可以有多么多才多艺。

TypeScript 是一种流行的 JavaScript 超集，它转译为普通 JavaScript，以便在任何 JavaScript 可运行的地方运行。其主要目的是使用强类型语言表达你的意图，以最大程度地减少充斥着平均 JavaScript 代码库的那种烦人且猖獗的类型错误。因此，即使它使用编译器，其目标更多是关于安全性而非性能。

如果你以前没有使用过强类型语言，这个想法可能看起来更像是一种负担而不是好处。公平地说，一些开发者确实有这种感觉。然而，专注于类型系统的语言定义了不同类型的含义和适用操作。编译器强制执行你只能在定义了这些操作的类型的实例上调用操作，从而避免运行时错误。因此，强类型系统帮助你生产出更正确、不可变、封装、可组合、并且通常更易读的软件。这并不是说你不能用弱类型系统做到同样的事情，只是你在尝试时获得的帮助较少。

TypeScript 由微软开发，是其在浏览器中交付办公应用套件和其他业务系统使用增加的回应。虽然单个 JavaScript 应用程序可以轻松开发，但一旦复杂、高性能的应用套件超过一定规模，就会带来挑战。这是尝试在仍留在充满活力的 JavaScript 生态系统内的同时解决这些增长问题的尝试。

作为一个非常简单的例子，示例 14-1 向我们展示了一个简单的 TypeScript 片段，定义了一个函数。请注意参数被类型化为



string. 习惯上，它的使用方式就像一段普通的 JavaScript。

#### Example 14-1. Basic TypeScript example.

// src/greeter.ts
function greeter(person: string) {
    return "Hello, " + person;
}
let user = "Mannequin Skywalker";
document.body.textContent = greeter(user);

浏览器和诸如 Node.js 和 Deno 之类的宿主环境并不会执行 TypeScript。编译器会将其转译³ 为普通的 JavaScript。

```
brian@tweezer ~/s/t/hello-ts> tsc src/greeter.ts
brian@tweezer ~/s/t/hello-ts> ls -alF src
total 32
drwxr-xr-x  6 brian  staff  192 Jan 14 17:17 ./
drwxr-xr-x  8 brian  staff  256 Jan 14 16:55 ../
-rw-r--r--  1 brian  staff  125 Jan 14 17:19 greeter.js
-rw-r--r--  1 brian  staff  135 Jan 14 17:19 greeter.ts
```

你需要仔细查看生成的 JavaScript 才能看出区别。在 Example 14-2 中可以看到，实际上发生的事情只是移除了类型。

#### Example 14-2. JavaScript generated from TypeScript

```
// src/greeter.js
function greeter(person) {
    return "Hello, " + person;
}
var user = "Mannequin Skywalker";
document.body.textContent = greeter(user);
```

那么，问题出在哪里？为什么我们要加上类型然后再把它去掉？添加类型的益处在于编译器可以捕获错误。通过确保代码不会被错误使用（在能力范围内），软件的整体正确性和质量应该会得到提升。我们在 Example 14-3 中看到的这种错误在 JavaScript 中要到运行时才会检测到。

#### Example 14-3. A type error in TypeScript

```
// src/greeter.ts
function greeter(person: string) {
  return "Hello, " + person;
}
let user = [0, 1, 2];
document.body.textContent = greeter(user);
```

TypeScript 编译器很容易就能在编译时阻止这种错误的发生，而这是在发现错误的好时机！

```
brian@tweezer ~/s/t/hello-ts> tsc src/greeter.ts
src/greeter.ts:7:37 - error TS2345: Argument of type 'number[]'
is not assignable to parameter
of type 'string'.
7 document.body.textContent = greeter(user);
                    ~~~~
Found 1 error.
```

更棒的是，TypeScript 感知的开发环境会在你输入代码时立即标记错误。微软的 VS Code 以及其他流行的 IDE 都为使用该语言的开发者辅助提供了非常不错的支持。

### “Definitely not a TypeScript to WebAssembly compiler”

本节标题直接引用自 AssemblyScript GitHub 页面。它是为明确其作用而做出的尝试，并说明不可能简单地将典型的 TypeScript 代码库编译为 WebAssembly。话虽如此，AssemblyScript 试图提供一种类似 JavaScript 的（更准确地说类似 TypeScript 的）体验，但同时具备 WebAssembly 提供的性能和安全性。它被广泛认为是一种在无需依赖 C、C++ 或 Rust 的情况下构建更高性能的 Web 应用的有用方式。对于现有的 Web 开发者来说，转向并从该平台受益应该是相对直接的过渡。

首先，AssemblyScript 并不是作为 JavaScript 运行，而是作为 WebAssembly 运行。编译器会生成 WebAssembly 模块，这些模块应像我们之前看到的其他模块一样有效工作。下面你会看到一些很酷的功能，例如在模块的 `Memory` 实例中分配内存、在 JavaScript 宿主环境和 Wasm 模块之间传递字符串等。

不幸的是，在基于 C 的章节中我尝试教你一些 C，在基于 Rust 的章节中教你一些 Rust，但我没有足够的篇幅来教你很多 TypeScript 然后告诉你 AssemblyScript 有何不同。因此，我假设你了解 TypeScript⁴，并在展示一些基本示例之前简要说明 AssemblyScript 的不同之处。

首先，虽然 TypeScript 强制执行类型，但它有 `any` 和 `undefined` 以更好地与未类型化的 JavaScript 兼容。AssemblyScript 不支持这两种类型。另一个区别是缺乏联合类型，这是我最喜欢的 TypeScript 功能之一。

由于 AssemblyScript 编译为 Wasm，它无法像任何其他模块一样对网络连接、文件系统访问、DOM 操作做任何事情。它们必须通过导入对象或通过 WASI 提供，正如你稍后会看到的那样。

AssemblyScript 与我们整本书中看到的 WebAssembly 指令集非常接近。数值类型基本上就是你所想象的那样，包括 `i32`、`i64`、`f32`、`f64` 和用于索引内存的 `usize`。由于 WebAssembly 的 MVP 版本支持 32 位寻址，因此目前它对应于 `i32` 类型。

此外，目前不支持实现接口、闭包、异常或类的访问修饰符（`public`、`private` 或 `protected`）。好消息是，其中大部分限制最终将被克服。其中一些可以直接通过 AssemblyScript 编译器的未来版本解决，而另一些则依赖于 WebAssembly 平台的演进。

还有其他差异，但与其再花时间关注它们的不同之处（网上有更好的文档资源），让我们看看积极的一面，看看 AssemblyScript 的实际效果。

### Simple Example

到现在为止，你不会对我们的第一个示例感到惊讶，那就是将两个数字相加。通常在 WebAssembly 中，这就是我们能展示的最简单的事情，然后事情就会变得奇怪。你将在 Example 14-4 中看到这样一个入门示例。

Example 14-4. The ubiquitous code to add two integers together

```
export function add(a: i32, b: i32): i32 {
  return a + b;
}
```

如果你已按照附录 A 中详细说明的方式安装了 AssemblyScript 编译器，那么你应该能够运行以下命令。

```
brian@tweezer ~/g/w/s/ch14> asc hello.ts -b hello.wasm
brian@tweezer ~/g/w/s/ch14> ls -alF
total 98560
drwxr-xr-x  7 brian  staff       224 Jul 18 15:39 ./
drwxr-xr-x 13 brian  staff       416 Jul 14 11:52 ../
-rw-r--r--  1 brian  staff        62 Jul 18 15:39 hello.ts
-rw-r--r--  1 brian  staff        91 Jul 18 15:39 hello.wasm
```

你应该会注意到 Wasm 模块非常小。显然目前还没有太多功能，但开销也非常小。调用我们的探索工具服务会显示还有什么在发生。

```
brian@tweezer ~/g/w/s/ch14> wasm-objdump -x hello.wasm

hello.wasm:     file format wasm 0x1

Section Details:
```

```
Type[1]:
  - type[0] (i32, i32) -> i32
Function[1]:
  - func[0] sig=0 <add>
Table[1]:
  - table[0] type=funcref initial=1
Memory[1]:
  - memory[0] pages: initial=0
Global[3]:
  - global[0] i32 mutable=0 - init i32=8
  - global[1] i32 mutable=1 - init i32=16392
  - global[2] i32 mutable=0 - init i32=16392
Export[2]:
  - func[0] <add> -> "add"
  - memory[0] -> "memory"
Elem[1]:
  - segment[0] flags=0 table=0 count=0 - init i32=1
Code[1]:
  - func[0] size=7 <add>
```

除非另行说明，否则 AssemblyScript 编译器会自动为你生成一个 Memory 实例并导出它。请注意，编译器也通过用 `export` 关键字修饰函数来导出我们的函数。这使得从 JavaScript 使用这些编译后的函数变得非常容易。

如果我们使用之前的那份简单的 utils.js 文件以及像 Example 14-5 那样的简单 HTML 骨架，我们应该会在 JavaScript 控制台中看到预期的结果。有很好的工具可以为 AssemblyScript 项目生成更精细的骨架，但目前我希望保持简单。

#### Example 14-5. Simple HTML to load our AssemblyScript-base Wasm module

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="bootstrap.min.css">
    <title>Basic AssemblyScript</title>
    <script src="utils.js"></script>
  </head>
  <body>
```


<script>
    fetchAndInstantiate('hello.wasm').then(function(instance) {
        console.log(instance.exports.add(12, 30));
    });
</script>
</body>
</html>

除了使从 Wasm 模块导出函数变得更容易之外，AssemblyScript 还使为模块导入可在其 JavaScript 环境中调用的功能变得容易。在示例 14-6 中，你可以看到我们使用 declare 关键字表达了导入名为 log() 的函数的意图。

#### 示例 14-6. 为 AssemblyScript 声明导入函数

```
declare function log(num: i32): void;

export function addAndLog(a: i32, b: i32): void {
  log(a + b);
}
```

你仍然必须像往常一样将预期的函数作为导入对象上的函数提供，但声明它的语法要简单得多。请注意示例 14-7 中的命名空间约定。

#### 示例 14-7. 提供我们预期导入函数的 HTML

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="bootstrap.min.css">
    <title>Importing JavaScript Functions to AssemblyScript</title>
    <script src="utils.js"></script>
  </head>
  <body>
    <script>
      var importObject = {
        hello: {
          log: value => console.log(value)
        }
      };

      fetchAndInstantiate('hello.wasm',
      importObject).then(function(instance) {
```

```
instance.exports.addAndLog(12, 30);
});
</script>
</body>
</html>
```

名为 *hello.wasm* 的 AssemblyScript 模块将期望在导入对象中提供 `hello` 命名空间。你可以通过查看模块在 `Import` 部分表达的内容来验证这一点。

```
brian@tweezer ~/g/w/s/c/hello-imp> wasm-objdump -x hello.wasm
hello.wasm:     file format wasm 0x1

Section Details:

Type[2]:
  - type[0] (i32) -> nil
  - type[1] (i32, i32) -> nil
Import[1]:
  - func[0] sig=0 <hello.log> <- hello.log
Function[1]:
  - func[1] sig=1 <addAndLog>
Table[1]:
  - table[0] type=funcref initial=1
Memory[1]:
  - memory[0] pages: initial=0
Global[3]:
  - global[0] i32 mutable=0 - init i32=8
  - global[1] i32 mutable=1 - init i32=16392
  - global[2] i32 mutable=0 - init i32=16392
Export[2]:
  - func[1] <addAndLog> -> "addAndLog"
  - memory[0] -> "memory"
Elem[1]:
  - segment[0] flags=0 table=0 count=0 - init i32=1
Code[1]:
  - func[1] size=9 <addAndLog>
```

AssemblyScript 编译器做的最后一件事是使使用模块的 `Memory` 实例变得相当容易⁵。正如我之前提到的，除非你告诉它不要，否则编译器会为你的模块创建一个 `Memory` 实例。在示例 14-8 中，我们通过一个页面扩展此内存，以确保

我们有 64K 的内存可供使用（尽管我们不需要接近这么多，但这是我们可以请求的最小大小）。

#### 示例 14-8. AssemblyScript 和 Memory 实例操作

```
memory.grow(1);

store<u8>(0, 100);

export function whereToStore(): i32 {
  let basePtr = load<u8>(0);
  return basePtr;
}

export function readFromLocation(loc: i32): i32 {
  let value = load<u8>(loc);
  return value;
}
```

在保证一些可用空间后，我们将一个位置写入零号位置。然后我们有两个函数导出到我们的 JavaScript 主环境。一个报告当前要写入的位置，而另一个让你能够从指定位置读取。方法 load<T>() 和 store<T>() 是围绕实际低级 WebAssembly 指令的便捷包装器，提供类似的行为。

正如你所知，并且很快会再次看到，我们可以直接操作内存缓冲区。我只是想向你展示两边的一些选项。更复杂的示例可能会提供将内存写入下一个可用位置并自动递增位置的能力。

在示例 14-9 中展示了从 JavaScript 使用此功能的一个示例。

#### 示例 14-9. 与 Memory 交互

```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>AssemblyScript and Wasm Memory</title>
    <script src="utils.js"></script>
  </head>
  <body>
    <script>
```

```
fetchAndInstantiate('mem.wasm').then(function(instance) {
  var mem = instance.exports.memory;
  var u8Arr = new Uint8Array(mem.buffer);

  let location = instance.exports.whereToStore();
  u8Arr[location] = 123;

  let value = instance.exports.readFromLocation(location);
  console.log("Round-tripped value: " + value);
});
</script>
</body>
</html>
```

我们确实用类型化数组包装了 Memory 实例的缓冲区。我们调用 whereToStore() 函数，然后在该位置写入我们的 Uint8Array。接下来我们将该位置传递给从模块导出的 readFromLocation() 函数，该函数加载存储在默认位置的值。令人高兴的是，当打印到控制台时，该值是我们期望的，“123”。

### 垃圾回收和 AssemblyScript 运行时

WebAssembly MVP 平台的一个大漏洞是垃圾回收能力。像 C/C++ 和 Rust 这样的语言不需要它，但其他更高层的语言需要它。有一个提案正在通过标准化流程⁶，但直到它通过平台可用为止，各种项目正在创建 shim 和附加支持。

如果你不熟悉垃圾回收的概念，它是在不再需要时自动清理已分配资源的机制。C 和 C++ 要求你调用 malloc() 和 free()（或 new() 和 delete()），因此该过程并没有真正自动化。Rust 有严格的内存所有权准则，如你在第 10 章中看到的那样，以便它知道值何时在词法上超出范围，然后可以清理它们。这两种方法代表了低开销但如果搞砸了就会很危险或强硬但安全的权衡。Python

使用引用计数来知道何时可以释放内存。其他语言如 Java 和 JavaScript 使用垃圾回收并通过各种复杂但耗时的算法跟踪内存引用。Java 作为嵌入式计算领域的一个小角色，部分原因在于这些算法可能不可预测地启动并阻止其他处理的发生。这对于用于医疗或工业安全目的的高精度设备来说是不可接受的。总是到处都有权衡。

AssemblyScript 从 0.18 版本开始添加了垃圾回收功能。我将在后面的部分通过 Loader 演示更方便的交互方式。但现在我只是想提到它，因为如果你告诉 AssemblyScript 编译器使用 `--exportRuntime` 指令导出对运行时的访问，你可以直接使用它。不仅如此，你还可以通过使用 `--runtime` 参数控制运行时的行为。这允许你指定它应使用增量、最小、存根甚至自定义可插拔实现。

我认为对该过程的深入介绍超出了本章的范围，但这取决于你尝试用 AssemblyScript 做什么可能会很有用⁷。

我确实想让你知道它的存在，并让你意识到可以从模块导出的 `Memory` 实例分配内存，以及固定和取消固定指向其中的引用，以便它们会被或不会被其他进程收集。我们将在下面看到使用这些能力的一些示例。

### AssemblyScript 标准库

AssemblyScript 生态系统中的另一个不错的补充是结构和功能的标准库，你可以从现代编程基础设施中期望。 正如我们所见，WebAssembly 要求这些常见函数由主机环境通过导入对象或更一般地通过 WASI 能力提供。

该库⁸ 具有数学函数、日期、字符串操作等的支持。

作为一个简单的例子，我们将构建一组用于根据其半径计算圆的细节的函数。在示例 14-10 中，我们有三个导出函数用于计算直径（2r）、周长（2πr）和面积（πr²）⁹。因为 AssemblyScript 默认使用 f64 数字，所以我们在从函数返回之前将它们转换为 f32 数字。

#### 示例 14-10. 使用标准库的 AssemblyScript

```
export function diameter(radius: f32): f32 {
    let diam = <f32>(2.0 * radius);
    return diam;
}

export function circumference(radius: f32): f32 {
    let circ = <f32>(2.0 * Math.PI * radius);
    return circ;
}

export function area(radius: f32): f32 {
    let area = <f32>(Math.PI * Math.pow(radius, 2));
    return area;
}
```

在示例 14-11 中，我们实例化我们的模块并调用该功能。虽然显然更快地直接在 JavaScript 中执行这种简单的数学运算，但更复杂的计算从在模块定义中提供此功能中受益，因此我们不必在 AssemblyScript 算法内部手动请求此类功能。

#### 示例 14-11. 从 JavaScript 调用使用标准库的 AssemblyScript

```
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>AssemblyScript Standard Library</title>
        <script src="utils.js"></script>
    </head>
    <body>
        <script>
            fetchAndInstantiate('stdlib.wasm').then(function(instance) {


```
let diameter = instance.exports.diameter(2.0);
console.log("Diameter of a circle for a radius 2.0: " +
diameter);
let circumference = instance.exports.circumference(2.0);
console.log("Circumference of a circle for a radius of 2.0:
" + circumference);
let area = instance.exports.area(2.0);
console.log("Area of a circle for a radius of 2.0: " + area);
});
</script>
</body>
</html>
```

如果一切如预期，当你通过 HTTP 将 HTML 提供给浏览器时，你应该会在 JavaScript 控制台中看到以下输出。

```
Diameter of a circle for a radius 2.0: 4
Circumference of a circle for a radius of 2.0: 12.566370964050293
Area of a circle for a radius of 2.0: 12.566370964050293
```

### AssemblyScript Loader

为了超越基础，我们将依赖 AssemblyScript Examples GitHub 仓库中的一个现有示例。它提供了我希望收尾的许多讨论要点，我认为直接以它为接下来几个示例的基础再合适不过。

首先，克隆该仓库。我们将重点关注 loader 目录，但请查看其他示例，其中包括图形的有趣用法、N 体模拟器等。

```
brian@tweezer ~/git-others> git clone https://github.com/AssemblyScript/examples.git as-examples
Cloning into 'as-examples'...
remote: Enumerating objects: 261, done.
remote: Counting objects: 100% (261/261), done.
remote: Compressing objects: 100% (163/163), done.
remote: Total 261 (delta 110), reused 215 (delta 72), pack-reused 0
Receiving objects: 100% (261/261), 553.00 KiB | 3.87 MiB/s, done.
Resolving deltas: 100% (110/110), done.
brian@tweezer ~/git-others> cd as-examples/
brian@tweezer ~/g/as-examples> ls
LICENSE        game-of-life interference loader        n-body
transform
README.md    i64            libm            mandelbrot    sdk
brian@tweezer ~/g/as-examples> cd loader
```

进入 *loader* 目录后，安装依赖项，然后运行 *asbuild* 脚本（在 *packages.json* 文件中定义）。这将为我们要关注的 AssemblyScript 代码创建经过优化和未优化的可调试版本，并支持运行时（因为使用了 --exportRuntime 指令）。

```
brian@tweezer ~/g/a/loader> npm install
added 4 packages, and audited 5 packages in 1s
1 package is looking for funding
  run `npm fund` for details
found 0 vulnerabilities
brian@tweezer ~/g/a/loader> npm run asbuild
> @assemblyscript/loader-example@1.0.0 asbuild
> npm run asbuild:untouched && npm run asbuild:optimized
> @assemblyscript/loader-example@1.0.0 asbuild:untouched
> asc assembly/index.ts -b build/untouched.wasm -t build/untouched.wat --exportRuntime --sourceMap --debug
> @assemblyscript/loader-example@1.0.0 asbuild:optimized
> asc assembly/index.ts -b build/optimized.wasm -t build/optimized.wat --exportRuntime --sourceMap --optimize
```

AssemblyScript 代码位于 *assembly/index.ts* 文件中。为了便于管理，我只会向你展示一些重要的片段。示例 14-12 向我们展示了一种令人惊讶地简单的方法，在 WebAssembly 和 JavaScript 主机环境之间共享字符串。我们不会在浏览器中运行，但也有类似的机制可以实现这一点。

#### Example 14-12. AssemblyScript that uses WebAssembly strings

```
// see: tests/index.js "Test for Example 1"

export function getHello(): string {
    return "Hello world (I am a WebAssembly string)";
}
```

暂停一下，考虑一下发生了什么。AssemblyScript 将字符串定义为字面量。该字符串将在 WebAssembly 线性内存中分配并初始化。我们在第 4 章中苦苦思索的那些细节此时似乎已成为遥远的记忆。

示例 14-13 取自 test/index.js。在此文件顶部，我们加载模块，然后提取导出的运行时元素。这包括初始化字符串和数组的能力，固定指针以防止其被垃圾回收，以及在两者之间传递值。我在之前关于垃圾回收的讨论中暗示了这种能力。

#### Example 14-13. Setting up the use of WebAssembly strings in JavaScript

```
// Load the node module exporting our WebAssembly module
const myModule = require("../index");

// Obtain the runtime helpers for
const {
    // memory management
    __newString, __newArray,
    // garbage collection
    __pin, __unpin,
    // and interop
    __getString, __getArray, __getArrayView
} = myModule;
```

示例 14-14 最后代表了第一个测试。从我们的模块 getHello() 方法返回的字符串将分配在我们模块的 Memory 实例中。该字符串在内存中的位置由运行时管理，但我们可以将其固定以防止其被垃圾回收。__getString() 方法将通过包装底层缓冲区并将字符串转换为可用的 JavaScript 字符串的动作。

#### Example 14-14. Using the exported runtime for working with WebAssembly strings

```
// Test for Example 1: Passing a string from WebAssembly to JavaScript.
{
    console.log("Example 1:");

    // Obtain a pointer to our string in the module's memory.
    const ptr = __pin(myModule.getHello());

    // Print its contents
    console.log("  " + __getString(ptr));

    __unpin(ptr); // it is ok if the string becomes garbage collected now
}
```

如果你有强健的胃口和大胆的精神，可以通过查看文件 node_modules/@assemblyscript/loader/index.js 来研究 loader 的实现细节，包括此函数。它可能会让你惊讶，但鉴于我们迄今为止在本书中所走的路，你应该对正在发生的事情感到相当自在。

#### Example 14-15. Window into the details of the AssemblyScript loader

...

```
/** Reads a string from the module's memory by its pointer. */
function __getString(ptr) {
    if (!ptr) return null;
    const buffer = memory.buffer;
    const id = new Uint32Array(buffer)[ptr + ID_OFFSET >>> 2];
    if (id !== STRING_ID) throw Error(`not a string: ${ptr}`);
    return getStringImpl(buffer, ptr);
}

extendedExports.__getString = __getString;
```

...

AssemblyScript 通过像 getStringImpl() 这样的函数将这些字符串解码为 UTF-16 字符串，该函数位于 index.js 文件中 __getString() 方法的稍上方。它反过来使用 AssemblyScript 标准库的字符串操作元素。

#### Example 14-16. Decoding Memory references using UTF-16 and AssemblyScript’s Standard Library

...
/** Gets a string from memory. */
function getStringImpl(buffer, ptr) {
    let len = new Uint32Array(buffer)[ptr + SIZE_OFFSET >>> 2] >>> 1;
    const utf16 = new Uint16Array(buffer, ptr, len);
    if (len <= STRING_SMALLSIZE) return
String.fromCharCode(...utf16);
    try {
        return utf16.decode(utf16);
    } catch {
        let str = "", off = 0;
        while (len - off > STRING_CHUNKSIZE) {
            str += String.fromCharCode(...utf16.subarray(off, off += STRING_CHUNKSIZE));
        }
        return str + String.fromCharCode(...utf16.subarray(off));
    }
}
...
```

我不会向你展示所有细节，但我想让你了解幕后发生的一些情况。首先，你已经接触过这些细节中的许多。其次，AssemblyScript、编译器和 Loader 为你屏蔽了大量这种复杂性，让你只需专注于代码的表达。WebAssembly 有一些相当原始的指令，但依赖高级工具可以让我们不必处理我们在早期章节中开始的那些烦人的细节。

如果你运行测试，你将在输出中看到来自示例 14-14 的第一个输出。测试 2 做类似的事情，但方向相反，将 JavaScript 字符串传递进去以便将其转换为 WebAssembly 字符串。文件 myConsole.ts 中还有一个巧妙的技巧，允许我们无缝地将日志记录回 JavaScript 控制台。我会让你自己深入研究细节。

```
Example 1:
    Hello world (I am a WebAssembly string)
```

其他测试和示例对数组和类执行类似的往返操作。总体而言，这些示例突出了 AssemblyScript 所填补的甜蜜点。





没有单一的使用场景能让 WebAssembly 有意义。我们已经谈到过为了性能原因重用现有库，以及使用 Rust 或 C 以可移植的方式编写底层代码。

如果你职业生涯的大部分时间都是 JavaScript 开发者，那么突然去学习这种语言无疑是一项艰巨的任务。AssemblyScript 带来了一种有点奇怪但仍然更熟悉、你很大程度上已经了解的语言。它允许你将其编译成性能可能更高的代码。这确实是一个非常不错的额外使用场景。

- 1 也称为老旧。
- 2 说真的，这是一张经典专辑。
- 3 转译是将一个源代码库翻译成另一个源代码库的过程，而不是二进制可执行文件（编译）。你可以在这里阅读更多相关内容：https://en.wikipedia.org/wiki/Source-to-source_compiler
- 4 你可以在他们的网站上找到许多很棒的教程和演练来学习 TypeScript：https://www.typescriptlang.org
- 5 这个示例和其他示例受到了 Aaron Turner (@torch2424) 的网站 https://wasmbyexample.dev/home.en-us.html 的启发，他是一位才华横溢、慷慨的开发者，也是个 badass DJ。去看看吧。
- 6 该提案需要先完成第 12 章中指出的其他几个提案，但你可以在这里阅读更多相关内容：https://github.com/WebAssembly/gc/blob/master/proposals/gc/Overview.md。
- 7 你可以在这里找到有关 AssemblyScript 垃圾回收支持提供的更多详细信息：https://www.assemblyscript.org/garbage-collection.html#runtime-interface
- 8 你可以在这里找到有关 AssemblyScript 标准库的更多详细信息：https://www.assemblyscript.org/stdlib/globals.html
- 9 虽然人们普遍认为 π 是方的，但事实并非如此。π 是圆的。玉米面包是方的。
