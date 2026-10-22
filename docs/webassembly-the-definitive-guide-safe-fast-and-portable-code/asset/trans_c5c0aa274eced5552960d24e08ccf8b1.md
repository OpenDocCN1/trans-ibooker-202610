

好消息是，我们不需要对 JavaScript 代码做太多修改！由于签名已经改变，我们现在可以将字符串传递给我们的方法来调用 main() 方法。与其在 HTML 中仅输出略有不同的 JavaScript，不如在图 6-6 中可以看到从开发者控制台使用参数调用可执行文件的结果。

![](img/16b7990eb0dbf589db177bdbd2f4a189_145_0.png)

图 6-6. 在浏览器中使用命令行参数调用我们的 Bitmap 生成器

除了基于命令行参数选择测试之外，你可能还想将测试仅作为函数运行。这需要更深入的讨论。

我们将通过向测试文件添加一个名为 run_test() 的方法开始，该方法将接受一个参数。此时无需重复实际代码，因此我们将仅打印出指示请求运行哪个测试的字符串。你可以在示例 6-14 中看到此函数的定义。

## 示例 6-14. 导出一个可从 JavaScript 调用的函数

```
void run_test(int i) {
    printf("Running test %d!\n", i);
}
```

默认情况下仅导出 main() 方法，因为它是启动程序所需的唯一函数。我们需要按如下方式添加 EXPORTED_FUNCTIONS 指令。函数名称以下划线字符开头。如果你仍希望 main() 可调用，则需要将其包含在内，我们在示例 6-15 中并未这样做。

## 示例 6-15. 修改 Makefile 以导出额外方法

```
bitmap_test: bitmap_test.cpp bitmap_image.hpp
    $(COMPILER) $(OPTIONS) bitmap_test.js bitmap_test.cpp
$(LINKER_OPT) ←
    -s FORCE_FILESYSTEM=1 ←
    -s ALLOW_MEMORY_GROWTH=1 ←
    -s INVOKE_RUN=0 ←
    -s EXPORTED_FUNCTIONS="['_main', '__run_test']" ←
    -s EXTRA_EXPORTED_RUNTIME_METHODS="['callMain']"
```

不幸的是，这行不通，因为我们使用的是 C++。生成的函数名称会被编译器进一步破坏，原因在此不赘述⁷。为了避免此问题，我们需要告诉编译器抑制此行为并使用 C 链接。要启用此行为，我们需要将函数定义修改为示例 6-16 的样子。

## 示例 6-16. 使用 C 链接导出一个可从 JavaScript 调用的函数

```
extern "C"
void run_test(int i) {
    printf("Running test %d!\n", i);
}
```

这应该能解决该问题，但我将再做一次更改来向你展示另一个选项。你有一个来自 Emscripten 工具链的便捷方法 cwrap，它会为调用特定 C 函数生成一个 JavaScript 函数。我们在示例 6-17 中将其添加到 EXTRA_EXPORTED_RUNTIME_METHODS 指令中。

## 示例 6-17. 更新 Makefile 以使用 cwrap

```
bitmap_test: bitmap_test.cpp bitmap_image.hpp
    $(COMPILER) $(OPTIONS) bitmap_test.js bitmap_test.cpp
$(LINKER_OPT) ↵
    -s FORCE_FILESYSTEM=1 ↵
    -s ALLOW_MEMORY_GROWTH=1 ↵
    -s INVOKE_RUN=0 ↵
    -s EXPORTED_FUNCTIONS="['_main', '_run_test']" ↵
    -s EXTRA_EXPORTED_RUNTIME_METHODS="['callMain',
'cwrap']"
```

如果重新构建并重新加载 HTML，你将能够从 JavaScript 开发者控制台调用此函数。参见图 6-7 中执行此操作的结果。

![](img/16b7990eb0dbf589db177bdbd2f4a189_147_0.png)

图 6-7. 直接通过 cwrap() 从 JavaScript 调用我们的函数

请注意，cwrap() 调用返回一个适当的 JavaScript 函数，我们可以像往常一样调用它。你可以将 switch 语句移至此方法并具有相同的调用任意测试的能力。

为了进一步练习，尝试添加一个写入名为 image.bmp 的 Bitmap 文件的方法。从 C++ 代码导出此方法并从浏览器调用

它。这随后可以让你在需要时将文件读回以供测试使用。你可以修改 `switch` 语句以允许调用这些方法。

最后，想象一些其他用户界面元素让你选择要运行哪个测试。运行后，想象一个文件列表要在 `<canvas>` 元素中显示。你几乎拥有完成此操作所需的所有组件，所以试试看！

## libsodium

在结束本章之前，我想引起你对一个名为 libsodium 的项目注意。我们不会直接使用它，但它展示了通过 WebAssembly 将诸如 C 和 C++ 之类的语言与浏览器混合的额外动机。

该库基于另一个名为 Networking and Cryptography library (NaCl)⁸ 的库。它是由懂得自己在做什么的人编写的高性能现代加密库。NaCl 的许多功能在 JavaScript 运行时中可能尚未可用。新的密码套件⁹ 包括带附加数据的认证加密 (AEAD) 可能在此出现，而在移植到 JavaScript 或通过操作系统提供给浏览器之前。WebAssembly 的一个令人信服的用例是能够基本上重新编译此类现代支持并立即将其带到浏览器，而无需等待更新。

第二个动机是 NaCl 库的作者知道自己在做什么。用糟糕的实现很容易削弱加密库的有效性。即使是比较两个哈希值是否相同这样微妙的事情，如果实现不正确也会泄露细节。令人沮丧的是，此比较的正确实现将违背开发人员通常比较两个哈希的方式。我的观点是，NaCl 代码库具有来源可追溯性。如果没有背景来理解这些细节的 JavaScript 开发人员尝试实现该功能，很可能会出现这些漏洞。当你拥有一个受信任的代码库时，能够重新编译并直接使用它也是考虑本章主题的另一个原因。

因此，libsodium 基本上是一个旨在通过 WebAssembly 将 NaCl 库导出到 JavaScript 环境的项目，无需重写或在性能上妥协。它旨在作为 WebAssembly 项目维护。我认为一旦人们更好地了解以这种方式使用 WebAssembly 的含义，我们将开始看到更多可以作为原生库或 WebAssembly 模块使用的项目，具体取决于你的配置需求。这将是代码重用的绝佳机会。我们将在第 10 章看到这种方法的另一个示例。

到那时，关于 WebAssembly 仍有更多内容需要学习。

1 “The Adventures of Buckaroo Banzai Across the 8th Dimension” 是有史以来最好的邪典科幻电影之一。更多详情：https://en.wikipedia.org/wiki/The_Adventures_of_Buckaroo_Banzai_Across_the_8th_Dimension

2 该名称是 JavaScript 和 “The Simpsons” 中流行的词 “embiggen” 的语言混合。

3 在此处阅读有关 Emscripten 对构建 C 和 C++ 应用程序的支持的更多信息：https://emscripten.org/docs/compiling/Building-Projects.html

4 查看这个很棒的学习资源：https://levelupwasm.com

5 代码可在此获取：https://tinyurl.com/bitmap-in-javascript

6 这是你的家庭作业，让它工作。

7 如果你想了解更多关于 C++ 名称修饰的信息，请查看此描述：https://en.wikipedia.org/wiki/Name_mangling

8 请注意，这与我们在第 1 章中提到的 Native Client (NaCl) 无关。你可以在此处找到有关此项目的更多详细信息：http://nacl.cr.yp.to

9 密码套件是一组提供加密引擎一组功能的密码学原语

## 第 7 章. WebAssembly 表

## 早期发布读者的注意事项

使用 Early Release 电子书，你可以在这些技术正式发布之前很久就获得书籍的早期形式——作者在写作时的原始未编辑内容——从而利用这些技术。

这将是最终书籍的第 7 章。

如果你对我们如何改进本书的内容和/或示例有任何评论，或者如果本章中缺少材料，请联系作者 wasmbook@bosatsu.net。

> 餐桌是教授和练习不仅是餐桌礼仪，也是交谈、体谅、宽容、家庭情感以及除小步舞以外的所有礼貌社会成就的中心。
> 
> —Judith Martin

餐桌是分享思想和故事的绝佳隐喻。与独自进食相比，与他人一起加入总是更有趣。如果你召集来自各行各业的一群人，你可以激发关于近乎无限多个话题的光辉、充满活力的对话。没有人拥有所有的故事。一些参与者可能会分享同一故事的某些方面。其他人可能有自己的版本。然而，为了发挥作用，仍需要一定程度的礼仪、克制以及接受其他参与者所提供内容的意愿。行为不端、不停止说话或踩到他人台词的餐桌客人会让所有人感到糟糕。

