

![](img/16b7990eb0dbf589db177bdbd2f4a189_386_0.png)

Figure 17-6. SwiftUI application running in the browser

图 17-6. 在浏览器中运行的 SwiftUI 应用

There are many other projects involving Swift and WebAssembly, but one more I wanted to highlight was the “Swift, Wasm, and Algorithms” project shown in Figure 17-7. This represents an interactive version of the algorithms Apple has added support for at the Swift and Algorithms repo (https://github.com/apple/swift-algorithms). This open-source package of algorithms focuses on generating sequences, combinations, permutations, and more from collections classes.

涉及 Swift 和 WebAssembly 的项目还有很多，但我还想重点介绍一下图 17-7 所示的“Swift, Wasm, and Algorithms”项目。这代表了 Apple 在 Swift and Algorithms 仓库 https://github.com/apple/swift-algorithms 中新增支持的算法的交互式版本。该开源算法包专注于从集合类生成序列、组合、排列等。

Figure 17-7. Swift, Wasm and Algorithms as interactive documentation

图 17-7. Swift、Wasm 和 Algorithms 作为交互式文档

The page shown above allows users to interactively experiment with the inputs and configurations for these algorithms which is a great way to learn how they work. By being able to use the Swift code directly, developers can see exactly how it will behave under different circumstances which is more useful than approximating the libraries by re-writing them in JavaScript.

上图所示的页面允许用户交互式地试验这些算法的输入和配置，这是一种了解它们工作原理的绝佳方式。通过能够直接使用 Swift 代码，开发人员可以准确地看到它在不同情况下的行为，这比通过用 JavaScript 重写来近似库更有用。

While the browser-based demos are fun and easy to show off, it will be more interesting to try our hand at more conventional programming. If you install the SwiftWasm toolchain as described in Appendix A, you should be able to run the following commands on macOS or Linux to verify that you have the WebAssembly-aware version of the Swift compiler installed.

虽然基于浏览器的演示很有趣且易于展示，但尝试更常规的编程会更有趣。如果按照附录 A 中所述安装 SwiftWasm 工具链，你应该能够在 macOS 或 Linux 上运行以下命令，以验证已安装 WebAssembly 感知的 Swift 编译器版本。

```
brian@tweezer ~> swift --version
SwiftWasm Swift version 5.3 (swiftlang-5.3.1)
Target: x86_64-apple-darwin20.6.0
```

In Example 17-5 we have our usual introductory program expressed in Swift.

在示例 17-5 中，我们用 Swift 表达了我们通常的入门程序。

### Example 17-5. Hello, world! in Swift

### 示例 17-5. Swift 中的 Hello, world！

```
print("Hello, world!")
```

We can generate a WASI-targeted version of the program as follows and notably run it in both Wasmer and Wasmtime which highlights how good the support is already.

我们可以通过以下方式生成该程序的 WASI 目标版本，并且值得注意的是，它可以在 Wasmer 和 Wasmtime 中运行，这凸显了目前支持的良好程度。

```
brian@tweezer ~/s/swift> swiftc -target wasm32-unknown-wasi
hello.swift ↵
  -o hello.wasm
brian@tweezer ~/s/swift> wasmer hello.wasm
Hello, world!
brian@tweezer ~/s/swift> wasmtime hello.wasm
Hello, world!
```

There are limitations to what is possible with this integration at the moment. Much of the functionality that the Swift standard library depends upon is not yet available because standard threading support in WebAssembly is still a moving target. Hopefully you can see that we are well on our way to having proper Swift support for WebAssembly though, both in and outside the browser.

目前这种集成存在一些限制。Swift 标准库所依赖的大部分功能尚未可用，因为 WebAssembly 中的标准线程支持仍在不断变化。希望你能看到，我们在为 WebAssembly 提供适当的 Swift 支持方面，无论是在浏览器内还是浏览器外，都已取得了长足的进展。

## Java

It seems inconceivable that Java is on this list of under-supported languages and runtimes, but, alas it is true. Java’s reliance on garbage collection and threads are among the biggest reasons this is still the case. As these proposals advance, things will obviously change but for now we have a limited number of options.

Java 似乎不可思议地出现在这个受支持程度不足的语言和运行时列表中，但不幸的是这是真的。Java 对垃圾回收和线程的依赖是导致这种情况持续存在的主要原因之一。随着这些提案的推进，情况显然会发生变化，但目前我们的选择有限。

The first option allows us to embed WebAssembly in Java with Wasmer as we have seen for other languages. That is clearly not the same thing as compiling Java to run on a WebAssembly platform, but it is a start. It depends on the Wasmer library and Java Native Interface (JNI) to load, but it is straightforward enough overall. The basic structure is shown in Example 17-6.

第一个选项允许我们使用 Wasmer 将 WebAssembly 嵌入到 Java 中，正如我们在其他语言中看到的那样。这显然与将 Java 编译为在 WebAssembly 平台上运行不同，但这是一个开始。它依赖于 Wasmer 库和 Java Native Interface (JNI) 来加载，但总体上足够简单。基本结构如示例 17-6 所示。

### Example 17-6. Our howOld function being called from Java via Wasmer

### 示例 17-6. 通过 Wasmer 从 Java 调用我们的 howOld 函数

```
import org.wasmer.Instance;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

class HowOldExample {
    public static void main(String[] args) throws IOException {
        byte[] bytes = Files.readAllBytes(Paths.get("howold.wasm"));
        Instance instance = new Instance(bytes);

        Function howOld = instance.exports.getFunction("howOld");
        Integer result = (Integer) howOld.apply(2021, 2000)[0];

        System.out.println("Result: " + result);

        instance.close();
    }
}
```

There is nothing substantially different from what we have seen so far with respect to the Wasmer API. We instantiate an instance of a WebAssembly module, retrieve a Function instance and invoke it with our parameters. In the wasmer-java/examples directory¹⁰ there are additional demonstrations on how to interact with exported Memory instances.

就 Wasmer API 而言，与我们迄今为止看到的内容没有实质性区别。我们实例化一个 WebAssembly 模块的实例，检索 Function 实例并使用我们的参数调用它。在 wasmer-java/examples 目录¹⁰ 中，有更多关于如何与导出的 Memory 实例交互的演示。

The Bytecode Alliance does not presently maintain Java APIs for interacting with the WasmTime runtime, but there are community-supported versions available here: https://github.com/kawamuray/wasmtime-java and here: https://github.com/bluejekyll/wasmtime-java. They behave very much like the Wasmtime APIs we have seen in Rust and .NET in previous chapters.

Bytecode Alliance 目前并未维护用于与 WasmTime 运行时交互的 Java API，但这里有社区支持的版本：https://github.com/kawamuray/wasmtime-java 和这里：https://github.com/bluejekyll/wasmtime-java。它们的行为与我们在前几章中在 Rust 和 .NET 中看到的 Wasmtime API 非常相似。

Another similar option is to use GraalVM, a high-performance JDK distribution that provides support for polyglot development and near native performance to the Java world. With additional support it is possible to run Python, Ruby, R, JavaScript, WebAssembly, and LLVM-based languages through the LLVM JIT engine. Not only is it possible to write these languages, it is also possible to have them interoperate. There is a similar API from the GraalVM community for instantiating WebAssembly modules and invoking them from Java which is substantively equivalent to what we just did. Given the polyglot interoperability, I would concede that it was a step closer to supporting Java and WebAssembly more fully, but it is still not a full solution. Until there is proper, standardized garbage collection and thread support in the WebAssembly ecosystem, it is not going to be straightforward to run Java in this way.

另一个类似的选项是使用 GraalVM，这是一个高性能的 JDK 发行版，为 Java 世界提供多语言开发支持和接近原生的性能。在额外支持下，可以通过 LLVM JIT 引擎运行 Python、Ruby、R、JavaScript、WebAssembly 和基于 LLVM 的语言。不仅可以编写这些语言，还可以让它们相互协作。GraalVM 社区提供了一个类似的 API，用于实例化 WebAssembly 模块并从 Java 调用它们，这在实质上等同于我们刚才所做的。鉴于多语言互操作性，我承认这更接近于更全面地支持 Java 和 WebAssembly，但它仍然不是完整的解决方案。在 WebAssembly 生态系统中实现适当的标准化垃圾回收和线程支持之前，以这种方式运行 Java 并不会很简单。

Once this does happen, and we see additional improvements in the WebAssembly run times from a performance and optimization perspective, then we will start to see organizations question the need for a JVM and a WebAssembly engine in production. Java itself will not go away, but I can see a scenario in the future where people are fine with a single runtime of co-mingled software whether it is using GraalVM or a WebAssembly engine.

一旦这种情况发生，并且我们从性能和优化的角度看到 WebAssembly 运行时有额外的改进，那么我们将开始看到组织质疑在生产环境中需要 JVM 和 WebAssembly 引擎的需求。Java 本身不会消失，但我可以看到未来的一种场景，人们对使用 GraalVM 或 WebAssembly 引擎的单一混合软件运行时感到满意。

While Java is not a fully-supported language yet, it does not mean there is not a strategy toward adoption. The company Leaning Technologies has a commercial offering called CheerpJ¹¹ that has some impressive results. The solution is a combination of ahead-of-time compilation, a WebAssembly and JavaScript runtime and the ability to do dynamic compilation on the fly. I will let you investigate their offering on your own, but you can see the remarkable achievement of running the SwingSet3 demo in the browser in Figure 17-8. This will not represent the ultimate integration strategy from Java and WebAssembly, but if you have a need now to support legacy systems in the browser, it might be an option.

虽然 Java 尚未完全受支持，但这并不意味着没有采用策略。公司 Leaning Technologies 有一个名为 CheerpJ¹¹ 的商业产品，其效果令人印象深刻。该解决方案结合了提前编译、WebAssembly 和 JavaScript 运行时以及动态即时编译的能力。我会让你自行研究他们的产品，但你可以在图 17-8 中看到在浏览器中运行 SwingSet3 演示的非凡成就。这不会代表 Java 和 WebAssembly 的最终集成策略，但如果你现在需要在浏览器中支持遗留系统，这可能是一个选择。

Figure 17-8. The SwingSet3 demo running in a browser via CheerpJ

图 17-8. 通过 CheerpJ 在浏览器中运行的 SwingSet3 演示

## Kotlin

Kotlin reminds me of the old Saturday Night Live ad parody for “Shimmer”, the combination floor wax and dessert topping¹². As a language and a runtime, there are so many things it can be, it feels like it falls into multiple categories simultaneously. It can be used to target applications on the JVM, it transpiles to JavaScript to run in the browser, it has a scripting side to its list of identities, and it can generate native applications on iOS and Android via an LLVM compiler.

Kotlin 让我想起了 Saturday Night Live 对“Shimmer”的旧广告恶搞，即地板蜡和甜点配料的组合¹²。作为一种语言和运行时，它可以做的事情太多了，感觉它同时属于多个类别。它可用于定位 JVM 上的应用程序，可转译为 JavaScript 在浏览器中运行，其身份列表中还有脚本方面，并且可通过 LLVM 编译器生成 iOS 和 Android 上的原生应用程序。

What it is, undoubtedly, is popular. Different organizations use it for different reasons, but it combines many different language features to create a concise and safe industrial-strength object-oriented programming language. It has been adopted as the preferred language for developing Android applications as well as a fully-supported language for super star open source projects such as Spring and Gradle.

它无疑是流行的。不同的组织出于不同的原因使用它，但它结合了许多不同的语言特性，创造出一种简洁且安全的工业级面向对象编程语言。它已被采用为开发 Android 应用程序的首选语言，也是 Spring 和 Gradle 等超级明星开源项目的完全支持的语言。

Along the way, they experimented with generating WebAssembly through its LLVM compiler. As of now, they are deprecating that approach for a more fully-supported backend to handle Kotlin to WebAssembly directly. This is definitely a work in progress as they were forming a new team to manage this and trying to staff it up in May of 2021.

在过程中，他们尝试过通过其 LLVM 编译器生成 WebAssembly。截至目前，他们正在弃用这种方法，转而采用更受支持的后端直接处理 Kotlin 到 WebAssembly 的转换。这绝对是一个正在进行的工作，因为他们在 2021 年 5 月组建了一个新团队来管理此事并试图配备人员。

You can still use the *kotlinc-native wasm32* backend to experiment if you like, but that is not going to be the long-term strategy. So, while there are great things in the plans for the pairing between these languages, we must take a wait-and-see approach for the time being.

如果你愿意，你仍然可以使用 *kotlinc-native wasm32* 后端进行实验，但这不会是长期策略。因此，虽然这些语言之间的配对计划中有很多很棒的东西，但目前我们必须采取观望态度。

## Zig

It will not surprise me to hear that you might have never heard of Zig, or if you have, only in passing. My favorite anecdote about it is that Jakub Konka, a well-respected software and researcher into algorithms and compiler theory learned Zig while waiting for a Rust compile to finish¹³. It is a funny story that gets to poke fun at the notoriously slow Rust compiler¹⁴ while also giving you a suggestive hint about what Zig brings to the table.

我不会感到惊讶，如果你从未听说过 Zig，或者如果你听说过，也只是偶然。关于它的我最喜欢的轶事是，备受尊敬的软件工程师和算法与编译器理论研究员 Jakub Konka 在等待 Rust 编译完成时学会了 Zig¹³。这是一个有趣的故事，既嘲笑了臭名昭著的缓慢 Rust 编译器¹⁴，也暗示了 Zig 带来的东西。

First of all, as indicated by the story, Zig is a simple language. Their website¹⁵ suggests that your time is better spent debugging your applications rather than your knowledge of your programming languages. Complex programming languages are hard to learn and hard to be effective with until you have mastered them over years. Zig has no hidden control flow, no hidden memory allocations, and neither a preprocessor, nor a macro system. Its entire syntax is captured in a 500 line parsing expression grammar file.

首先，正如故事所示，Zig 是一种简单的语言。他们的网站¹⁵ 建议你把时间花在调试应用程序上，而不是花在对编程语言的知识上。复杂的编程语言很难学习，直到你花几年时间掌握它们之前，很难有效地使用它们。Zig 没有隐藏的控制流，没有隐藏的内存分配，也没有预处理器，也没有宏系统。它的整个语法都包含在一个 500 行的解析表达式语法文件中。
