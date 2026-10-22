

你可以在图 9-1 中看到 API 设计的基本结构。

![](img/16b7990eb0dbf589db177bdbd2f4a189_188_0.png)

图 9-1. Tensorflow.js 分层 API 和后端

这里有很多内容需要拆解。首先，低层的算子通过上图所示的 Ops API 暴露出来。需要深入细节的开发者可以选择在该层进入 API。然而，在其之上，还有一个受 Keras 启发的 Layers API，其设计考虑了愉悦的开发者体验。这是一个用于使用简化的堆叠层方法定义复杂神经网络架构的简单 API。它隐藏了许多细节，但仍通过相对直接的机制暴露出强大的行为。

无论客户端代码使用哪种 API 进入 Tensorflow.js API，针对这些 API 编写的应用程序都可以在后端覆盖的所有环境中移植。这包括在浏览器中、在浏览器之外、带硬件加速和不带硬件加速的情况。这本身就是一个相当了不起的设计成就，但当能够利用硬件加速时，代码在不同环境中也会运行得非常出色。如我们所见，这可以有多种形式，但我们并不是针对最低公分母环境编写代码，应用程序代码也不必因一堆功能检测代码而负担沉重，以查看环境提供了什么。

使用 MobileNet⁷ 进行单次推理的结果，平均一百分次运行，结果显示在表 9-1 中。这些数据取自向世界介绍 Tensorflow.js 的论文《Tensorflow.js : Machine Learning for the Web and Beyond⁸》。

## 表 9-1. Tensorflow.js

## 性能结果

| Backend | Time(ms) | Speedup |
|---|---|---|
| CPU JavaScript | 3426 | 1x |
| WebGL (Intel Iris Pro) | 49 | 71x |
| WebGL (GTX 1080) | 5 | 685x |
| Node.js CPU w/ AVX2 | 87 | 39x |
| Node.js CUDA (GTX 1080) | 3 | 1105x |

趋势上的结果并不令人惊讶，但具体细节有点令人惊讶。在没有任何硬件加速的原始 JavaScript 后端上运行需要近 3.5 秒。只需将后端切换为受 WebGL 支持优化的后端，即使是性能非常适中的集成 GPU，也能在浏览器中实现 71 倍的加速。在配备更强 GPU 的桌面机器上，我们实现了近 700 倍的加速。没有 GPU 优势但拥有带优化 AVX 指令的 CPU 的服务器应用程序改进了近 40 倍。同一环境在更强大的 GPU 支持下产生了令人瞠目结舌的 1000 倍以上的加速。请记住，应用程序本身没有任何变化，只是运行它的环境发生了变化。

这就是我的主要观点。从“免费午餐”时期到多核时期的转变迫使开发者必须以显著不同的方式编程才能利用这些额外资源。如果我们必须改变编程方式才能利用定制加速、多 CPU、云托管环境以及在浏览器中运行，我们的深度学习应用程序将变成一团无法维护的代码。相反，我们看到的是一种隔离变化部分并利用我们可用的硬件选项的策略。这引出了我们将要考虑的最后一个后端。

## WebAssembly 后端

Tensorflow.js 发布后不久，团队发布了一个用 WebAssembly 编写的新的后端。鉴于我们刚刚讨论的覆盖范围，你可能会惊讶他们为何觉得有必要这样做。然而，到现在我希望你意识到这可能并不意味着从零开始。显然需要做一些工作，但大量工作是通过依赖现有代码来处理的。特别是，XNNPack⁹ 库被扩展以支持 WebAssembly 构建。

这将我们的后端集合扩展到包括图 9-2 中所示的后端。

![](img/16b7990eb0dbf589db177bdbd2f4a189_192_0.png)

图 9-2. Tensorflow.js 的 WebAssembly 后端

这突出了 WebAssembly 的一个主要用例。它并不是要取代 JavaScript，至少不是在所有情况下。它是关于在不必等待浏览器厂商社区达成共识的情况下扩展浏览器中可能实现的功能。WebGPU¹⁰ 或类似技术正在通过标准化流程推进，但这可能需要数年时间。用 C++ 编写的现有库旨在优化并在许多平台上移植，现在就能将其中很大一部分能力带到浏览器中，而无需等待。

这将计算运行时扩展到包括没有强大 GPU 的旧设备。代码可以优化以在各种平台上仍然运行良好。此外，WebAssembly 设计者正在逐步采用我们将在后续章节中讨论的两个高级功能。这包括使用单指令多数据（SIMD）¹¹ 并行化以及使用多线程。

在图 9-3 中，我们与之前一样使用了 MobileNet。显然，仅 WebAssembly 后端本身的性能不如 WebGL 支持的后端。你被鼓励使用 Wasm 后端而不是纯 JavaScript 后端，因为它在任何地方都会表现得更好。取决于模型大小，由于 WebGL 执行的一些固定成本，Wasm 后端比 WebGL 后端更有意义。请注意，当线程和 SIMD 并行化被添加到 Wasm 组合中时，支持这些功能的平台的性能有了显著提升。

## MobileNet v2

![](img/16b7990eb0dbf589db177bdbd2f4a189_193_0.png)

图 9-3. Tensorflow.js 后端在各平台上的相对性能

在图 9-4 中，我们看到参数数量更少的另一个模型的结果。在这种情况下，Wasm 后端更有意义。请注意 Pixel 4 上的可比性能以及 Linux 和 Mac 笔记本上显著更好的性能。再次，线程和 SIMD 的使用显著改善了 WebGL 为基础的后端的性能。

## BlazeFace

![](img/16b7990eb0dbf589db177bdbd2f4a189_194_0.png)

图 9-4. Tensorflow.js 后端在各平台上的相对性能

至少目前，WebAssembly 并非在每种情况下都是最快的解决方案。它是一个相对较新的平台，要榨干其中所有潜在优化还需要一段时间。然而，鉴于其年轻的年龄，它已经改变了在各种计算环境中可能实现的功能。

本章的关键教训是，WebAssembly 及其相关技术可以为我们提供更多选择，并且在许多情况下，为在多样化的计算表面上部署高性能软件系统提供更好的选择。我们在接下来的内容中会看到许多其他用途，但现在是时候扩展我们的语言和 WebAssembly 选项了。

- 1 有关这个日益流行的努力的更多细节可以在这里找到：https://onnx.ai
- 2 这就是我在正文中提到的文章。在其中，他还引用了他写的一篇名为“免费午餐结束了”的文章，也值得你花时间阅读：https://herbsutter.com/welcome-to-the-jungle/
- 3 摩尔定律是对硅芯片制造趋势中密度方面一个著名的观察。https://en.wikipedia.org/wiki/Moore%27s_law
- 4 你可以在这里尝试 Tensorflow Playground：http://playground.tensorflow.org
- 5 直接操作是一种用户界面风格，用户拥有控件，可以安全地探索性地操作应用程序及其数据。
https://en.wikipedia.org/wiki/Direct_manipulation_interface
- 6 AVX/AVX2 是对 Intel x86 指令集架构（ISA）的扩展，提供了我们在此使用的那种数学的并行优化：
https://en.wikipedia.org/wiki/Advanced_Vector_Extensions
- 7 MobileNets 模型集合专为高效的移动和嵌入式视觉应用而设计：https://arxiv.org/pdf/1704.04861.pdf
- 8 这篇论文读起来不错，值得你花时间：https://arxiv.org/pdf/1901.05350.pdf
- 9 XNNPack 可在此处获得，尽管它并不打算直接供深度学习研究人员使用：https://github.com/google/XNNPACK
- 10 这个新兴标准将以跨平台的方式直接向浏览器公开 GPU 支持：https://en.wikipedia.org/wiki/WebGPU
- 11 SIMD 是一种数据并行处理类型，对数据的不同部分执行相同指令于多个计算元素上。https://en.wikipedia.org/wiki/SIMD

