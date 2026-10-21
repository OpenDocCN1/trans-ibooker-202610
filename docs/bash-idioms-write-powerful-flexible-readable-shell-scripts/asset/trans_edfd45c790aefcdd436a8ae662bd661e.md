

![](img/553ffa1bc15093729b691f706befc0ac_36_0.png)

## 非正则表达式

case 语句中使用的模式匹配不是正则表达式。在 bash 中，唯一允许使用正则表达式（regex 或 regexp）的地方是在 if 语句中使用 =~ 比较运算符。如果确实需要使用正则表达式，那么你需要使用一系列 if/then/else 语句而不是 case。

## 一个现实的使用案例

解析命令行选项的代码是常见的使用 case 语句的地方。让我们来看一个简单但相当现实的脚本，它很好地利用了 case 语句。

## 动机

如果你曾经使用过 Linux 或 Unix，你很可能大量使用了 ls 命令来列出文件名和相关信息。ls 的一些选项在提供更多信息或以特定方式排序时非常方便。你可能会养成使用 ls 或最常使用的选项的某些习惯。因此，你可能会创建一些别名甚至整个脚本来更方便地使用你喜欢的组合。但随后你最终会拥有多个不同的脚本。它们在功能上都相关但各不相同。我们如何将它们全部合并到一个脚本中？

考虑一个熟悉的例子，名为 Git 的流行源代码控制软件。它有几个相关但不同的功能，所有功能都使用同一个命令名 git 调用，但每个功能都由第二个不同的关键字区分，例如 git clone、git add、git commit、git push 等等。

## 我们的脚本

我们可以将这种“子命令”方法应用于我们的情况。让我们考虑一些我们想要使用的 ls 相关功能：按文件名长度顺序列出文件（连同长度），仅列出最长的文件名，列出最近修改的最后几个文件，以及使用颜色编码按文件类型列出文件名——这是 ls 的标准功能，但需要一些难以记住的选项。

我们的脚本将命名为 list，但会有第二个词来指定其中一个功能。那个词可以是 color、last、length 或 long。示例 3-1 包含一个执行此操作的脚本。

## 示例 3-1. 使用 case 的简单包装脚本

```
#!/usr/bin/env bash
# list.sh: A wrapper script for ls-related tools & simple `case..esac` demo
# Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch03/list.sh
# ----------------------------------------------------------------------------
VERSION='v1.2b'

function Usage_Exit {
    echo "$0 [color|last|len|long]"
    exit
}

# Show each filename preceded by the length of its name, sorted by filename
# length. Note '-' is valid but uncommon in function names, but it is not
# valid in variable names. We don't usually use it, but you can.
function Ls-Length {
    ls -1 "$@" | while read fn; do
        printf '%3d %s\n' ${#fn} ${fn}
    done | sort -n
}

(( $# < 1 )) && Usage_Exit
sub=$1
shift

case $sub in
    color)
        ls -N --color=tty -T 0 "$@"    # Colorized ls
    ;;
    last | latest)
        ls -lrt | tail "-n${1:-5}"     # Latest files
    ;;
    len*)
        Ls-Length "$@"                 # Files with name lengths
    ;;
    long)
        Ls-Length "$@" | tail -1       # File with longest name
    ;;
    *)
        echo "unknown command: $sub"    # Default
        Usage_Exit
    ;;
esac
```

我们不会在这里解释这个脚本的所有部分，尽管到本书结束时，你将学到所使用的所有功能。我们主要想关注 case 语句：

+   1. 你能认出非 if 的 if 逻辑吗？如果不能，请（重新）阅读第 1 章。
+   2. 这是两个词之间的简单“或”选择。
+   3. 如果 $1 中未给出值，则使用 tail -n5 作为默认值；请参阅第 38 页的“默认值”。
+   4. 此模式将匹配以“len”开头的任何词，因此“len”或“length”都会匹配，但“lenny”和“lens”也会匹配。它随后调用 Ls-Length 函数（我们在脚本中定义），并将传递给此脚本的所有命令行参数（如果有）传递给它。

## 包装脚本

每个人都有很多事情要做，很多需要记住的事情，所以当你可以自动化或编写脚本为你记住细节时，这就是胜利。我们在示例 3-1 中展示了一种“包装脚本”的方法，但根据你解决的问题的复杂性或你“记住”的细节，你可以使用许多有趣的变化和技巧。在示例 3-1 中，我们调用了一个函数或只是内联放置代码。对于非常短的代码块来说，这效果最好，在我们的经验中，这些在这种类型的包装脚本中非常常见。如果你有更复杂的解决方案，或者你正在使用现有工具，你可以调用那些工具或子脚本，不过你需要调整错误检查和可能的用法选项。你还可以将它与第 93 页的“Drop-in Directories”结合使用，并从目录中 source 所有“模块”，也许可以将代码部分的维护委托给其他人或团队。

这个更大的例子实际上是我们编写本书时使用的脚本的简化版和摘录。AsciiDoc 很酷，但我们使用很多标记语言，它们都混在一起了，所以我们可以编写一个工具来为我们记住事情，如示例 3-2 所示。

## 示例 3-2. 使用 case 的复杂包装脚本

```
#!/usr/bin/env bash
# wrapper.sh: Simple "wrapper" script demo
# Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch03/wrapper.sh
#_____________________________________________________________________________
# Trivial Sanity Checks
```

```
[ -n "$BOOK_ASC" ] || {
    echo "FATAL: export \$BOOK_ASC to the location of the Asciidoc files!"
    exit 1
}
\cd "$BOOK_ASC" || {
    echo "FATAL: can't cd to '$BOOK_ASC'!"
    exit 2
}

SELF="$0"
action="$1"
shift
[ -x /usr/bin/xsel -a $# -lt 1 ] && {
    # Read/write the clipboard on Linux
    text=$(xsel -b)
    function Output {
        echo -en "$*" | xsel -bi
    }
} || {
    # Read/write STDIN/STDOUT
    text=$*
    function Output {
        echo -en "$*"
    }
}

case "$action" in

########################################
# Content/Markup

### Headers
 h1 )
        # Inside chapter heading 1 (really AsciiDoc h3)
        Output "[[${$SELF id $text}]]\n=== $text"
    ;;
 h2 )
        # Inside chapter heading 2 (really AsciiDoc h4)
        Output "[[${$SELF id $text}]]\n==== $text"
    ;;
 h3 )
        # Inside chapter heading 3 (really AsciiDoc h5)
        Output "[[${$SELF id $text}]]\n===== $text"
    ;;

### Lists
 bul|bullet )
        # Bullet list (** = level 2, + = multiline element)
        Output "* $text"
    ;;
 nul|number|order* )
        # Numbered/ordered list (.. = level 2, + = multiline)
        Output ". $text"
    ;;
 term )
        # Terms
        Output "term_here::\n  $text"

```

```
;;

### Inline
bold )
    # Inline bold (O'Reilly prefers italics to bold)
    Output "*${text}*"
;;
i|italic|itl )      # Inline italics (O'Reilly prefers italics to bold)
    Output "_${text}_"
;;
c|constant|cons )   # Inline constant width (command, code, keywords, more)
    Output "+${text}+"
;;
type|constantbold ) # Inline bold constant width (user types literally)
    Output "*+${text}+*"
;;
var|constantitalic ) # Inline italic constant width (user-supplied values)
    Output "_+${text}+_"
;;
sub|subscript )     # Inline subscript
    Output "~${text}~"
;;
sup|superscript )   # Inline superscript
    Output "^${text}^"
;;
foot )              # Create a footnote
    Output "footnote:[${text}]"
;;
url|link )          # Create a URL with alternate text
    Output "link:\${\$text\$\$[]"   # URL[link shows as]
;;
esc|escape )        # Escape a character (esp. *)
    Output "\$\$${text}\$\$"  # $*$ $
;;

#####################################################
# Tools

id )                ## Convert a hack/recipe name to an ID
    #us_text=${text// /_}  # Space to '_'
    #lc_text=${us_text,,}  # Lowercase; bash 4+ only!
    # Initial `tr -s ' ' ' ' to preserve _ in case we process an ID
    # twice (like from "xref")
    # Also note you can break long lines with a trailing |
    Output $(echo $text | tr -s '_' ' ' | tr '[:upper:]' '[:lower:]' \
    | tr -d '[:punct:]' | tr -s ' ' '_')
;;

index )             ## Creates 'index.txt' in AsciiDoc dir
    # Like:
        # ch02.asciidoc== The Text Utils
        # ch02.asciidoc=== Common Text Utils and similar tools
```

