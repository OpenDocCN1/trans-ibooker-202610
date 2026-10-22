## 第 1 章. 引言

### 早期发布读者的说明

凭借 Early Release 电子书，您可以在这些技术正式发布之前很久就获得书籍的最早形式——作者在写作时的原始未编辑内容——从而利用这些技术。

这是最终书籍的第 1 章。

如果您对我们如何改进本书的内容和/或示例有任何评论，或者如果您注意到本章中缺少材料，请联系作者 wasmbook@bosatsu.net。

>  extraordinary claims require extraordinary evidence.
> —Dr. Carl Sagan

本章将介绍 WebAssembly 并为其广泛的影响提供背景。在某种意义上，它是过去几十年网络演进的 culmination。要理解所有这些，有相当多的历史需要回顾。如果您不喜欢历史和阐述，您可以跳过本章直接进入第 2 章，但我希望您不要。我认为了解这项技术为何如此重要以及它从何而来是很重要的。

## WebAssembly 提供什么

工程师可以培养的最伟大技能之一是评估新技术带来的价值。正如北卡罗来纳大学的 Dr. Fred Brooks 提醒我们那样，没有“银弹”¹，一切都有取舍。复杂性往往不会随着新技术而消除，而只是被转移到其他地方。因此，当某件事确实以积极的方式改变了可能实现的事情或我们的工作方式时，它值得我们关注，我们应该尝试弄清楚原因。

在尝试理解某件新事的影响时，我通常会先尝试确定其背后的动机。另一个很好的洞察来源是替代方案在何处失败。在我们之前有什么？它如何影响我们试图解读的这项新技术？正如艺术和音乐一样，我们不断从多个来源借用好主意，因此要真正理解 WebAssembly 为何值得我们关注以及它提供了什么，我们必须首先看看它之前的东西以及它如何产生差异。

在正式向世界介绍 WebAssembly 的论文²中，作者指出，动机是为了满足现代、通过网络交付的软件的需求，而 JavaScript 单独无法满足。最终这是一项提供软件的探索：

- 安全
- 快速
- 可移植
- 紧凑

在这个愿景中，WebAssembly 位于软件开发、网络、其历史以及它如何在地理分布的空间中提供功能性的交汇点。随着时间的推移，这个想法已远远超出了这一起点，设想了一个无处不在的、安全、高性能的计算平台，触及我们作为技术人员专业生活的几乎每一个方面。WebAssembly 将影响客户端 Web 开发、桌面和企业应用程序、服务器端功能、遗留现代化、游戏、教育、云计算、移动平台、物联网生态系统、无服务器和微服务计划等世界。我希望在本书的进程中说服你这一点。

我们的部署平台比以往任何时候都更加多样化，因此我们在代码和应用层面上都需要可移植性。通用的指令集或字节码目标可以使算法在各种环境中工作，因为我们只需要将逻辑步骤映射到它们如何在特定机器架构上表达。程序员使用应用程序编程接口（API）如 OpenGL³、POSIX⁴ 或 Win32⁵，因为它们提供了打开文件、生成子进程或在屏幕上绘制事物的功能。它们很方便，减少了开发人员需要编写的代码量，但它们对提供功能的库的存在产生了依赖。如果目标环境中没有 API，应用程序将无法运行。这是微软能够利用其在操作系统市场中的优势在应用套件领域也占据主导地位的方式之一。另一方面，开放标准可以使软件更容易移植到不同的环境中。

我们正在构建的软件的运行时方面存在另一个问题，不同的主机具有不同的硬件能力（核心数量、GPU 的存在与否）或安全限制（是否可以打开文件或发送/接收网络流量）。软件通常通过使用特性检测方法来适应可用资源，以确定应用程序可以利用哪些资源，但这通常会使业务功能复杂化。我们根本无法负担不断为多个平台重写软件的时间和金钱。我们需要更好的重用策略。我们也需要在不修改代码以支持其运行的平台的情况下获得这种灵活性。为不同主机环境使代码不同会增加其复杂性并使测试和部署策略复杂化。

经过几十年的发展，开源软件的价值主张已经很明确。我们倾向于使用其他开发人员编写的有价值、可重用的组件作为满足我们自身需求的一种方式。然而，并非所有可用代码都是可信的，当我们执行从互联网下载的不受信任的位时，我们就会敞开软件供应链攻击的大门。我们变得容易受到风险、业务影响和个人


通过网络钓鱼攻击、数据泄露、恶意软件和勒索软件造成的不安全软件系统的成本。

到目前为止，JavaScript 一直是解决其中一些问题的唯一方案。当它在沙箱环境中运行时，它为我们提供了一定程度的安全性。它无处不在且可移植。引擎变得更快。生态系统爆发成生产力雪崩。然而，一旦你离开基于浏览器的保护的限制，我们仍然存在安全问题。客户端运行的 JavaScript 代码与服务器上运行的 JavaScript 之间存在差异。单线程设计使长时间运行或高度并发的任务变得复杂。由于其作为动态语言的起源，其他编程语言可用的几类优化，仍然无法作为选项提供给即使是最快和最现代的 JavaScript 运行时。

此外，添加 JavaScript 依赖项太容易了，而且没有意识到在传递过程中拖入了多少负担和风险。没有花时间仔细考虑这些决定的开发人员最终会给上游软件的测试、部署和使用带来负担。每个脚本都必须在通过网络传输后加载和验证。这减慢了使用时间，让一切感觉迟缓。当依赖包被修改或删除时，它有潜力破坏大量已部署的软件⁷。

业余观察者中存在一种看法，认为 WebAssembly 是对 JavaScript 的攻击，但事实并非如此。当然，如果你想的话，你可以避免使用 JavaScript，但它主要是为了给你提供选择，让你能够用你选择的语言解决问题，而无需单独的运行时，也无需关心另一段软件是用什么语言编写的。已经有可能在不知道它是如何构建的情况下使用 WebAssembly 模块。这将增加我们从软件中获得的业务价值的生命周期，同时也让我们能够在采用新语言时进行创新，而不会影响其他一切。

在过去几十年中，我们经历了多种工具、语言、平台和框架，它们试图解决这些问题，但 WebAssembly 代表了我们第一次做对其中之一的时候。它的设计者并没有试图过度规范任何东西。他们正在从过去学习，拥抱网络并应用问题空间。任何理解标准的软件都可以与任何其他同样理解标准的软件进行通信。这给了我们选择自由以及让任一方面独立于另一方演进的能力。

## JavaScript 的起源

网络的交互模型称为超文本传输协议（HTTP）。它基于用于交换基于文本的消息的受限动词集。虽然这是一个简单有效且易于实现的模型，但由于不断返回服务器所固有的延迟，它很快被认为不足以应对交互式现代应用的任务。能够将代码发送到浏览器的想法一直很有吸引力。如果它在用户交互的一侧运行，并非每个活动都需要返回服务器。这将使 Web 应用变得更加交互、响应迅速且令人愉快地使用。如何实现这一点却并不完全清楚。哪种编程语言最有意义？我们如何平衡表达能力与浅显的学习曲线，以便更多个人能够参与开发过程？哪些语言性能更好，我们又如何保护客户端上的敏感资源免受恶意软件侵害？

浏览器空间的大部分创新最初由 Netscape Communications Corp 推动。令人难以置信的是，Netscape 浏览器最初是一款付费软件，¹⁶但他们更大的兴趣在于销售服务器端软件。通过扩展客户端上可能实现的功能，他们可以创建并销售更强大、更有利可图的服务器功能。

当时，Java 正从作为消费设备嵌入式语言的起步中脱颖而出，但它还没有太多成功的记录。它作为一种简化版的 C++ 的有吸引力的想法，可以在虚拟平台上运行，因此本质上是跨平台的。作为一种设计用于在网络上下载运行软件的环境，它通过语言设计、沙箱容器和细粒度的权限模型内置了安全性。

在各种操作系统之间移植应用程序是一件棘手的事情，而无需这样做的前景引发了人们对软件开发未来将如何发展的狂热。Sun Microsystems 发现自己处于一个有利可图的位置，拥有解决完美风暴般问题和机遇的解决方案。鉴于这种潜力，正在进行将 Java 引入浏览器的讨论，但尚不清楚这笔交易会是什么样子或何时落地。

作为一种面向对象编程（OOP）语言，Java 包含线程和继承等复杂的语言特性。Netscape 担心这可能对非专业软件开发人员来说太难掌握，因此他们聘请 Brendan Eich 创建一种“用于浏览器的 Scheme¹⁷”¹⁸，设想一种更易用、轻量级的脚本语言。Brendan 在想要包含在语言中的内容方面拥有一些决策自由，但也面临着尽快完成的压力。交互式应用的语言被视为这一新兴平台向前迈出的关键一步，每个人都希望它昨天就已完成。正如 Sebastian Peyrott 在上面引用的博客文章中指出的那样，什么



出现了“Scheme 与 Self¹⁹ 的早产私生子，外表像 Java。”

最初浏览器中的 JavaScript 仅限于简单的交互，例如动态菜单、弹出对话框以及响应按钮点击。这些相比每次操作都要往返服务器而言是显著的进步，但与当时桌面和工作站机器上所能实现的功能相比，它仍然只是个玩具。

我在网络早期工作过的公司创建了第一个全地球可视化环境，涉及数 TB 的地形信息、高光谱图像以及从无人机视频中提取视频帧²⁰。这当然最初需要 Silicon Graphics 工作站，但最终在几年内就能在配备消费级图形处理单元（GPU）的 PC 上运行。当时在网络上根本不可能实现类似的东西，虽然，[content]

出现了“Scheme 与 Self¹⁹ 的早产私生子，外表像 Java。”

最初浏览器中的 JavaScript 仅限于简单的交互，例如动态菜单、弹出对话框以及响应按钮点击。这些相比每次操作都要往返服务器而言是显著的进步，但与当时桌面和工作站机器上所能实现的功能相比，它仍然只是个玩具。

我在网络早期工作过的公司创建了第一个全地球可视化环境，涉及数 TB 的地形信息、高光谱图像以及从无人机视频中提取视频帧²⁰。这当然最初需要 Silicon Graphics 工作站，但最终在几年内就能在配备消费级图形处理单元（GPU）的 PC 上运行。当时在网络上根本不可能实现类似的东西，虽然，得益于 WebAssembly，这已不再是事实²¹。

根本不会有人把真正的软件开发与网页开发混为一谈。我们已经注意到，虽然客户端与服务器之间的关注点分离带来的好处之一是客户端可以独立于服务器演进。在 Java 和 Java 企业模型主导后端的同时，JavaScript 在浏览器中演进，最终成为如今的主导力量。

## Web 平台的演进

随着 Java 小程序和 JavaScript 在 Netscape 浏览器中可用，开发者开始尝试动态页面、动画以及更复杂的用户界面组件。多年以来这些仍然只是玩具应用，但愿景很有吸引力，很容易想象它最终可能走向何方。

微软感到有必要跟上，但并不太感兴趣直接支持竞争对手的技术。他们（正确地）认为这种网页开发最终可能会动摇其操作系统的主导地位。

当他们发布带有脚本支持的 Internet Explorer 时，他们称之为 JScript 以避免法律问题，并逆向工程了 Netscape 的解释器。他们的版本支持与基于 Windows 的组件对象模型（COM）组件的交互，并有其他变化，使得在浏览器之间编写不兼容的脚本变得容易。他们最初对将 JavaScript 标准化为 ECMAScript 的努力的支持减弱了一段时间，最终浏览器大战²² 开始了。这对开发者来说是一个令人沮丧的时期，最终涉及美国政府对微软的反竞争诉讼。

随着 Netscape 的命运衰落，Internet Explorer 开始主导浏览器领域，跨平台创新在一段时间内减弱，即使 JavaScript 正在经历标准化过程。Java 小程序在某些圈子中被广泛使用，但它们运行在沙箱环境中，因此将其用作驱动动态网页活动的基石变得棘手。你当然可以使用 Sun 的图形和用户界面 API 来做富有成效且有趣的事情，但它们运行在与 HTML 文档对象模型（DOM）²³ 不同的内存空间中。它们不兼容且具有不同的编程和事件模型。沙箱元素与网页元素之间的用户界面看起来并不相同。总体而言这是一种完全不合适的开发体验。

其他不可移植的技术，如 ActiveX，在微软的网页开发领域变得流行。Macromedia 的 Flash 成为 Adobe 的 Flash，并在大约十年间拥有短暂但活跃的流行期。然而，所有这些次要选项都存在问题。内存空间彼此隔离，安全模型也不如人们希望的那么健壮。引擎是新的且不断开发，因此错误很常见。ActiveX 提供了代码签名保护，但没有沙箱，因此如果证书可以被伪造，就可能出现相当可怕的攻击。

Firefox 从 Mozilla 诞生，成为 Netscape 灰烬中一个可行的竞争对手。它和 Google 的 Chrome 最终成为 Internet Explorer 的合适替代品。每个阵营都有其拥护者，但人们对解决它们之间的不兼容性日益感兴趣。浏览器空间中选择的引入迫使每个厂商都更加努力地工作，以超越彼此，从而实现技术主导并吸引市场份额。

结果是 JavaScript 引擎变得显著更快。即使 HTML 4 在浏览器和平台之间仍然“古怪”且痛苦地使用，但开始有可能隔离这些差异。这些发展与在基于标准的环境结构内工作的愿望相结合，促使 Jesse James Garrett²⁴ 设想了一种不同的网页开发方法。他引入了 Ajax 一词，它代表了一组标准的组合：异步 JavaScript 和 XML。想法是让后端系统的数据流入前端应用，前端应用会动态响应新的输入。通过在操作 DOM 的层面工作，而不是拥有一个单独的、沙箱化的用户界面空间，浏览器可以在基于网络的客户端-服务器架构中成为通用应用消费者。

HTML 5 标准化进程也在此期间开始，目的是在浏览器之间提高一致性，引入新的输入元素和元数据模型，并提供硬件加速的 2D 图形和视频元素等其他功能。Ajax 风格的融合、ECMAScript 作为语言的标准化与成熟、更轻松的跨浏览器支持以及日益功能丰富的基于网络的环境，导致了活动和创新的爆炸。我们见证了无数 JavaScript 基础的应用框架来来去去，但就可能实现的内容而言，存在着稳步的前进势头。随着开发者不断突破极限，浏览器厂商会改进其引擎以允许极限被进一步突破。这是一个良性循环，开启了对安全、可移植、零安装软件系统潜力的新愿景。

随着其他障碍和限制被移除，这一切核心的奇怪小语言变得越来越成为前进动力的高惯性阻力。引擎正变得世界级的开发环境，具有更好的调试和性能分析工具。新的编程范式，如基于 Promise²⁵ 的风格，允许在 JavaScript 臭名昭著的单线程环境中实现更好的模块化和异步友好应用代码，以取得强大的成果。但语言本身无法实现 C 或 C++ 等其他语言中可能的优化类型。从语言性能角度来看，存在着简单的限制。

Web 平台标准继续推进，随着 WebGL²⁶ 和 WebRTC²⁷ 等技术的开发和采用。不幸的是，JavaScript 的性能限制使其不适合扩展浏览器以涉及低级网络、多线程代码以及图形和流式视频编解码器的功能。

平台的演进需要 W3C 成员组织痛苦地决定什么重要、设计和构建什么，然后在各种浏览器实现中推出。随着人们对使用网络作为更重量级、交互式应用平台的兴趣日益增长，这一过程被视为越来越不可取。所有东西要么必须用 JavaScript 编写（或重写），要么浏览器必须标准化行为和接口，而这可能需要数年时间才能实现新进展。

正是因为这些和其他原因，Google 开始考虑一种替代的安全、快速和可移植的客户端网页开发方法。

## Native Client (NaCl)

2011 年，Google 发布了一个名为 Native Client (NaCl) 的新开源项目。其想法是在浏览器中提供接近原生速度的代码执行，同时在受限权限沙箱中运行以确保安全。你可以把它想成有点像 ActiveX，但背后有一个真正的安全模型。该技术非常适合 Google 的一些更大目标，例如支持 ChromeOS 并将事物从桌面应用转移到网页应用。它最初并不一定意味着要扩展开放网络为所有人提供的能力。





The uses cases were mainly to support browser-based delivery of computationally-intensive software such as:

- games
- audio and video editing systems
- scientific computing and CAD systems
- simulations

The initial focus was on C and C++ as source languages, but because it was based upon the LLVM28 compiler toolchain, it would be possible to support additional languages that could generate the LLVM Intermediate Representation (IR)29. This will be a recurring theme in our transition to WebAssembly as you will see.

There were two forms of distributable code here. The first was the eponymous NaCl which resulted in “nexe” modules that would target a specific hardware architecture (e.g. ARM or x86-64) and could only be distributed through the Google Play store. The other was a portable form called PNaCl30 that would be expressed in LLVM’s Bitcode format making it target independent. These were called “pexe” modules and would need to be transformed into a native architecture in the client’s host environment.

The technology was successful in the sense that the performance demonstrated in browser was only minimally off of native execution speeds. By using software fault isolation (SFI) techniques, they created the ability to download high performance, secure code from the web and run it in browsers. Several popular games such as Quake and Doom were compiled to this format to show what was ultimately possible. The problem was that the NaCl binaries would need to be generated and maintained for each target platform and would only run in Chrome. They also ran in an out-of-process space so they could not directly interact with other Web APIs or JavaScript code.

While running in limited privilege sandboxes was achievable, it did require static validation of the binary files to ensure that they did not attempt to invoke operating system services directly. The generated code had to follow certain address boundary-alignment patterns to make sure they did not violate allocated memory spaces.

As indicated above, the PNaCl modules were more portable. The LLVM infrastructure could generate either the NaCl native code or the portable Bitcode without modifying the original source. This was a nice outcome, but there is a difference between code portability and application portability. Applications require the APIs that they rely upon to be available in order to work. Google provided an application binary interface (ABI) called the Pepper APIs31 for low-level services such as 3D graphics libraries, audio playback, file access (emulated over IndexedDB or LocalStorage) and more. While PNaCl modules could run in Chrome on different platforms because of LLVM, they could only run in browsers that provided suitable implementations of the Pepper APIs. While Mozilla had originally expressed interest in doing so, they eventually decided they wanted to try a different approach that came be known as asm.js. NaCl deserves a tremendous amount of credit for moving the industry in this direction, but it was ultimately too fiddly and too Chrome-specific to carry the open web forward. Mozilla’s attempt was more successful on that front even if it did not provide the same level of performance that the native client approach did.

## asm.js

The asm.js32 project was at least partially motivated by an attempt to bring a better gaming story to the web. This soon expanded to include a desire to allow arbitrary applications to be delivered securely to browser sandboxes without having to substantively modify the existing code.

As we have previously discussed, the browser ecosystem was already advancing to make 2D and 3D graphics, audio handling, hardware-accelerated video and more available in standards-based, cross-platform ways. The idea was that operating within that environment would allow applications to use any of those features which were defined to be invoked from JavaScript. The JavaScript engines were efficient and had robust sandboxed environments that had undergone significant security audits so no one felt like starting from scratch there. The real issue remained the inability to optimize JavaScript ahead-of-time (AoT) so runtime performance could be improved even further.

Because of its dynamic nature and lack of proper integer support, there were several performance obstacles that could not meaningfully be managed until the code was loaded into the browser. Once that happened, Just-in-Time (JIT) optimizing compilers are able to speed things up nicely, but there were still inherent issues like slow bounds-checked array references. While JavaScript in its entirety could not be optimized ahead-of-time, a subset of it could be.

The exact details of what that means are not super relevant to our historical narrative, but the end result is. asm.js also used the LLVM-based clang33 front-end parser via the Emscripten34 toolchain. Compiled C and C++ code is very optimizable ahead-of-time so the generated instructions can be made very fast through existing optimization passes. LLVM represents a clean, modular architecture so pieces of it can be replaced including the backend generation of machine code. In essence, the Emscripten team could reuse the first two stages (parsing and optimization) and then emit this subset of JavaScript as a custom backend. Because the output was all “just JavaScript”, it would be much more portable than the NaCl/PNaCl approach. The tradeoff unfortunately was in performance. It represented a significant improvement over straight JavaScript, but was not nearly as performant as Google’s approach. It was good enough to amaze developers though. Beyond the modest performance improvements, however, the mere fact that you could deploy existing C and C++ applications into a browser with reasonable performance and virtually no code changes was compelling. While there were extremely compelling demos involving the Unity engine35, let’s look at a simple example. “Hello, World!” seems like a good place to start.

```
#include <stdio.h>
int main() {
```

```
printf("Hello, world!\n");
return 0;
}
```

Notice there is nothing unusual about this version of the classic program. If you stored it in a file called hello.c, the emscripten toolchain would allow you to emit a file called a.out.js which can be run directly in Node.js36 or, via some scaffolding, in a browser.

```
brian@tweezer ~/s/w/ch01> emcc hello.c
brian@tweezer ~/s/w/ch01> node a.out.j
Hello, world!
```

Pretty cool, no?

There’s only one problem.

```
brian@tweezer ~/s/w/ch01> ls -lah a.out.js
-rw-r--r-- 1 brian staff 119K Aug 17 19:08 a.out.js
```

At 119 kilobytes that is an awfully large hello world program! A quick look at the native executable might give you a sense of what is going on.

```
brian@tweezer ~/s/w/ch01> clang hello.c
brian@tweezer ~/s/w/ch01> ls -lah a.out
-rw-xr-xr-x 1 brian staff 48K Aug 17 19:11 a.out
```

Why is our supposedly optimized JavaScript program close to three times bigger than the native version? It is not just because as a text-based file JavaScript is more verbose. Look at the program again:

```
#include <stdio.h> ①
int main() {
    printf("Hello, world!\n"); ②
    return 0;
}
```

① The header identifies the source for standard IO-related function definitions.

2 The reference to the printf() function will be satisfied by a dynamic library loaded at runtime.

If we look at the symbols defined in the compiled executable using nm, we will see that the definition of the printf() function is not contained in the binary37. It is marked “U” for “undefined”.

```
brian@tweezer ~/s/w/ch01> nm -a a.out
0000000100002008 d __dyld_private
0000000100000000 T __mh_execute_header
0000000100000f50 T _main
          U _printf
          U dyld_stub_binder
```

When clang generated the executable it left a placeholder reference to the function that it expects to be provided by the operating system. There is no standard library available in this way for a browser, at least not in the dynamically-loadable sense, so that library function and anything it needs also need to be provided. Additionally, this version cannot talk directly to the console in a browser, so it will need to be given hooks to call into a function such as the browser’s console.log() functionality. In order to work in the browser the functionality has to be shipped with the application which is why it ends up being so big.

This highlights nicely the difference between portable code and portable applications, another common theme in this book. For now, we can marvel that it works at all, but there is a reason this book is not called “asm.js : The Definitive Guide”. It was a remarkable stepping stone that demonstrated it was possible to generate reasonably performant sandboxed JavaScript code from various optimizable languages. The JavaScript itself could be optimized further as well in ways that the superset could not. By being able to generate this subset through LLVM-based toolchains and a custom backend, the level of effort was much smaller than it might otherwise have been.





asm.js 对于不支持 WebAssembly 标准的浏览器来说是一个不错的后备方案，但现在是时候为本书的主题做好铺垫了。

## WebAssembly 的崛起

有了 NaCl，我们找到了提供沙箱和性能的解决方案。有了 PNaCl，我们也找到了平台可移植性，但没有浏览器可移植性。有了 asm.js，我们找到了浏览器可移植性和沙箱，但没有同等水平的性能。我们还仅限于处理 JavaScript，这意味着如果不先改变语言本身，就无法通过新增功能（例如高效的 64 位整数）来扩展平台。鉴于这是由国际标准化组织管理的，这不太可能是一种能够快速周转的方法。

此外，JavaScript 在浏览器从网络加载和验证它方面存在某些问题。浏览器必须等到完成下载所有引用的文件后，才开始验证和优化它们（而进一步的优化则需要等到应用程序已经运行）。鉴于我们已经说过，开发人员会用极其大量的传递性依赖来拖累他们的应用程序，JavaScript 的网络传输和加载时间性能是需要克服的另一个瓶颈，除此之外还有已建立的运行时间问题。

在看到这些部分解决方案所能实现的效果后，对高性能、沙箱化、可移植代码的需求变得非常强烈。浏览器、网络标准和 JavaScript 环境中的各个利益相关者都觉得需要一种在现有生态系统限制内工作的解决方案。投入大量工作使浏览器达到目前的水平。完全有可能在操作系统平台和浏览器实现之间创建动态、吸引人且交互式的应用程序。只要再多花一点努力，似乎就可以将这些愿景合并成一个统一的、基于标准的方案。

正是在这种情况下，2015 年，JavaScript 的创造者布兰登·艾奇宣布 WebAssembly 的工作已经开始³⁸。他强调了努力的几个具体原因，并称其为“低级安全代码的二进制语法，最初与 asm.js 同等表达，但在长期来看能够从 JS 的语义中分化，以便最好地作为多种源语言的通用对象级格式。”

他继续说道：“可能的长期分化示例：零成本异常、动态链接、call/cc。是的，我们的目标是开发 Web 的多语言编程对象文件格式。”

至于为什么这些各方对这件事感兴趣，他给出了这个理由：“asm.js 很棒，但一旦引擎为其优化，解析器就成了热点——在移动设备上非常热点。需要传输压缩，可以节省带宽，但解析前的解压缩会带来伤害。”

最后，也许公告中最令人惊讶的部分是参与者是谁：“一个 W3C 社区组，WebAssembly CG，对所有人开放。正如你从 github 日志中看到的那样，WebAssembly 到目前为止一直是 Google、Microsoft、Mozilla 和其他一些人的联合努力。我很抱歉工作最初是通过私有 github 账户进行的，但那是为了帮助几家大公司达成共识并买入必须进行的长期合作游戏，以便完成这项工作，而采取的临时措施。”

短时间内，其他公司如 Apple、Adobe、AutoCAD、Unity 和 Figma 也支持了这项工作。不可思议的是，这个几十年前就开始的、涉及无尽冲突和自私自利的愿景正在转变为一个统一的倡议，终于为我们带来一个安全、快速、可移植且与网络兼容的运行时环境。

引导这个平台诞生所涉及的潜在令人困惑的复杂性无穷无尽。尚不完全清楚应该提前指定什么。并非所有语言都原生支持线程。并非所有语言都使用异常。C/C++ 和 Rust 是没有垃圾回收需求的运行时语言的例子。魔鬼总是在细节中，但合作的意愿是存在的。而且，正如他们所说，有志者事竟成。

在接下来的一年左右的时间里，CG 成为了 W3C 工作组（WG），负责定义实际标准。他们做出了一系列决定，以定义一个最小可行产品（MVP）WebAssembly 平台，该平台将得到所有主要浏览器厂商的支持。

此外，Node.js 社区也感到兴奋，因为这可以为管理 Node 应用程序中需要用低级语言编写的部分的原生库的繁琐工作提供解决方案。

与其依赖 Windows、Linux 和 macOS 库，Node.js 应用程序可以拥有一个 WebAssembly 库，该库可以加载到 V8 环境中并即时转换为原生汇编代码。突然之间，WebAssembly 似乎准备好超越在浏览器中部署代码的目标，但让我们不要抢跑。我们有整本书的其余部分来告诉你故事的这一部分。

1 你可以在这里阅读更多关于这种权衡思想的内容：
https://en.wikipedia.org/wiki/No_Silver_Bullet

2 “Bringing the Web up to Speed with WebAssembly” 于 2017 年出版，可在此处获取：http://dx.doi.org/10.1145/3062341.3062363

3 多年来，OpenGL 是可移植 3D 图形应用程序的定义标准。如今它正被 Vulkan 和 Metal 等更现代的 API 取代，但你可以在这里了解更多关于标准的信息：https://www.opengl.org

4 可移植操作系统接口（POSIX）是一组 IEEE 标准，用于定义通用应用程序功能，以便在多个操作系统上工作：https://en.wikipedia.org/wiki/POSIX

5 Win32 是更大的一组 API 的一部分，这些 API 为开发人员提供访问 Windows 操作系统提供的常见功能的途径：https://en.wikipedia.org/wiki/Windows_API

6 这种技术允许决策者建立最低标准来决定何时满足其需求。目标不是找到完美的解决方案，而是在手头情况下可接受的解决方案：https://en.wikipedia.org/wiki/Satisficing

7 npm 的 Wikipedia 页面强调了几个因损坏的依赖项而产生重大影响的案例：https://en.wikipedia.org/wiki/Npm_(software)#Notable_breakages

8 据最好的人所知，是 https://twitter.com/jfbastien 最先说出这句话的，但他自己也不确定。

9 汇编语言是一种低级编程语言，通常与特定机器的处理器架构和指令集相关联：
https://en.wikipedia.org/wiki/Assembly_language

10 这个名字来自法语“Conseil européen pour la recherche nucléaire”。其众多令人兴奋的项目在主页上详细介绍：https://home.cern

11 在他自己的业余时间！

12 Archie 是帮助人们在 ftp 服务器上查找文件的早期搜索引擎：
https://en.wikipedia.org/wiki/Archie_(search_engine)

13 Gopher 是我们现在依赖的基于 HTTP 的网络的令人兴奋的前身：
https://en.wikipedia.org/wiki/Gopher_(protocol)

14 广域信息服务器（WAIS）是用于在分布式系统中搜索和请求文本信息的另一个早期系统：
https://en.wikipedia.org/wiki/Wide_area_information_server

15 SGML 是定义结构化、声明性文档的 ISO 标准，是 HTML、DocBook 和 LinuxDoc 的基础：
https://en.wikipedia.org/wiki/Standard_Generalized_Markup_Language

16 当时我购买了 Netscape 1.0 Silicon Graphics IRIX 的许可证。我仍然在某个地方有一张 CD……出于历史原因。

17 Scheme 是 Lisp 的一个相当轻量级的版本，你可以在这里阅读更多内容：
https://en.wikipedia.org/wiki/Scheme_(programming_language)

18 这里有一个关于 JavaScript 早期历史的很好的总结：https://auth0.com/blog/a-brief-history-of-javascript/

19 Self 是一种面向对象的编程语言，影响了 JavaScript 的基于原型的继承。你可以在这里找到更多关于它的信息：
https://en.wikipedia.org/wiki/Self_(programming_language)

20 Autometric 有一个疯狂的背景，涉及派拉蒙影业、Trinitron 管以及帮助 NASA 决定在月球上着陆地点！他们此后已被波音收购，但一些细节可在以下位置找到：https://en.wikipedia.org/wiki/Autometric

21 Google Earth 现在可以在浏览器中运行：https://www.google.com/earth

22 虽然浏览器厂商现在在标准上更紧密地合作，但一段时间以来他们竞争激烈。这个时期在此讨论：
https://en.wikipedia.org/wiki/Browser_wars

23 DOM 是由浏览器渲染的网页或应用程序的树结构。它通常以声明性文本形式从服务器发送到客户端作为 HTML，但 JavaScript 能够在浏览器中操作它。更多细节可在此处找到：
https://en.wikipedia.org/wiki/Document_Object_Model

24 你可以在这里了解更多关于 Jesse 的信息：https://en.wikipedia.org/wiki/Jesse_James_Garrett





25 Promise（或 future）允许开发者在仍提供并发功能的应用程序中拥有相对简单的编程模型：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

26 WebGL 将 OpenGL 世界中类似的 3D 图形模型带到了网络：https://en.wikipedia.org/wiki/WebGL

27 WebRTC 提供建立对摄像头和麦克风的受权限访问的机制，以及加密的点对点连接 https://en.wikipedia.org/wiki/WebRTC

28 LLVM 的缩写没有任何含义，但它是一个极具影响力的工具链，你应该更深入地了解。我们将在本书中频繁提到它。更多细节可在此找到：https://llvm.org

29 通常软件会被编译为二进制、可执行形式。IR 使其能够以解析的结构化形式存在，以便于优化等目的。

30 发音为“pinnacle”。

31 因为，NaCl… 懂了吗？

32 有关 asm.js 的详细信息可在此获取：https://asmjs.org

33 Clang 是 LLVM 的 C、C++ 和 Objective-C 编译器工具套件：https://clang.llvm.org

34 我们将在本书的后续章节中更多地学习 Emscripten，但如果你好奇：https://emscripten.org

35 在浏览器中实现零安装的游戏体验推动了大部分这一创新。你可以在此处看到 Unity 引擎在浏览器中使用 WebGL 的示例：https://beta.unity3d.com/jonas/AngryBots/

36 Node.js 是一个极受欢迎的服务器端 JavaScript 环境，我们将在第 8 章中更详细地讨论它。

37 nm 是一个 Unix 命令，用于显示可执行文件的符号表。

38 关于 WebAssembly 的公告文本可在此获取：https://brendaneich.com/2015/06/from-asm-js-to-webassembly/
