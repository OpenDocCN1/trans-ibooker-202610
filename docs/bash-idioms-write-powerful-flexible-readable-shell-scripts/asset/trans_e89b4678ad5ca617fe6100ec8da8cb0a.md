

你应该在你的参数处理/帮助选项中添加一个“handler”来输出你的文档。如果你使用 POD，就使用那些工具，但确保它们已安装！如果你使用其他标记，你必须以某种方式自行提取它。

我们知道这是一本 bash 书，但这个使用正则表达式和范围运算符的 Perl 单行命令非常方便：

```
perl -ne "s/^\t+//; print if m/DOCS'?'\$/ .. m/^\s*'?DOCS'?'\$/ \
and not m/DOCS'?'\$/;" $0
```

它会在匹配正则表达式 m/DOCS'?$/ 时开始打印行，并在匹配 m/^\s*'?DOCS'?$/ 时停止打印，但它根本不会打印包含 m/DOCS/ 的实际行。

将其添加到你的参数处理中作为“emit documentation”。

## More Docs AFTER the code

忽略此标题中的标注；它只在后面的输出中才有意义。

此块恢复为通用标记。我们*不*推荐像我们在这里所做的那样混合搭配！选择一种标记和一些工具并坚持使用。如果不确定，GitHub 已使 Markdown _非常_流行。

文档可以*在*代码结束之后。把所有文档放在脚本顶部或底部的同一个地方是有道理的。这使得底部变得容易。另一方面，保持文档 _靠近_ 相关代码是有道理的，尤其是对于函数。所以……由你决定。

但如果此部分上方只有 `exit 0` 并且未包装在 bogo-here-doc 中，这可能会导致某些语法高亮器不满意，并且我们的 Perl 文档发射器会遗漏它，因此你必须找到另一种方式来显示文档。

- 1. 当我们使用嵌入式文档正常运行脚本时，运行的就是代码，正如我们所期望和期望的那样。
- 2. 这代表一个将被运行的代码块。请注意，块之后的嵌入式文档将被忽略。
- 3. 另一个代码块；块之前和之后的嵌入式文档将被忽略。
- 4. 你能猜到吗？
- 5. 这是代码的结尾，之前的嵌入式文档如上所述被忽略，但我们现在退出脚本，因此此点之后的文档技术上不需要 here-document，但为了保持一致性并使语法高亮器满意，最好还是使用一个。

⑥ 现在我们正在发出文档！在这种情况下，我们有一个 --emit-docs 选项，但你也可以使用其他工具来提取和处理文档。

⑦ 这是通用标记。它可以是 Markdown、Textile，等等。

⑧ 这是 Perl POD，并通过各种 pod2* 工具处理。

⑨ 此块可能是 POD 或通用；重点是它被缩进了，因此我们需要在处理工具中处理这种情况。

⑩ 正如我们在 ⑤ 中讨论的那样，这是在代码结束之后。

参见以下内容：

- - https://en.wikipedia.org/wiki/Pandoc
- - https://en.wikipedia.org/wiki/Plain_Old_Documentation
- - https://en.wikipedia.org/wiki/Markdown
- - https://en.wikipedia.org/wiki/Textile_(markup_language)
- - https://en.wikipedia.org/wiki/Asciiidoc
- - https://en.wikipedia.org/wiki/Comparison_of_document_markup_languages
- - https://en.wikipedia.org/wiki/Lightweight_markup_language

## bash Debugging

我们已经在“Debug and Verbose”第 82 页讨论过在代码中添加调试语句、“printf for Reuse or Debugging”第 58 页和示例 10-3 中显示调试值，但我们直到现在才谈到解释器级别的调试。

首先，你可以通过运行 bash -n /path/to/script 来进行粗略的语法检查。技术上，bash -n 表示“读取命令但不执行它们”（参见 help set），它类似于 perl -c。这不会告诉你运行时错误、错误逻辑、外部命令的错误选项等。但它会告诉你引号、括号和方括号不平衡；bash 内置的无效语法；等等。这是在编写或更改脚本时早期和经常运行的一个很棒的命令。

有一个类似的 bash -v 或 set -v 命令，表示“打印 shell 输入行，因为它们被读取”，但实际上它没有听起来那么有用，因为它在 shell 插值之前显示，所以你只是得到源代码被吐回给你。

bash -x（xtrace），另一方面，表示“打印命令及其参数，因为它们被执行”，非常有用，这是调试的下一步。运行 bash -x /path/to/script，并准备好面对大量输出。参见示例 10-3，了解使该输出更有用的方法之一。你也可以在代码中间使用 set -x 打开 xtrace，然后稍后使用 set +x 关闭它（是的，这也让我们头疼了一点，但一旦你使用 - 打开它，使用 + 关闭它至少是可记忆的）。示例 10-20 说明了这一点。

### Example 10-20. Simple debug example

```
### Without debugging
$ examples/ch10/select-ssh.sh
1) Exit          3) gitlab.com      5) mythtv-be01
2) git.atlas.oreilly.com  4) github.com      6) kitchen
SSH to> 1
```

```
### With debugging (2 lone lines broken to fit)
$ bash -x examples/ch10/select-ssh.sh
+xtrace examples/ch10/select-ssh.sh:9:: ssh_config=examples/ch10/ssh_config
+xtrace examples/ch10/select-ssh.sh:11:: PS3='SSH to> '
+xtrace examples/ch10/select-ssh.sh:12:: Select ssh_target in Exit $(egrep \
-i 'Host \w+' "$ssh_config" | cut -d ' ' -f2-)
++xtrace examples/ch10/select-ssh.sh:12:: egrep -i '^Host \w+' \
examples/ch10/ssh_config
++xtrace examples/ch10/select-ssh.sh:12:: cut '-d ' -f2-
1) Exit          3) gitlab.com      5) mythtv-be01
2) git.atlas.oreilly.com  4) github.com      6) kitchen
SSH to> 1
+xtrace examples/ch10/select-ssh.sh:13:: case $REPLY in
+xtrace examples/ch10/select-ssh.sh:14:: exit 0
```

你可能想看看第 96 页的“Unofficial bash Strict Mode”，它可以在稍后添加时揭露微妙问题，或在编写时防止它们出现。

![](img/553ffa1bc15093729b691f706befc0ac_136_0.png)

## Dumping Your Environment

当从 cron、持续集成或其他环境可能发生变化或与你期望的不同之处运行时，记录或转储你的环境通常很有帮助。你会看到外部 env 或 printenv 用于此目的，但 bash 内部的 set 命令会给你更多信息，包括函数。另一方面，它嘈杂得多，所以全部检查一下并决定哪个最适合你。

![](img/553ffa1bc15093729b691f706befc0ac_137_0.png)

## Environment Information Leaks

如果你记录或转储你的环境，请注意它通常会包含敏感信息，例如用户名、密码、API 密钥等等！你可能希望修改你的转储命令以删除数据，也许使用 sed 或 Perl 单行命令：

```
set | perl -pe 's/^(SECRET=).*\/1<REDACTED>/g;'
```

从 bash v3.0 开始，有一个 --debugger 标志和 shopts extdebug 选项，但在这里我们必须诚实地说我们从未使用过这些选项；set -x 一直足够。运行 bash --debugger -- /path/to/script 很可能会导致：

/path/to/script: /usr/share/bashdb/bashdb-main.inc: No such file or directory
/path/to/script: warning: cannot start debugger; debugging mode disabled

如果你看到这个，你需要查看 Bash Debugger Project，它是“一个遵循 gdb 命令语法的 bash 源代码调试器”。它是 bash 版本相关的，所以确保为你的系统获取正确的版本。

另请参阅：

- - http://bashdb.sourceforge.net
- - Example 10-3
- - “bash Linter”第 134 页
- - “Debug and Verbose”第 82 页
- - “printf for Reuse or Debugging”第 58 页
- - “Unofficial bash Strict Mode”第 96 页

## bash Unit Testing

在 GitHub 上，有一个“Bourne-based shell 脚本的单元测试框架，它的设计方式与 JUnit、PyUnit 等类似”……以及第 134 页的“bash Linter”，我们将在第 11 章讨论，这两者都很有趣。

## Summary

希望这些提示和技巧在你日常的 bash 工作中以及在规划未来增长以保持尽可能简单和灵活时会有用。如果你喜欢这一章，你也会喜欢 bash Cookbook，它有超过 600 页的、嗯、像这样的有用食谱。

