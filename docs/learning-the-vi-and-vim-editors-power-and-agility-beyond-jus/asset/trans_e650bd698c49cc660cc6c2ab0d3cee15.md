
注意使用 `echo` 命令中的点 (`.`) 记法。此操作符将表达式连接成一个字符串，而 `echo` 最终会显示它。在此例中，我们连接了一个字面字符串 `"setting color scheme to "` 和变量 `colorScheme` 赋予的值。

### 警告

我们对在该脚本中执行命令做出了错误的假设。如果你跟着示例一起编码，你已经知道这一点。我们将在下一节中纠正这个错误。

## execute 命令

到目前为止，我们已经改进了选择配色方案的方式，但上一次的更改引入了一个小小的变化。最初，我们决定根据一天中的时间执行一个独立的 `colorscheme` 命令。我们的上一次改进看起来是正确的，但在定义了一个变量 (`colorScheme`) 来保存配色方案的值之后，我们发现命令：

```
colorscheme colorScheme
```

结果在 [图 12-1](#ch12.html#vi8-ch-12-fig-1) 中显示的错误。

![](assets/lvv8_1201.png)

我们需要一种方法来执行引用变量而不是字面字符串（如 `darkblue`）的 Vim 命令。为此 Vim 提供了 `execute` 命令。当传入一个命令时，它会计算变量和表达式并将其值代入命令。我们可以利用这一特性以及上一节中展示的连接，将变量的值传递给 `colorscheme` 命令：

```
execute "colorscheme " . colorScheme
```

精确的语法（特别是引号）可能令人困惑。`execute` 命令期望变量或表达式，但 `colorscheme` 只是一个普通字符串，不是变量或表达式。我们不想让 `execute` 计算 `colorscheme`；我们只想让它原样接受名称。因此我们通过将其括在引号中把命令名变成字面字符串。顺便说一下，我们在最后的引号前添加一个空白。这很重要，因为我们需要在命令和值之间有一个空格。

我们的变量 `colorScheme` 必须在引号 *之外*，以便由 `execute` 计算。可以这样理解 `execute` 的行为：
* 普通单词会被计算为变量或表达式，`execute` 会用其值替换它们。
* 用引号括起来的字符串会被原样对待；`execute` 不会尝试计算它们以返回一个值。

使用 `execute` 修复了我们的错误，Vim 现在按预期加载了配色方案。

加载 Vim 后，你可以验证是否加载了正确的配色方案。`colorscheme` 命令会设置它自己的变量 `colors_name`。除了回显脚本中设置的变量值之外，你还可以手动执行 `echo` 命令并检查 `colors_name` 变量，以查看我们的脚本是否确实根据一天中的时间执行了正确的 `colorscheme` 命令：

```
:echo colors_name
```

## 定义函数

到目前为止，我们已经创建了一个对我们来说运行良好的脚本。现在让我们创建可以在会话期间任何时间执行的代码，而不仅仅是在 Vim 启动时。我们很快会给出一个示例，但在那之前，我们需要先创建一个包含脚本代码的 `function`。

Vim 允许你使用 `function…endfunction` 语句定义自己的函数。以下是用户自定义函数的一个示例骨架：

```
function MyFunction(arg1, arg2...)
  line of code
  another line of code
endfunction
```

我们可以轻松地将脚本转换为函数。请注意，我们不需要传入任何参数，因此函数定义中的括号为空：

```
function SetTimeOfDayColors()
  " progressively check higher values... falls out on first "true"
  let currentHour = strftime("%H")
  echo "currentHour is " . currentHour
  if currentHour < 6
    let colorScheme = "darkblue"
  elseif currentHour < 12
    let colorScheme = "morning"
  elseif currentHour < 18
    let colorScheme = "shine"
  else
    let colorScheme = "evening"
  endif
  echo "setting color scheme to " . colorScheme
  execute "colorscheme " . colorScheme
endfunction
```

### 注意

Vim 用户自定义函数名必须以大写字母开头。

现在我们在 `*.gvimrc` 文件中定义了一个函数。但如果不调用它，代码永远不会执行。你可以使用 Vim 的 `call` 语句调用函数。在我们的示例中它看起来像：

```
call SetTimeOfDayColors()
```

现在我们可以在 Vim 会话中的任何时间、任何位置设置配色方案。一个选择就是将之前的 `call` 行放入我们的 `*.gvimrc` 中。结果与我们早先的示例相同，即在不使用函数的情况下运行代码。但在下一节中，我们将看到一个巧妙的 Vim 技巧，它会反复调用我们的函数，以便在会话期间定期设置配色方案，从而在一天中动态变化！当然，这也引入了我们必须解决的其他问题。

## 一个不错的 Vim 搭便车技巧

在上一节中，我们定义了一个 Vim 函数 `SetTimeOfDayColors()`，我们调用它一次来定义配色方案。如果我们想反复检查一天中的时间并相应地更改配色方案怎么办？显然，`*.gvimrc` 中的一次性调用并不能实现这一点。为此，我们引入了一个使用 `statusline` 选项的巧妙 Vim 技巧。

大多数 Vim 用户都理所当然地使用 Vim 状态行。默认情况下，`statusline` 没有值，但你可以定义它以在状态行中显示 Vim 中几乎任何可用的信息。而且由于状态行可以显示动态信息，例如当前行和列，Vim 在编辑状态发生变化时就会重新计算并重新显示状态行。Vim 中的几乎任何操作都会触发状态行重绘。因此，我们将利用这一点作为技巧来调用我们的配色方案函数并动态更改配色方案。但正如我们很快会看到的，这是一种不完美的方法。

`statusline` 接受一个表达式，计算它并在状态行中显示它。这包括函数。我们利用此功能在每次状态行更新时调用 `SetTimeOfDayColors()`，而状态行更新很频繁。由于此功能会覆盖默认状态行，而我们不想丢失默认情况下获得的有价值信息，让我们在以下状态行的初始定义中加入大量信息：

```
set statusline=%<%t%h%m%r\ \ %a\ %{strftime(\"%c\")}%=0x%B\
    \\ line:%l,\ \ col:%c%V\ %P
```

### 注意

`statusline` 的定义跨了两行。Vim 将以反斜杠 (`\`) 作为首个非空白字符的行视为上一行的延续，并忽略反斜杠之前的全部空白。因此，如果你使用我们的定义，请确保它被完全复制并准确输入。如果无法使其工作，你可以恢复到以未定义的 `statusline` 开始。

你可以在 Vim 文档中查找百分号前各种字符的含义。该定义会产生如下所示的状态行：

```
ch12.asciidoc   Thu 26 Aug 2021 12:39:26 PM EDT     0x3C line:1,  col:1 Top
```

本章的重点不是状态行可以显示什么，而是利用 `statusline` 选项来计算函数。

现在我们将 `SetTimeOfDayColors()` 函数添加到 `statusline` 中。通过使用 `+=` 而不是普通的等号，我们是在末尾添加内容，而不是替换之前定义的内容：

```
set statusline += \ %{SetTimeOfDayColors()}
```

现在我们的函数已成为状态行的一部分。虽然它没有为状态行贡献有趣的信息，但它现在会检查一天中的时间，并在一天中的小时推移时可能更新我们的配色方案。这存在两个问题：
