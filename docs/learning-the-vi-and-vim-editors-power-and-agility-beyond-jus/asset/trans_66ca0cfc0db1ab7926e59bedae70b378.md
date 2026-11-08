
# Vim 自动命令文件类型检测

## Vim 事件

在写入缓冲区到文件之前。

`FileType`:
设置 `filetype` 之后。

`VimResized`:
Vim 窗口大小发生变化后。

`WinEnter`, `WinLeave`:
分别在进入或离开 Vim 窗口时。

`CursorMoved`, `CursorMovedI`:
分别在 `vi` 命令模式或插入模式中每次光标移动时。

总共有近 80 个 Vim 事件。对于这些事件中的任何一个，你都可以定义一个自动 `autocmd`，在该事件发生时执行。`autocmd` 的格式是：

```
autocmd [group] event pattern [nested] command
```

## Autocmd 格式

此格式的元素如下：

`group`:
一个可选的命令组（稍后描述）。

`event`:
将触发 `command` 的事件。

`pattern`:
匹配 `command` 应执行的 文件名的模式。

`nested`:
如果存在，允许此自动命令嵌套在其他命令中。

`command`:
事件发生时要执行的 Vim 命令、函数或用户定义脚本。

我们的目标是识别打开的任何新文件的文件类型，因此我们对 `pattern` 使用 `*`。

下一个决定是使用哪个事件来触发我们的脚本。因为我们希望尽可能早地尝试识别文件类型，所以有两个不错的候选者：`CursorMoved` 和 `CursorMovedI`。

`CursorMoved` 在光标移动时触发事件，这似乎很浪费，因为仅仅移动光标不太可能提供有关文件类型的更多信息。相比之下，`CursorMovedI` 在输入文本时触发，因此似乎是最好的候选者。

我们必须编写一个函数来每次执行工作。让我们称之为 `CheckFileType()`。我们现在有足够的信息来定义我们的 `autocmd` 命令。它看起来像这样：

```
autocmd CursorMovedI * call CheckFileType()
```

## 检查选项

在我们的 `CheckFileType()` 函数中，我们需要检查 `filetype` 选项的值。Vim 脚本使用特殊变量通过在选项名称（我们这里是 `filetype`）前加上与号（`&`）字符来提取选项的值。因此我们在函数中使用变量 `&filetype`。

我们从 `CheckFileType()` 的简单版本开始：

```
function CheckFileType()
 if &filetype == ""
   filetype detect
 endif
endfunction
```

Vim 命令 `filetype detect` 是安装在 `$VIMRUNTIME` 目录中的 Vim 脚本。它会遍历许多标准并尝试为你的文件分配文件类型。通常这只发生一次，因此如果文件是新的并且 `filetype` 无法确定文件类型，编辑会话就无法分配语法格式。

有一个问题：我们在输入模式下每次光标移动时都会调用函数，不断尝试检测文件类型。为了解决这个问题，我们首先检查文件是否已有文件类型，这意味着函数在之前的执行中已成功，因此不再需要再次执行。我们不会担心异常情况，例如错误识别或从一种编程语言开始编辑文件然后决定改为另一种语言。

让我们编辑一个新 shell 脚本文件并查看结果：

```
$ vim ScriptWithoutSuffix
```

输入以下内容：

```
#! /bin/sh

inputFile="DailyReceipts"
```

到现在为止，Vim 已打开颜色语法，如 [图 12-2](#ch12.html#vi8-ch-12-fig-2) 所示。

![](assets/lvv8_1202.png)

从图片可以看出 Vim 对字符串使用灰色，但印刷图像未显示 `#! /bin/sh` 为蓝色，`inputFile=` 为黑色，而 `"Daily​Re⁠ceipts"` 为紫色。不幸的是，这些不是 shell 语法高亮的颜色！通过命令 `:set filetype` 快速检查 `filetype` 选项会显示 [图 12-3](#ch12.html#vi8-ch-12-fig-3) 所示的消息。

![](assets/lvv8_1203.png)

Vim 将文件类型 `conf` 分配给了我们的文件，这不是我们想要的。出了什么问题？

如果你尝试这个示例，你会看到 Vim 在第一个 `CursorMovedI` 事件中输入第一个字符 `#` 时立即分配了文件类型。Unix 实用程序和守护进程的配置文件通常使用 `#` 字符来开始注释，因此 Vim 的启发式方法假设行首的 `#` 是配置文件中注释的开始。我们必须教会 Vim 更有耐心一些。

让我们修改函数以允许更多上下文。不要在第一个可用机会就尝试检测文件类型，让用户先输入大约 20 个字符。

## 缓冲区变量

我们需要在函数中引入一个变量，告诉 Vim 暂缓并 *不要* 尝试检测文件类型，直到 `CursorMovedI` 自动命令调用该函数超过 20 次。我们对什么是新文件以及要在该文件中输入多少字符的认识是特定于缓冲区的。换句话说，编辑会话中其他缓冲区的光标移动不应计入检查。因此，我们使用缓冲区变量并将其命名为 `b:countCheck`。

接下来，我们修改函数以检查输入模式下光标至少移动 20 次（意味着大约输入了 20 个字符），同时检查是否已分配文件类型：

```
function CheckFileType()
  let b:countCheck += 1

  " Don’t start detecting until approximately 20 chars.
  if &filetype == "" && b:countCheck > 20
    filetype detect
  endif
endfunction
```

但现在我们得到了 [图 12-4](#ch12.html#vi8-ch-12-fig-4) 所示的错误。

![](assets/lvv8_1204.png)

这是一个熟悉的错误。和以前一样，我们在变量定义之前就大胆地递增了它。而这次完全是我们的错，因为我们的脚本负责定义 `b:countCheck`。我们将在下一节处理这个微妙之处。

## exists() 函数

了解如何管理所有变量和函数很重要：Vim 要求你在任何类型的求值引用它之前就已定义每个变量和函数，使其 `exists`。

我们可以通过检查 `b:countCheck` 是否存在并使用前面显示的 `:let` 命令为其分配值来轻松解决脚本错误：

```
function CheckFileType()
  if exists(“b:countCheck”) == 0
    let b:countCheck = 0
  endif

  let b:countCheck += 1

  " Don't start detecting until approx. 20 chars.
  if &filetype == "" && b:countCheck > 20
    filetype detect
  endif
endfunction
```

现在测试代码。[图 12-5](#ch12.html#vi8-ch-12-fig-5) 显示了达到 20 个字符限制之前的时刻，而 [图 12-6](#ch12.html#vi8-ch-12-fig-6) 显示了输入第 21 个字符的效果。

![](assets/lvv8_1205.png)![](assets/lvv8_1206.png)

`/bin/sh` 文本突然有了语法颜色高亮。通过 `set filetype` 快速检查可以验证 Vim 已做出正确的分配，如 [图 12-7](#ch12.html#vi8-ch-12-fig-7) 所示。

![](assets/lvv8_1207.png)

在所有实际用途上，我们已经有一个完整且令人满意的解决方案，但为了形式更规范，我们添加另一个检查，以在输入大约 200 个字符后停止 Vim 尝试检测文件类型：

```
function CheckFileType()
  if exists("b:countCheck") == 0
    let b:countCheck = 0
  endif

  let b:countCheck += 1

  " Don't start detecting until approx. 20 chars.
  if &filetype == "" && b:countCheck > 20 && b:countCheck < 200
    filetype detect
  endif
endfunction
```
