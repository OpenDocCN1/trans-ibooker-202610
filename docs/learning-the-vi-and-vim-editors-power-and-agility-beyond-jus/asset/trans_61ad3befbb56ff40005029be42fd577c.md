
Vim 的 `highlight` 命令让你操作不同的组并控制它们在编辑会话中的高亮方式。此命令功能强大。你可以以列表形式检查各种组的设置，或请求特定组的高亮信息。例如：

```
:highlight comment
```

返回 [图 11-22](#ch11.html#vi8-ch-11-fig-22)。第一个字段列出高亮名称（在此示例中为“Comment”）。第二个字段始终显示字符串“`xxx`”，其显示方式由终端或 GUI 中的高亮定义决定。

![](assets/lvv8_1122.png)

输出显示了此文件中注释的显示方式。`xxx` 在印刷页面上为深灰色，但在屏幕上为红色.^([9](#ch11.html#idm46601075601696)) `term=bold` 的输出意味着在不支持颜色的终端上，`Comments` 将以粗体显示。`ctermfg=12` 意味着在彩色终端上，例如彩色显示器上的 `xterm`，`Comments` 的前景色将为对应的 DOS 颜色蓝色。最后，`guifg=Red` 意味着 GUI 界面将以红色前景色显示 `Comments`。

#### 注
DOS 颜色方案比现代 GUI 集合的颜色更受限制。在此方案中，只有八种颜色：`black`、`red`、`green`、`yellow`、`blue`、`magenta`、`cyan` 和 `white`。每个颜色都可以设置为文本前景或背景，并且可选地定义为“亮”，即屏幕上更亮的颜色。Vim 在非 GUI 窗口中定义文本颜色时使用类似的映射，例如在 `xterm` 中。

GUI 窗口提供几乎无限的颜色定义。Vim 允许你使用 `Blue` 等常用名称定义一些颜色，但你也可以使用红、绿、蓝值来定义这些颜色。格式为 `#rrggbb`，其中 `#` 为字面量，而 `rr`、`gg` 和 `bb` 是表示每种颜色等级的十六进制数字。例如，红色可以用 `#ff0000` 定义。

使用 `highlight` 命令更改你不喜欢的颜色组的设置。例如，我们可以发现此文件中标识符在我们的 GUI 界面中为深青色，如 [图 11-23](#ch11.html#vi8-ch-11-fig-23) 输出所示：

```
:highlight identifier
```

![](assets/lvv8_1123.png)

我们可以用以下命令重新定义标识符的颜色：

```
:highlight identifiers guifg=red
```

现在屏幕上的所有标识符都是（相当难看的）红色。这种定制相当不灵活：它适用于所有文件类型，并且不适应不同的背景或配色方案。

要查看存在多少高亮定义及其值是多少，再次使用 `highlight`：

```
:highlight
```

[图 11-24](#ch11.html#vi8-ch-11-fig-24) 显示了 `highlight` 命令结果的一个小样本。

![](assets/lvv8_1124.png)

注意有些行包含完整定义（列出 `term`、`ctermfg` 等），而其他行则从父组继承属性（例如，`String` 链接回 `Constant`）。

### 覆盖语法文件
在上一节中，我们学习了如何为组的全部实例定义语法组属性。假设你只想为一个或少数几个语法定义更改某个组。Vim 允许你使用 `after` 目录来实现这一点。这是一个目录，你可以在其中创建任意数量的 `after` 语法文件，Vim 会在正常语法文件之后执行这些文件。

为此，只需在包含在 `runtimepath` 选项中的名为 `after` 的目录中的特定文件中包含高亮命令（或任何处理命令——“after”处理的概念是通用的）。现在当 Vim 为你的文件类型设置语法高亮规则时，它也会执行 `after` 文件中的自定义命令。

例如，让我们对使用 `xml` 语法的 XML 文件进行定制。这意味着 Vim 从语法目录中的名为 `xml.vim` 的文件加载了语法定义。如前例所示，我们希望标识符始终为红色。因此我们在名为 `~/.vim/after/syntax` 的目录中创建自己的名为 `xml.vim` 的文件。在我们的 `xml.vim` 文件中，我们放入以下行：

```
highlight identifier ctermfg=red guifg=red
```

在此定制生效之前，我们必须确保 `~/.vim/after/syntax` 在 `runtimepath` 路径中：

```
:set runtimepath+=~/.vim/after/syntax      In our .vimrc file
```

为了使更改永久生效，当然该行应放入我们的 `.vimrc` 文件中。

现在，每当 Vim 为 `xml` 加载语法定义时，它将使用我们的自定义覆盖 `identifier` 的定义。

## 自行创建
凭借前几节的构建模块，我们现在有足够的知识来编写自己的语法文件，尽管它们可能很简单。在我们能够完全开发一个语法文件之前，还有许多方面需要学习。

我们将逐步构建自己的语法文件。由于语法定义可能极其复杂，让我们考虑一个足够简单易于理解，但又足够复杂以展示其潜在威力的例子。

考虑一个生成的拉丁文件 `loremipsum.latin` 的摘录：

```
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Proin eget
tellus. Suspendisse ac magna at elit pulvinar aliquam. Pellentesque
iaculis augue sit amet massa. Aliquam erat volutpat. Donec et dui at
massa aliquet molestie. Ut vel augue id tellus hendrerit porta. Quisque
condimentum tempor arcu. Aenean pretium suscipit felis. Curabitur semper
eleifend lectus. Praesent vitae sapien. Ut ornare tempus mauris. Quisque
ornare sapien congue tortor.

In dui. Nam adipiscing ligula at lorem. Vestibulum gravida ipsum iaculis
justo. Integer a ipsum ac est cursus gravida. Etiam eu turpis. Nam laoreet
ligula mollis diam. In aliquam semper nisi. Nunc tristique tellus eu
erat. Ut purus. Nulla venenatis pede ac erat.

...
```

你通过创建该语法名称的新文件来创建新的语法文件，在本例中为 `latin`。对应的 Vim 文件是 `latin.vim`，你可以在个人 Vim 运行时目录 `$HOME/.vim/syntax` 中创建它。然后只需使用 `syntax keyword` 命令创建一些关键字即可开始语法定义。选择 `lorem`、`dolor`、`nulla` 和 `lectus` 作为关键字，你可以用以下行启动语法文件：

```
syntax keyword identifier lorem dolor nulla lectus
```

编辑 `loremipsum.latin` 时仍然没有语法高亮。在高亮自动生效之前还需要做更多工作。但就目前而言，使用以下命令激活语法：

```
:set syntax=latin
```

文本现在应类似于 [图 11-25](#ch11.html#vi8-ch-11-fig-25)。

![](assets/lvv8_1125.png)

在屏幕上，文本单词为黑色，关键字为红色。在印刷书籍中，更难区分；关键字为深灰色而不是黑色。

你可能注意到 `Lorem` 的第一次出现没有被高亮。默认情况下，语法关键字区分大小写。在语法文件顶部添加以下行：

```
:syntax case ignore
```

然后你应该现在看到 `Lorem` 作为高亮关键字包含在内。

在再次尝试之前，让我们使其自动工作。Vim 在尝试检测任何文件类型后，会选择性地在你的 `runtimepath` 中的名为 `ftdetect` 的目录中检查其他定义，甚至覆盖定义（不推荐）。因此，在 `$HOME/.vim` 下创建该目录，并在其中创建名为 `latin.vim` 的文件，包含以下单行：

```
au BufRead,BufNewFile *.latin set filetype=latin
```
