

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

