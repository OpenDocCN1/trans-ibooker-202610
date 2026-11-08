
正如真正的程序员会放弃所见即所得的文字处理器而选择 `troff`，真正的网页管理员也会放弃花哨的 HTML 作者工具而选择 `vi`。你可以用 `vi Powered` 标志自豪地展示这一事实。☺

你可以在 [https://www.vim.org/logos.php](https://www.vim.org/logos.php) 找到多种变体的 `Vim` 标志。多个用于网站的 `Vim Powered` 标志位于 [https://www.vim.org/buttons.php](https://www.vim.org/buttons.php)。

# Java 爱好者的 vi

尽管标题如此，本小节讲的是你喝的咖啡，而不是你编程用的 Java。

我们假设的真正的程序员在使用 `vi` 编写她的 C++ 代码、`troff` 文档和网页时，无疑偶尔会想要一杯咖啡。她现在可以从印有 `vi` 命令参考的马克杯中喝咖啡！

所以这里是我们的第三项：`vi` 参考马克杯、T 恤、运动衫、烧烤围裙、婴儿兜布，甚至鼠标垫都可以在 [https://www.cafepress.com/geekcheat/366808](https://www.cafepress.com/geekcheat/366808) 购买。

## Vim 离合器

如果用手在 `vi` 或 `Vim` 中切换模式让你感到麻烦，你可能想制作自己的“Vim 离合器”。这个 USB 连接的脚踏板在按下时发送 `i`，释放时发送 `ESC`。

该项目在 [https://github.com/alevchuk/vim-clutch](https://github.com/alevchuk/vim-clutch) 有详细描述，包括零件、链接、说明和照片。

另一个 Vim 离合器展示在 [https://l-o-o-s-e-d.net/vim-clutch](https://l-o-o-s-e-d.net/vim-clutch)。在介绍过程中，作者还描述了除他自己之外几个其他的 Vim 离合器项目。

## 惊艳你的朋友！

与 `vi` 相关的有用物品集合曾经在 [alf.uib.no](http://alf.uib.no/) 的 FTP 档案中提供。原始档案位于 `ftp://afl.uib.no/pub/vi`。该集合被镜像在 `ftp://ftp.uu.net/pub/text-processing/vi`。这两个站点现在都已不可用。

幸运的是，Clement Cole 将他的档案副本提供给我们，以便收录到我们的 [GitHub 仓库](https://www.github.com/learning-vi/vi-files) 中，我们对此表示感谢。^[2](#app03.html#idm46601067593872)

不幸的是，这些文件最后更新于 1995 年 5 月。幸运的是，`vi` 的基本功能没有改变，档案中的许多信息和宏仍然有用。原始档案有四个子目录：

* `docs`
: 关于 `vi` 的文档，还有一些 `comp.editors` 帖子。

* `macros`
: `vi` 宏。

* `comp.editors`
: 发布到 `comp.editors` 的各种资料。

* `programs`
: 各种平台的 `vi` 克隆（以及其他程序）的源代码。

由于该目录今天大多已无关紧要，我们未包含 `programs` 目录。

`docs` 和 `macros` 最有趣。`docs` 目录包含大量文章和参考资料，包括初学者指南、错误解释、快速参考，以及许多简短的“如何做”类文章（例如，如何在 `vi` 中仅大写句子的第一个字母）。甚至还有一首关于 `vi` 的歌曲！

`macros` 目录中有 50 多个文件，各自执行不同的功能。我们在此仅提及其中三个。原始归档名称以 `.tar.Z` 结尾的文件已在我们的 [GitHub 仓库](https://www.github.com/learning-vi/vi-files) 中展开为独立目录。

* `evi-tar`
: 一个 Emacs“模拟器”。其思路是将 `vi` 变成无模式编辑器（始终处于输入模式，命令通过控制键执行）。实际上它是通过一个替换 `EXINIT` 环境变量的 shell 脚本实现的。

* `hanoi`
: 这或许是 `vi` 的不寻常用途中最著名的：一个解决汉诺塔编程问题的宏集。该程序仅显示移动步骤；它不会实际绘制圆盘。为有趣起见，我们已在 [“汉诺塔，vi 版本”](#app03.html#vi8-app-c-sidebar-1) 中重印了它。

* `turing-tar`
: 该程序使用 `vi` 实现了一台真正的图灵机！观看它执行程序相当令人惊叹。

除此之外还有许多、许多有趣的宏；请自行查看！

## Vi 爱好者主页

[Vi 爱好者主页](http://www.thomer.com/vi/vi.html) 包含以下内容：

* 所有已知 `vi` 克隆的表格，并附有源代码或二进制发行版的链接
* 指向其他 `vi` 站点的链接
* 大量指向不同级别的 `vi` 文档、手册、帮助和教程的链接
* 用于编写 HTML 文档和解决汉诺塔问题的 `vi` 宏，以及其他宏集的 FTP 站点
* 杂项 `vi` 链接：诗歌、关于 `vi`“真实历史”的故事、`vi` 与 Emacs 的讨论，以及 `vi` 咖啡杯（参见章节 [“Java 爱好者的 vi”](#app03.html#vi8-app-c-sect-4)）

请注意，该网站似乎很久没有更新了。许多链接有效，但许多无效。

## 另一种 vi 克隆

图 [C-2](#app03.html#vi8-app-c-fig-2) 至 [C-9](#app03.html#vi8-app-c-fig-9) 中描绘了 `vigor` 的故事，这是一种不同的 `vi` 克隆。

![](assets/lvv8_ac02.png)![](assets/lvv8_ac03.png)![](assets/lvv8_ac04.png)![](assets/lvv8_ac05.png)![](assets/lvv8_ac06.png)![](assets/lvv8_ac07.png)![](assets/lvv8_ac08.png)![](assets/lvv8_ac09.png)

`vigor` 的源代码可在 [http://vigor.sourceforge.net](http://vigor.sourceforge.net) 获取。

## 味道好，填充少

```
vi is [[13~^[[15~^[[15~^[[19~^[[18~^ a
muk[^[[29~^[[34~^[[26~^[[32~^ch better editor than this emacs. I know
I^[[14~'ll get flamed for this but the truth has to be
said. ^[[D^[[D^[[D^[[D ^[[D^[^[[D^[[D^[[B^
exit ^X^C quit :x :wq dang it :w:w:w :x ^C^C^Z^D
```
