

```
}
let printString = function(str) {
    console.log(str);
};

const importObject = {
    env: {
        print_string: function(str) {
            printString(charPtrToString(str));
        }
    }
};
...
</script>
```

这涵盖了 JavaScript 端。在 C/C++ 端，我们在示例 5-17 中看到 *nanolibC/libc_extra.h* 头文件定义了一个名为 `print_string()` 的函数，它接受一个 `char *`。

## Example 5-17. Exposing JavaScript functions as C functions.

```
#ifndef _NANOLIB_C_EXTRA_H
#define _NANOLIB_C_EXTRA_H

extern "C" {
    // Will be provided by Javascript.
    void print_string(const char* str);
}

#endif
```

在 *nanolibC* 目录中有一个文件定义了我们的 `printf()` 实例。细节很复杂所以我不想深入，但我会指出它调用 `puts()` 来将 `char *` 输出到输出控制台。通常这是操作系统提供的低级服务，但根据你迄今为止所见，一旦我们在示例 5-18 中连接最后一步，我们的 JavaScript 处理程序会将其路由到其控制台。

## Example 5-18. Exposing JavaScript functions as C functions.

```
int puts ( const char * str ) {
    print_string(str); ①
```

```
return 0;
}
```

1 puts() 调用 JavaScript 函数并传入一个 char *。

终于，我们看到了这是如何工作的。我们的函数调用 printf()，printf() 调用 puts()，而 puts() 被定义为调用提供的函数。我现在不想描述它是如何连接的，但我希望结果仍然令人满意。关于使用 C/C++ 与 WebAssembly 还有更多需要了解的知识，但那是即将出版的章节的主题。在此之前，你已经跨越了理解 WebAssembly 背后工作原理的重要鸿沟。接下来我们将学习如何将现有软件移植到浏览器中运行。

- 1 C 的历史是我们行业不可分割的一部分，也是现代操作系统开发的一部分。 https://en.wikipedia.org/wiki/C_(programming_language)

- 2 有史以来最受欢迎的编程书籍之一：
+   https://en.wikipedia.org/wiki/The_C_Programming_Language

- 3 以及安全问题的大部分份额！

- 4 LLVM 以前代表 Low-Level Virtual Machine，但现在它只是意味着 LLVM。 在 https://llvm.org 了解更多信息

- 5 循环展开是许多编程语言中常见的优化。
+   https://en.wikipedia.org/wiki/Loop_unrolling

- 6 如果你不介意一些烦人的广告，可以在 https://learn-c.org 找到一个有用的 C 教程。否则，Steve Oualline 的 Practical C Programming 或上述的 The C Programming Language 都是很好的入门书籍。

- 7 nm 是 C 世界中常见的工具，用于打印此类二进制文件的内容。

- 8 有时被称为“五磅袋装十磅粪肥”。装不下！

- 9 你可以在这里找到 Petter 的示例项目： https://github.com/PetterS/clang-wasm

- 10 本书超出范围涵盖这些细节，但如果你感兴趣，可以在这里找到更多信息： https://en.wikipedia.org/wiki/Name_mangling

- 11 Merge Sort 是一种 O(n log n) 复杂度的实用排序算法。实现起来很容易，这就是我选择它的原因。你可以在这里找到更多细节：
+   https://en.wikipedia.org/wiki/Merge_sort

- 12 Petter 在之前列出的 GitHub 仓库中的示例在 JavaScript 和 C 之间做了更多字符串的移动

## Chapter 6. Applied WebAssembly : Legacy Code in the Browser

## A NOTE FOR EARLY RELEASE READERS

对于早期发布的电子书，你可以在作者撰写时获得书籍的最早形式——作者的原始未编辑内容，以便你可以在这些技术正式发布之前利用它们。

这将是最终书籍的第 6 章。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系作者 wasmbook@bosatsu.net。

> 无论你走到哪里，那里都有你。

—Buckaroo Banzai

是时候更仔细地审视在浏览器中调用 C/C++ 代码的过程了。这些语言中的大多数代码从未打算以下载的形式在浏览器中运行。但正如香港骑士队¹的队长在上面的引语中告诉我们的，有时你会发现自己身处意想不到的新地方，但仍然只是你自己在那里。

我们出于各种原因对这样做感兴趣。取代 JavaScript 不是其中之一。至少对大多数人来说不是。相反，我们在外面的 C 和 C++ 等语言中拥有庞大的遗留代码库。其中很多非常有用，能够在我们的 Web 应用程序中访问它们将会很棒。其中一些可能将组织绑定到遗留系统。通过浏览器分发这些代码将是一个巨大的进步！

此外，简单地用 JavaScript 编写并不适合的一些问题。能够在不要求单独运行时机的情况下用另一种语言编写应用程序的该部分是非常有吸引力的。而且，正如我们的最终示例所示，让受信任来源提供的受信任代码为加密算法等敏感和棘手的软件提供出处具有真正的价值。能够简单地重新编译你认识的人编写的现有代码，并且他们知道自己在做什么，这也是一种有用的能力。

在上一章中，我们展示了使用像 clang 这样的常规 WebAssembly 启用的 C 编译器以及一些头文件和库依赖管理来实现基本集成是可能的。然而，手动提供我们自己的标准库版本并将 C 代码链接到提供的 JavaScript 很快就会变得过时。

幸运的是，Emscripten 项目已经为使这个过程比它可能更简单奠定了基础。这并不令人惊讶，因为其主要开发者 Alon Zakai 和 Luke Wagner 在从 asm.js 开始的工作中一直处于领先地位，扩展到 WebAssembly MVP 以及至今仍在进行的规范改进。Emscripten² 工具链在旅程的大部分时间里都在那里。

该项目基于 LLVM 平台。在前面的章节中，我指出它最初有一个自定义后端，发出 asm.js 的可优化 JavaScript 子集。一旦 WebAssembly 平台被定义，一个新的后端就能够发出 Wasm 二进制文件。

不幸的是，这只是解决方案的一部分。还需要支持将数据输入和输出内存、链接模块、包装现有库等。提供用户界面的 C 程序或监听网络请求的程序通常在响应输入活动的相当紧密的循环中旋转。鉴于浏览器默认是单线程环境，这种主循环将存在操作不匹配。Emscripten 工具链已被修改以解决将原生 C/C++ 移植到我们的 Web 环境中运行时可能出现的许多类型问题。与大多数主题一样，这不能是对该项目所做的一切的全面介绍，但我会尝试快速让你入门。

## Proper “Hello, World!”

所以，首先一个坦白：我们本可以在第 2 章就拥有一个在浏览器中工作的、未修改的“Hello, World!”示例。最后一次，我们将在示例 6-1 中向你展示代码。

### Example 6-1. The typical “Hello, World!” program as expressed in C.

```
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    return 0;
}
```

使用 Emscripten C 编译器（安装说明可在附录 A 中找到），我们只需要告诉它编译 C 代码并生成一些 JavaScript 脚手架。之后它在 Node.js 中未修改运行。

```
brian@tweezer ~/g/w/s/ch06> emcc hello.c -o hello.js
brian@tweezer ~/g/w/s/ch06> ls -laF
total 520
drwxr-xr-x  7 brian  staff     224 Mar  1 14:45 ./
drwxr-xr-x  7 brian  staff     224 Mar  1 13:02 ../
-rw-r--r--  1 brian  staff  121457 Mar  1 13:05 bootstrap.min.css
-rw-r--r--  1 brian  staff      76 Mar  1 13:02 hello.c
-rw-r--r--  1 brian  staff     388 Mar  1 13:07 hello.html
-rw-r--r--  1 brian  staff  121686 Mar  1 14:45 hello.js
-rwxr-xr-x  1 brian  staff   11711 Mar  1 14:45 hello.wasm*
brian@tweezer ~/g/w/s/ch06> node hello.js
Hello, World!
```

示例 6-2 中的 HTML 文件不是通过此过程生成的，并且与我们之前看到的有 suspiciously 不同。有一个单一的 <script> 元素加载我们生成的 JavaScript。我们没有使用到目前为止使用的 utils.js 文件。相反，我们有一个更长的

