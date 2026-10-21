## 嵌入式文档

程序员有不喜欢编写或更新文档的名声。有时确实如此，有时你遇到的程序员就像……嗯，就是我们。但很明显，编写或更新文档涉及的摩擦越多，发生的可能性就越小。你的团队对摩擦的定义以及如何减少它会有所不同，但一个可能有帮助的方法是将文档嵌入代码中。各种语言找到了不同的方式来实现这一点，你也可以在 bash 中做到。

你总可以有注释，我们已经展示了一些将注释用作使用或帮助文档的一部分的方法（参见第 79 页的“HELP!”）。并且很容易将文档放在代码旁边，比如 myscript.sh 和 myscript.asciidoc。但你可以将真正的文档，以 wiki 或标记语言的形式，嵌入到与代码相同的文件中，如示例 10-18 所示。

请注意，此示例是 Perl POD（Plain Old Documentation）和通用标记的拼凑物；你通常应该只选其一。POD 很老，与 Markdown、Textile 等相比感觉非常笨拙，但它有一些不错的转换工具，特别是对于 man 页面，如果你使用 Perl 和/或这些工具，它可能仍然是不错的选择。对于其他标记，Pandoc 是一个很棒的转换器——例如，pandoc README.md > /tmp/README.html && firefox file:///tmp/README.html。

请务必阅读示例或输出，因为我们……嗯，嵌入了一些更多的讨论和提示。另外，标注在源代码中看起来会很奇怪且顺序不对，因为它们只有在最终输出中才有意义。继续阅读即可。

### Example 10-18. bash embedded documentation example: code

```
#!/usr/bin/env bash
# embedded-docs.sh: Example of bash code with various kinds of embedded docs
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch10/embedded-docs.sh
##### ----------------------------------------------------------------------------
# Does not work on Zsh 5.4.2!

[[ "$1" == '--emit-docs' ]] && {
  # Use the Perl "range" operator to print only the lines BETWEEN the bash
  # here-document "DOCS" markers, excluding when we talk about this code
  # below in the docs themselves. See the output for more.
  perl -ne "s/^\t+//; print if m/DOCS'?\$/ .. m/^\s*'?DOCS'?\$/ \
    and not m/DOCS'?\$/;" $0
  exit 0
}
```

```
}
echo 'Code, code, code... <2>'
echo 'Code, code, code...'
: << 'DOCS'
```

# === Docs for My Script ⑦

Ignore the callout in this title; it only makes sense in the output later.

Docs can be Markdown, AsciiDoc, Textile, whatever. This block is generic markup.

We've wrapped them using the no-op ':' operator and a here-doc, but you have to remember, it's not bash that's processing the here-docs, so using <<-DOC for indenting will not work. Not quoting your marker will not allow variable interpolation either, or rather, it will, but that won't affect your documentation output. So always quote your here-doc marker so your docs do not interfere with your script (e.g., via the backticks we'll use below).

All of your docs could be grouped near the top of the file, like this, for discoverability. Or they could all be at the bottom, to stay grouped but out of the way of the code. Or they could be interspersed to stay near the relevant code. Do whatever makes sense to you and your team.

```
DOCS
echo 'More code, code, code... <3>'
echo 'More code, code, code...'
: << 'DOCS'
```

# =head1 POD Example ⑧

Ignore the callout in this title; it only makes sense in the output later.

This block is Perl POD (Plain Old Documentation).

If you use POD, you can then use `perldoc` and the various `pod2*` tools, like `pod2html`, that handle that. But you can't indent if using POD, or Perl won't see the markup, unless you preprocess the indent away before feeding the POD tools.

And don't forget the `=cut` line!

```
=cut
```

```
DOCS
echo 'Still more code, code, code... <4>'
echo 'Still more code, code, code...'
: << 'DOCS'
```

# Emitting Documentation ⑨
-------------------

Ignore the callout in this title; it only makes sense in the output later.

This could be POD or markup, whatever.

This section uses a TAB indented here-doc, just because we can, but we handle that in the Perl post processor, not via bash. :-/

You should add a "handler" to your argument processing/help options to emit your docs. If you use POD, use those tools, but make sure they are installed! If you use some other markup, you have to extract it yourself somehow.

We know this is a bash book, but this Perl one-liner using regular expressions and the range operator is really handy:
    perl -ne "s/^\t+//; print if m/DOCS'?\$/ .. m/^\s*'?DOCS'?\$/ \n    and not m/DOCS'?;/" $0

That will start printing lines when it matches the regular expression m/DOCS'?$/, and stop printing when it matches m/^\s*'?DOCS'?$/, except that it won't print the actual line containing m/DOCS/ at all.

Add that to your argument processing as "emit documentation."

DOCS

```
echo 'End of code... <5>'
```

```
exit 0  # Unless we already exited >0 above
```

```
: << 'DOCS'
h2. More Docs AFTER the code ⑩
```

Ignore the callout in this title; it only makes sense in the output later.

This block is back to generic markup. We do *not* recommend mixing and matching like we've done here! Pick a markup and some tools and stick to them. If in doubt, GitHub has made Markdown _very_ popular.

Docs can just go *after* the end of the code. There's an argument for putting all the docs together in one place at the top or bottom of the script. This makes the bottom easy. On the other hand, there's an argument for keeping the docs _close_ to the relevant code, especially for functions. So...your call.

But if this section only has an `exit 0` above it and is not wrapped in a bogo-here-doc, this might cause some syntax highlighters to be unhappy, and our Perl doc emitter will miss it, so you have to find a different way to display the docs.
DOCS

And that looks like Example 10-19.

# Example 10-19. bash embedded documentation example: output

### Running looks like this (`./embedded-docs.sh`): ①
Code, code, code... ②

Code, code, code...
More code, code, code... ③
More code, code, code...
Still more code, code, code... ④
Still more code, code, code...
End of code... ⑤

# ### Emitting (un-rendered) docs looks like this (`./embedded-docs.sh --emit-docs`): ⑥

# === Docs for My Script ⑦

Ignore the callout in this title; it only makes sense in the output later.

Docs can be Markdown, AsciiDoc, Textile, whatever. This block is generic markup.

We’ve wrapped them using the no-op ‘:’ operator and a here-doc, but you have to remember, it’s not bash that’s processing the here-docs, so using <<-DOC for indenting will not work. Not quoting your marker will not allow variable interpolation either, or rather, it will, but that won’t affect your documentation output. So always quote your here-doc marker so your docs do not interfere with your script (e.g., via the backticks we’ll use below).

All of your docs could be grouped near the top of the file, like this, for discoverability. Or they could all be at the bottom, to stay grouped but out of the way of the code. Or they could be interspersed to stay near the relevant code. Do whatever makes sense to you and your team.

# =head1 POD Example ⑧

Ignore the callout in this title; it only makes sense in the output later.

This block is Perl POD (Plain Old Documentation).

If you use POD, you can then use `perldoc` and the various `pod2*` tools, like `pod2html`, that handle that. But you can’t indent if using POD, or Perl won’t see the markup, unless you preprocess the indent away before feeding the POD tools.

And don’t forget the `=cut` line!

=cut

# Emitting Documentation ⑨

---------------------

Ignore the callout in this title; it only makes sense in the output later.

This could be POD or markup, whatever.

This section uses a TAB indented here-doc, just because we can, but we handle that in the Perl post processor, not via bash. :-/





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
