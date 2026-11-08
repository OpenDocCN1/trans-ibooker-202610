
到目前为止，如果你一直在添加我们介绍过的插件，Vim 就已经提供了 IDE 大部分的功能。难怪很多人都在我们之前走过这条路，并提供了将 Vim 打造成 IDE 的自己的配方。

以下是我们发现的一些，你可能想去查看一下。至少，它们为你提供了我们没有介绍的有用插件的线索。

买家自负：我们并没有尝试所有这些，而且我们尝试过的那些，也只尝试了很短的时间。把这个列表当作你自己探索的起点吧：

### Vim as an IDE

这更像是一份关于用于软件开发的 Vim 插件的教程，而不是即拿即用的方案。其价值在于提供了许多指向更多信息的来源的链接。参见 [`https://github.com/jez/vim-as-an-ide`](https://github.com/jez/vim-as-an-ide)。

### vimspector

这是一个“适用于 Vim 的多语言图形化调试器”。这里的重点是调试代码，而不是成为功能齐全的 IDE。参见 [`https://github.com/puremourning/vimspector`](https://github.com/puremourning/vimspector)。

### C/C++ IDE

这组插件组合在一起为 C 和 C++ 提供 IDE。引用自[网页](https://github.com/kingofctrl/vim.cpp)，你将获得：

> * 自动下载 [sic] 最新版本的 `libclang` 并编译 `YCM` 所需的 `ycm_core` 库
> * 一键安装
> * 支持所有 GNU/Linux
> * 按需加载以加快启动时间
> * 语义自动补全
> * 语法检查
> * C++11/14 语法高亮
> * 保留历史记录
> * 即时预览 Markdown 文件

以下适用于 Python：

### Python-mode

引用自[该网页](https://github.com/python-mode/python-mode)：

> 该插件包含了在 Vim 中开发 Python 应用所需的一切。
>
> * 支持 Python 及 3.6+
> * 语法高亮
> * Virtualenv 支持
> * 运行 Python 代码 (`<leader>r`)
> * 添加/移除断点 (`<leader>b`)
> * 改进的 Python 缩进
> * Python 移动和操作符 (`]]`, `3[[`, `]]M`, `vaC`, `viM`, `daC`, `ciM`, …)
> * 改进的 Python 代码折叠
> * 同时运行多个代码检查器 (`:PymodeLint`)
> * 自动修复 PEP8 错误 (`:PymodeLintAuto`)
> * 在 Python 文档中搜索 (`<leader>K`)
> * 代码重构
> * IntelliSense 代码补全
> * 转到定义 (`<C-c>g`)

### Vim and Python—A Match Made in Heaven

来自 Real Python 的人士的[此页面](https://realpython.com/vim-and-python-a-match-made-in-heaven)提供了逐步说明，教你如何将 Vim 配置为 Python IDE。

### Vim Upgrade 2017

[此页面](https://haridas.in/vim-upgrade-2017.html)类似，提供了配置 Vim 的说明并推荐了日常编程的不同插件。

### Vim as a Python IDE

[再次引用](https://rapphil.github.io/vim-python-ide)：

> 本项目旨在将 Vim 用作功能强大且完整的 Python IDE。为此，我们整理了社区中可用的优秀插件列表，并为这套插件提供了自动安装程序。

这个项目有趣之处在于它不留任何偶然。它会检出、配置并构建特定版本的 Vim，以确保一切都能正常工作。如果你想用 Vim 进行 Python 开发，并且对它为你做的选择感到满意，这可能是最简单的方法。

### chenfjm’s VimPlugins

这是一组 Vim 配置文件，由 24 个不同的插件构建成 IDE。里面有简洁的英文安装说明和中文教程。尽管如此，由于它使用了如此多的插件，它也是指向你可能想要调查的额外插件的良好来源。参见 [`https://github.com/chenfjm/VimPlugins`](https://github.com/chenfjm/VimPlugins)。

# 编程很棒，但如果我是作家呢？

有很多插件旨在帮助人们使用 Vim 进行写作，而不仅仅是软件开发。

Tomas Fernández 在博客文章 [“作家用的 Vim 插件 Top 10”](https://tomfern.com/posts/vim-for-writers) 中提供了一个不错的列表。最简单的方法就是直接引用文章中插件的列表、描述和链接：

### vim-pencil

我最喜欢的写作插件。`vim-pencil` 带来了大量不错的功能，如导航辅助、基于标点的更智能撤销，以及适当的软换行。参见 [`https://github.com/reedes/vim-pencil`](https://github.com/reedes/vim-pencil)。

### vim-ditto

`vim-ditto` 会突出显示段落中重复的单词，正是我需要避免反复使用词语的。参见 [`https://github.com/dbmrq/vim-ditto`](https://github.com/dbmrq/vim-ditto)。

### vim-goyo

Vim 的 Writeroom 仿制品，`goyo` 移除了所有分散注意力的元素，如状态行和行号。参见 [`https://github.com/junegunn/goyo.vim`](https://github.com/junegunn/goyo.vim)。

### vim-colors-pencil

一种优雅、低对比度的配色方案，专为写作而设计。参见 [`https://github.com/reedes/vim-colors-pencil`](https://github.com/reedes/vim-colors-pencil)。

### vim-litecorrect

`Litecorrect` 会自动纠正常见的打字错误，比如把 “teh” 改为 “the”。参见 [`https://github.com/reedes/vim-litecorrect`](https://github.com/reedes/vim-litecorrect)。

### vim-lexical

拼写检查器和同义词词典合一。`Vim-lexical` 让我可以用 `]s`、`[s` 在拼写错误之间导航，并用 `<leader> t` 快速查找同义词。参见 [`https://github.com/reedes/vim-lexical`](https://github.com/reedes/vim-lexical)。

### vim-textobj-sentence

一个用于更好句子导航的插件。我可以用 `(` 和 `)` 在句子之间移动，用 `dis` 剪切一个句子。依赖于 [`vim-textobj-user`](https://github.com/kana/vim-textobj-user)。参见 [`https://github.com/reedes/vim-textobj-sentence`](https://github.com/reedes/vim-textobj-sentence)。

### vim-textobj-quote

这个插件智能地创建“引号”，我就不必自己动手了。参见 [`https://github.com/reedes/vim-textobj-quote`](https://github.com/reedes/vim-textobj-quote)。

### ALE

Asynchronous Lint Engine 是一个多语言分析工具，不限于代码。它支持许多风格检查器，如 [`proselint`](http://proselint.com) 和 [`LanguageTool`](https://languagetool.org)。参见 [`https://github.com/dense-analysis/ale`](https://github.com/dense-analysis/ale)。

文章还有更多内容；值得通读全文。

# 结论

Vim Awesome 的朋友们说得对：Vim 真的很棒！本章只触及了 Vim 插件世界冰山一角。我们希望你能搭建自己的 IDE，并充分利用 Vim 的强大功能和能力，这些能力远远超出了普通文本编辑器的范围。

只是请记住，在探索所有可用的选项时偶尔也要浮出水面透口气。祝好运！

^[1](#ch15.html#idm46601071502976-marker) 真正的程序员只使用 `Make`。

# 第16章. vi 无处不在

## 引言
