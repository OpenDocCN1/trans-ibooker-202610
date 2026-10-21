## Arguing

Some scripts are meant to do a single task; they need no variations. Many others, though, take arguments: one or more filenames, or options to provide variations on their behavior. Once you have more than a single option (or maybe two), you need to parse those arguments in an orderly fashion to be sure that you’ve covered all the possible ways that a user of that script might order those arguments. And come to think of it, even that single task script probably wants -h (or even --help). Let’s take a look at how to parse those arguments and still have a readable, maintainable script.

### Your First Argument

If your script just wants a single parameter, you can reference that in your script as $1. You might have statements like echo $1 or cat $1 as part of your script. We don’t recommend using $1 throughout your script as it doesn’t tell the reader anything about this parameter. It’s better, for readability’s sake, if you assign this parameter to a variable with an informative name. If the parameter is meant to be a filename, then choose a variable name like in_file or pathname or similar and assign it right away, early in the script. As we saw in “Default Values” on page 38, we can even supply a default value:

```
filename=${1:-favorite.txt}    # Or maybe use /dev/null as the default?
```

If the user doesn’t supply any parameter when invoking your script, $1 will be unset. In the preceding example, the shell will assign favorite.txt as the value when parameter one is unset.

Need a second or third parameter or more? As you might have guessed, those would be $2, $3, and so on. They would be unset if no such parameter is supplied when invoking the script.

But what if you don’t have good default values? Or what if you don’t want to proceed when the user doesn’t supply the correct number of arguments? Your script can check to make sure that the user has supplied the correct number of arguments by checking the $# variable. It holds the number of arguments supplied. If $# is 0, then the user invoked the script with no arguments. If $# is 1 but you wanted two arguments, then don’t proceed. Issue an error message and exit (see “Exit Codes” on page 96):

```
if (($# != 2)); then
    echo "usage: $0 file1 file2"    # $0 is the script's name, as invoked
    exit
fi
```

We’re only looking here at how to know that you’ve got the right number of arguments to your script. How you use all these arguments within the script is quite varied. We looked at some of those in Chapter 2.

Traditionally, when you wanted a list of all the arguments, you might have used $* to write something like echo $*, which works fine for this simple output. But once spaces were allowed as part of a filename, a slightly different syntax became the better choice.

Quotes are needed around a filename with embedded spaces in the name (e.g., “my file”), otherwise the shell sees that as two separate words. To refer to all the arguments to a script and have each one quoted, we use "$@" (string) or "${@}" (list). If we use "$*", that will give us one large quoted string containing all the arguments. For example, if we invoke a script like this:

```
myscript file1.dat "alt data" "local data" summary.txt
```

then "$*" would result in the single value “file1.dat alt data local data summary.txt,” whereas "$@" would result in four separate words: file1.dat "alt data" "local data" summary.txt.

Yes, we know we talked about this in “Quotes and Spaces” on page 14 and then kept harping on it again in Chapter 7. It’s just tricky and bears repeating.

### Having Options

Options are ways to vary (slightly?) the behavior of a command. The classic (idiomatic?) way to specify options to a Unix or Linux command is with a single letter preceded by a -, called a minus, a dash, or a tack. For example, to have a command produce its long-form output, you might specify that with -l. Similarly, to have a command run in a more quiet mode (less output), you might specify -q right after the command name.

Not every command or script has these options, and not every command or script that has, for example, a -q option uses that option to specify a “quiet” output format.





它可能在另一个命令中表示“快速”。在另一个命令中它可能根本不是一个有效选项。这些都是基于现有命令的约定——可以说是传统。

除非有令人信服的理由放弃它，否则遵守传统是值得的。原因之一是它降低了学习曲线。你在运行某些命令或脚本时学到的知识可以迁移到其他命令中。它还意味着你可以使用他人使用的相同技术来解析选项。

## 解析选项

使用 getopts 内置命令来解析 shell 脚本的选项。它会被重复调用（通常通过 while 循环），直到找到所有选项。它假定所有选项都出现在其他参数之前。它可以识别单独的选项 (-a -v) 以及组合在一起的选项 (-av)。你可以指定某个选项必须提供额外的参数。例如，你可能希望 -o 选项指定输出文件，因此用户会使用 -o filename 或 -ofilename 调用它，这两种格式都由 getopts 支持。

让我们看一下示例 8-1，它处理了这类选项。

### 示例 8-1. getopts 参数解析：简单

```
#!/usr/bin/env bash
# parseit.sh: Use getopts to parse these arguments
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch08/parseit.sh
#___________________________________________________________________________

while getopts ':ao:v' VAL ; do
  case $VAL in
    a ) AMODE=1 ;;
    o ) OFILE="$OPTARG" ;;
    v ) VERBOSE=1 ;;
    : ) echo "error: no arg supplied to $OPTARG option" ;;
    * ) echo "error: unknown option $OPTARG"
        echo "  valid options are: aov"
  ;;
  esac
done
shift $((OPTIND -1))
```

1. 我们编写 while getopts 是因为我们希望重复调用 getopts，而 getopts 命令在找到选项时（一个破折号后跟任意字母，无论是否有效）会返回 true，并在到达选项末尾时返回 false。getopts 内置命令需要给出两个词。第一个是选项列表，第二个是它将在解析命令行时找到的选项将被放入其中的变量名。它每次调用只会找到一个选项，因此我们通过 while 循环重复调用它。在我们的示例（':ao:v'）中，我们希望支持选项 a、o 和 v。开头的冒号告诉 getopts 不要报告错误消息，而是留给我们的脚本处理。o 字符后的冒号表示 o 选项有一个随之而来的参数。VAL 是如果找到选项将被赋值的变量名。

2. 一旦调用了 getopts，我们就可以使用 case 语句查看找到的是哪个选项。（有关 case 语句的更多信息，请参见第 3 章。）

3. 关于错误处理，由于我们要求 getopts 不要发出错误消息，我们需要两个 case 来处理错误。第一个情况是如果 -o 选项没有提供参数。我们已通过在 ':ao:v' 中在 o 后放置“：”来告诉 getopts 预期一个参数。如果在脚本调用时未给出参数，则 bash 会将 $VAL 设置为冒号，并将 $OPTARG 设置为我们找不到其参数的字符（在这种情况下为 o）。

4. 其次，如果用户提供了一个不在我们有效选项列表中的选项，则 $VAL 将被赋值为 '?'（问号字符），并且它将 shell 变量 $OPTARG 设置为它无法识别的选项字符。我们使用 case 语句中的通配符模式 (*) 处理这种情况。

5. 当 getopts 完成且 while 循环退出时，还有一个重要的步骤要执行。getopts 内置命令在解析过程中沿着命令行参数移动时，会跟踪其在命令行参数中的位置。$OPTIND 是它保存下一个要考虑的参数的索引的位置。现在所有这些参数都已解析，我们添加 shift $OPTIND -1 以将所有与选项相关的参数从进一步考虑中移除。

无论你使用 myscript -a -o xout.txt -v file1 还是仅使用 myscript file1 调用脚本，在 shift 完成其工作后，$1 之后将引用 file1，因为中间的选项参数已被移除。脚本的参数现在只是剩余的（非选项）参数。

## 长参数

对于一些人来说，单个字母是不够的。他们希望使用完整的单词甚至短语来描述他们调用的选项。getopts 内置命令也支持这些。

我们需要区分长选项和串联在一起的多个单字母选项（例如，-last 是指 -l -a -s -t，还是指长选项 last？）。

约定是长选项以两个破折号开头。（在此示例中，我们将写 --last 来指定长选项。）

要使用 getopts 解析长选项，我们在选项列表中添加一个减号和冒号，然后添加另一个 case 语句来识别每个长参数。我们即使在长选项不带参数的情况下也包含冒号。（我们将在后面的章节中解释原因。）

示例 8-2 显示了之前的示例，该示例解析了一组选项，并扩展为包含几个长参数。这将处理的两个长参数是 --amode，它将与较短的 -a 选项相同；以及 --outfile，它像 -o 一样需要参数。对于长选项，选项及其对应参数可以通过两种方式提供：使用等号作为单个词，或作为两个词。例如，在调用脚本时，你可以写 outfile=file.txt 或 outfile file.txt 来指定输出文件选项。getopts 和这个第二个 case 语句可以处理这两种格式。

### 示例 8-2. getopts 参数解析：长

```
#!/usr/bin/env bash
# parselong.sh: Use getopts to parse these arguments, including long ones
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch08/parselong.sh
#
# Long arguments: --amode
#               and --outfile filename or --outfile=filename

VERBOSE=':'  # Default is off (no-op)

while getopts ':-:ao:v' VAL ; do
  case $VAL in
    a ) AMODE=1 ;;
    o ) OFILE="$OPTARG" ;;
    v ) VERBOSE='echo' ;;
#--------------------------------------------------------------------
    - ) # This section added to support long arguments
      case $OPTARG in
        amode ) AMODE=1 ;;
        outfile=* ) OFILE="${OPTARG#*=}" ;;
        outfile )
                  OFILE="${!OPTIND}"
                  let OPTIND++
                  ;;
        verbose ) VERBOSE='echo' ;;
        * )
          echo "unknown long argument: $OPTARG"
          exit
          ;;
      esac
esac
```
```
#- --------------------------------------------------
    ;; 
    : ) echo "error: no argument supplied" ;;
    * ) 
    echo "error: unknown option $OPTARG"
    echo "      valid options are: aov"
    ;;
esac
done
shift $((OPTIND -1))
```

这如何支持长格式选项？这里到底发生了什么？

- 1. 考虑 getopts 是为处理单字符选项（例如 -a）而设计的。将减号添加到选项列表意味着 -- 将被识别为有效选项。

- 2. 两个减号后的任何剩余字符都被视为 -- 选项的“参数”，并放入 $OPTARG 变量中。我们可以使用内部 case 语句将 $OPTARG 的值与我们想要实现的长度选项名称进行匹配。

- 3. 那么，对于需要用户提供参数的选项，例如 --outfile，会发生什么？如果参数使用等号语法提供（例如 --outfile=my.txt），则 getopts 会将整个字符串（-- 之后的部分）分配给 OPTARG。我们可以使用 bash 的参数扩展字符串操作（参见第 32 页的“参数扩展”）从选项字符串中提取参数，使用 (#) 删除所有字符 (*) 直到且包括等号 (=) 的表达式。那就只剩下等号后的字符，这就是我们想要的参数。我们也可以更字面地编码要删除的字符串，例如使用 OFILE="${OPTARG#outfile=}" 来从 OPTARG 前面删除该文本。

- 4. 当参数作为单独的词提供时，我们 case 语句中的第二个 outfile 模式将被采用。这里我们使用了一个变量 $OPTIND，getopts 用它来跟踪其解析。文件名参数使用 ${!OPTIND} 间接检索。它是如何工作的？

- 5. 使用 getopts 时，shell 变量 $OPTIND 保存 getopts 下一个要处理的参数的索引。! 表示使用间接引用——取 $OPTIND 的值并将其用作要检索的变量的名称。例如，如果 --output 是 getopts 遇到的第三个参数，则此时 $OPTIND 将具有值 4，因此 ${!OPTIND} 将返回 ${4} 的值，即下一个参数，即文件名。





6 然后我们还需要“移动” $OPTIND，因为我们已经处理过它了。

其余脚本与我们之前介绍的一样。

## HELP!

前面的示例中有一个明显的遗漏；你需要帮忙找出它吗？（那是个提示。）对，-h 和/或 --help 缺失了。我们强烈建议按需输出某种使用说明或帮助信息；它易于发现，与大多数其他工具的工作方式一致，而且一旦你知道方法就很容易实现。

如果你有一个非常简单的脚本，示例 8-3 显示了一个快速而粗糙的解决方案。

### Example 8-3. Quick-and-dirty help

```
PROGRAM=$({0##*/})  # bash version of `basename`
if [ $# -lt 2 -o "$1" = '-h' -o "$1" = '--help' ]; then
  # Tab indents below, starting after the EoH (End-of-Help) line!
  cat <<-EoH
    This script does nothing but show help; a real script should be more exciting.
    usage: $0 <foo> <bar> (<baz>)

    As you can see, there are two required arguments, +foo+ and +bar+, and one
    optional argument, +baz+.
    e.g.
    usage: $PROGRAM foo bar
    usage: $PROGRAM foo bar baz

    You can put more detail here if you need to.
  EoH
  # Tab indents above!
  exit 1
fi
```

1 我们在第 32 页的“Shorthand for basename”中谈到过这个。

2 如果 $#（参数计数）是 -lt（小于）2，或者第一个参数是 -h，或者第一个参数是 --help，则使用一个 here-document 来显示帮助和使用信息。（我们在第 98 页的“Here-Documents and Here-Strings”中讨论了这些。）

3 启动一个 <<（here-document）直到我们到达 EoH，即 end-of-help，但也可以是 EOF 或你喜欢的任何其他名称。它包含一个 -，因此会剥离块中开头的制表符（但不剥离空格），这允许你在代码中缩进而不在输出中缩进。

4 end-of-help 标记也用制表符缩进以匹配其余内容。

5 我们可以争论在那里 exit code（“Exit Codes” 第 96 页）exit 0 表示成功还是 exit 1（实际上大于零的任何值）表示失败是否正确。如果我们请求帮助，那么我们刚刚得到了它，并且它成功了，所以 exit 0。但如果我们没有提供正确的参数，那么我们就失败了，所以 exit 1。此外，脚本除了显示消息之外未能真正做任何有用的事情，所以这也可以算作 exit 1 失败。选择一个标准并坚持下去，但 exit 1 是一个更好的 fail-safe。

我们还在这里内联了 help/usage 消息，但有时让它成为自己的函数是有意义的，这样如果需要你就可以从其他地方调用它，正如我们在示例 8-4 中看到的那样。

### Example 8-4. getopts argument parsing: long with help

```
#!/usr/bin/env bash
# parselonghelp.sh: Use getopts to parse these arguments, including long & help
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch08/parselonghelp.sh
#
# Long arguments: --amode and --help
#                 and --outfile filename or --outfile=filename

PROGRAM=${0##*/}  # bash version of `basename`
VERSION="$PROGRAM v1.2.3"

VERBOSE=':'   # Default is off (no-op)
DEBUG=':'     # Default is off (no-op)

function Display_Help {
    # Tab indents below, starting after the EoN (End-of-Note) line!
    cat <<-EoN
This script does nothing but show help; a real script should be
more exciting.
        usage: $PROGRAM (options)

Options:
    -a | --amode    = Enable "amode", default is off
    -d | --debug    = Include debug output, default is off
    -h | --help     = Show this help message and exit
    -o | --outfile  = Send output to file instead of STDOUT
    -v | --verbose  = Include verbose output, default is off
    -V | --version  = Show the version and exit

You can put more detail here if you need to.
EoN
    # Tab indents above!
    # If we have this next line, the script will always exit after calling
```

```
# Display_Help. You may or may not want that...you decide.
# exit 1 # If you use this, remove the other exits after the call!
} # end of function Display_Help

while getopts ':-adho:vV' VAL ; do
case $VAL in
# If you keep options in lexical order, they are easier to find and
# you reduce the chances of a collision
a ) AMODE=1 ;;
d ) DEBUG='echo' ;;
h ) Display_Help ; exit 1 ;;        # We violated our style here
o ) OFILE="$OPTARG" ;;
v ) VERBOSE='echo' ;;
V ) echo "$VERSION" && exit 0 ;;   # We violated our style here too
#-------------------------------------------------------------
- ) # This section added to support long arguments
    case $OPTARG in
        amode      ) AMODE=1 ;;
        debug      ) DEBUG='echo' ;;
        help       )
            Display_Help
            exit 1
            ;;
        outfile=* ) OFILE="${OPTARG#*=}" ;;
        outfile    )
            OFILE="${!OPTIND}"
            let OPTIND++
            ;;
        verbose    ) VERBOSE='echo' ;;
        version    )
            echo "$VERSION"
            exit 0
            ;;
        * )
            echo "unknown long argument: $OPTARG"
            exit
            ;;
    esac
    ;;
#-------------------------------------------------------------
: ) echo 'error: no argument supplied' ;;
* )
    echo "error: unknown option $OPTARG"
    echo "  valid options are: aov"
    ;;
esac
done
shift $((OPTIND -1))

echo "Code for $0 goes here."

$VERBOSE 'Example verbose message...'

```

```
$DEBUG 'Example DEBUG message...'

echo "End of $PROGRAM run."
```

我们把很多内容都打包到这个示例中，但你已经见过其中的大部分。我们在 long arguments 示例的基础上添加了 help，但我们还添加了一种简单的方法来拥有 debug 和 verbose 标志和输出。我们现在为 Display_Help 创建了一个函数，因为我们需要从至少两个地方调用它：短选项和长选项处理。这应该很清楚。

我们还违反了我们关于“one-liners”的 case..esac 风格指南，因为 exit 命令使其成为多行块。我们完全可以像在长选项部分那样将其展开，但我们从一行变成了四行……对清晰度没有提升，反而损失了三行垂直空间，而这些空间可能更适合显示更重要的代码：

```
h ) Display_Help ; exit 1 ;;
# Versus
h )
    Display_Help
    exit 1
;;
```

## Debug and Verbose

在示例 8-4 中，我们将冒号设为变量 VERBOSE 的默认值。这可能看起来是个奇怪的值，但考虑一下脚本后面对该变量的示例用法：

```
$VERBOSE 'Example verbose message...'
```

在默认情况下（如果用户未指定 verbose 模式），执行该行时，$VERBOSE 会被替换为 :；即 no-op（无操作）或 null 命令（什么都不做，忽略其参数，并且始终为真）。因此不会打印任何消息。但如果用户使用 -v 或 --verbose 调用脚本，则 $VERBOSE 会被替换为 echo，而要执行的行在效果上现在是这样的：

```
echo 'Example verbose message...'
```

消息将被打印给用户查看。

这种用于条件消息的习惯用法是简单易写、易读的代码，甚至在编写文档时也很容易 grep。你可以根据需要对 DEBUG 使用相同的方法。

## Version

就像 -h 和/或 --help 一样，version 命令也很有用。首先，不过是 -v 表示 version 还是 verbose？我们更喜欢 -V 表示 version，-v 表示 verbose（如果适用）。其次，你需要 version 吗？没有 help 那么重要，所以这真的取决于情况。像 CVS 和 SVN 这样的 gen2 版本控制系统让 version 变得容易——只需使用类似 VERSION=$Id$ 的关键字扩展。但像 Git 这样的 gen3 系统不支持这一点，所以你又回到了手动更新变量，或某种构建时或 CI（持续集成）递增器。对于较大或公开的脚本，这是有意义的；对于较小的个人脚本，可能就不需要了。由你决定。

你通过 VERSION=something 实现 version 并以与 help 相同的方式显示它。选择这个的 exit code（“Exit Codes” 第 96 页）更容易，因为你只有在请求时才会得到 version 输出，所以 exit 0 显然是有意义的。

你将 $VERSION 设为什么取决于你：

- VERSION=$Id$ 用于 CVS 或 SVN
- VERSION=v1.2.3
- VERSION="$PROGRAM v1.2.3"
- VERSION="$0 v1.2.3" # 可能不合适，因为 $0 会变化
- VERSION=12

#### Style and Readability: Recap

在本章中，我们讨论了 CLI 选项、参数和使用方法。我们展示了如何解析单个参数以及如何通过检查 $# 来在参数不足时不继续执行。然后我们描述了 bash 内置的 getopts 作为解析短格式和长格式选项的好方法。

任何具有一定数量参数解析的脚本都应努力将该代码与脚本的核心功能分开。这通常最好通过在 case 语句中设置标志变量来实现，这些标志变量可以在脚本后面在功能实现的地方引用。通过将参数解析作为脚本中的一个单独函数，你可以极大地提高脚本的可读性。将示例 8-4 包裹在一个函数定义中：

```
# Called like: parseargs "${@}"
function parseargs {
  ...
}
```

然后用 parseargs "${@}" 调用它，你的脚本其余部分就可以跟随，使用 parseargs 函数中设置的标志。





添加大量条件调试信息或扩展（详细）输出会在这些语句被 if 语句包裹时削弱简洁性和整体可读性。它们会使代码逻辑变得杂乱和复杂。我们展示了一种习语，提供相同的输出选择，但避免了使用 if 语句的需求，使编码风格更简单，可读性更高。

## Files and More

是什么让一个普通的旧文件成为 shell 脚本，以及你如何发出退出码？你如何读取文件？我们将在本章中讨论所有这些内容——以及更多。

## Reading Files

将文件读入 bash 脚本有三种主要惯用方法。其中一些会将整个文件“slurp”到内存中，而其他方法则一次处理一行。

### read

我们已经在第 2 章中使用 read 处理键/值对，并在第 104 页的“Getting User Input”中再次看到它，但另一个主要用途是逐行读取文件并解析输入：

```
$ grep 'nobody' /etc/passwd | read -d':' user shadow uid gid gecos home shell

$ echo "$user | $shadow | $uid | $gid | $gecos | $home | $shell"
```

等等，发生了什么？我的数据去哪了？好吧，这是一个陷阱——它进入了由管道 (|) 创建的子 shell，并且再也没有出来。那这个呢？

```
$ grep 'nobody' /etc/passwd | { \
    read -d':' user shadow uid gid gecos home shell; \
    echo "$user | $shadow | $uid | $gid | $gecos | $home | $shell" \
}
nobody | | | | | |
```

好一点，但剩下的去哪了？好吧，-d 是行尾分隔符，而不是字段分隔符 ($IFS)。再试一次：

```
$ grep '^nobody' /etc/passwd | { \
  read -d':' user shadow uid gid gecos home shell; \
  echo "$user | $shadow | $uid | $gid | $gecos | $home | $shell" \
}
nobody | x | 65534 | 65534 | nobody | /nonexistent | /usr/sbin/nologin
```

另见第 89 页的“Fiddling with $IFS for Fun and Profit, to Read Files”。

![](img/553ffa1bc15093729b691f706befc0ac_101_0.png)

### lastpipe

如果你运行的是 bash 4+，可以设置 shopt -s lastpipe，这将使管道的最后一个命令在当前 shell 中运行，以便你的脚本可以看到环境。请注意，这仅在作业控制被禁用时才有效，而这是在脚本中默认的，但在交互式会话中不是。你可以使用 set +m 禁用作业控制，但这会禁用 CTRL-C、CTRL-Z、fg 和 bg，所以我们不建议这样做。

### mapfile

mapfile 也称为 readarray，但它们是同一个命令。添加到 bash v4 中，mapfile 将文件读入数组（列表）并具有选项可一次读取仅 -n count 行、跳过 -s count 行、显示进度指示器（-c/-C）等。这比其他方法更容易使用。

简单的用法，嗯，很简单，它会将整个文件“slurp”到内存中，如示例 9-1 所示。

Example 9-1. Simple mapfile

```
mapfile -t nodes < /path/to/list/of/hosts  # -t removes newlines
```

```
# Loop around the nodes
for node in ${nodes[@]}; do
  ssh $node 'echo -e "$HOSTNAME:\t$(uptime)"'
done
```

使用 -n 会稍微复杂一些，因为虽然 mapfile 会继续读取无内容，所以你必须检查它是否真的获取了数据（${#nodes[@]} 非零），否则你会创建一个无限循环（示例 9-2）。

### Example 9-2. Batch mapfile example

```
BATCH=10
# While we're reading data...   && there is still data there!
while mapfile -t -n $BATCH nodes && (( ${#nodes[@]} )); do
    for node in ${nodes[@]}; do
        ssh $node 'echo -e "$HOSTNAME:\t$(uptime)"'
    done
done < /path/to/list/of/hosts
```

当然，你可以做得更花哨，并添加一些运行时用户反馈。你可能还需要出于各种原因限制你的进程，所以你可以做类似示例 9-3 中的代码。

### Example 9-3. Fancy mapfile example: code

```
#!/usr/bin/env bash
# fancy_mapfile.sh: Fancy `mapfile` example
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch09/fancy_mapfile.sh
##### ----------------------------------------------------------------------------
# Does not work on Zsh 5.4.2!

HOSTS_FILE='/tmp/nodes.txt'

# Create test file
> $HOSTS_FILE
for n in node{0..9}; do echo "$n" >> $HOSTS_FILE; done

### ADJUSTABLE VARIABLES
#BATCH_SIZE=0  # Do the entire file at once (default); watch out for memory
BATCH_SIZE=4
SLEEP_SECS_PER_NODE=1     # Can set to 0
SLEEP_SECS_PER_BATCH=1    # Set to zero if `BATCH_SIZE=0`!

# Display runtime feedback to STDERR (so STDOUT can go into `tee` or a file)
node_count="$(cat $HOSTS_FILE | wc -l)"
batch_count="$(( node_count / BATCH_SIZE ))"
echo '' 1>&2
echo "Nodes to process:          $node_count" 1>&2
echo "Batch size and count:      $BATCH_SIZE / $batch_count" 1>&2
echo "Sleep seconds per node:    $SLEEP_SECS_PER_NODE" 1>&2
echo "Sleep seconds per batch:   $SLEEP_SECS_PER_BATCH" 1>&2
echo '' 1>&2

node_counter=0
batch_counter=0
# While we're reading data...   && there is still data in $HOSTS_FILE
while mapfile -t -n $BATCH_SIZE nodes && (( ${#nodes[@]} )); do
    for node in ${nodes[@]}; do
        echo "node $(( node_counter++ )): $node"
```
```
sleep $SLEEP_SECS_PER_NODE
done
(( batch_counter++ ))
# Don't get stuck here AFTER the last (partial) batch...
[ "$node_counter" -lt "$node_count" ] && {
    # You can also use `mapfile -C Call_Back -c $BATCH_SIZE` for feedback but
    # it runs the callback up front too, so if you have a delay you'll
    # have to wait for that
    echo "Completed $node_counter of $node_count nodes;" \
        "batch $batch_counter of $batch_count;" \
        "sleeping for $SLEEP_SECS_PER_BATCH seconds..." 1>&2
    sleep $SLEEP_SECS_PER_BATCH
}
done < $HOSTS_FILE
```

- 1. 为此示例创建一个简单的测试文件。
- 2. 这不是“无用的 cat 使用”。通常 wc -l 输出为 <行数> <文件名>，但我们只想要 <行数>，而没有该选项。当从 STDIN 读取时，wc 不会显示文件名，这正是我们想要的。
- 3. 请记住，bash 仅做整数运算，因此批次数可能会有截断错误。
- 4. 仅整数运算；参见之前的说明。
- 5. 你注意到我们在 for 循环中懒惰地没有对 ${nodes[@]} 加引号了吗？我们可以用它，因为我们读取的是主机名，它们不允许空格。但最好养成一致的习惯并使用引号。

输出如示例 9-4 所示。

### Example 9-4. Fancy mapfile example: output

```
Nodes to process:      10
Batch size and count:  4 / 2
Sleep seconds per node: 1
Sleep seconds per batch: 1

node 0: node0
node 1: node1
node 2: node2
node 3: node3
Completed 4 of 10 nodes; batch 1 of 2; sleeping for 1 seconds...
node 4: node4
node 5: node5
```
```
node 6: node6
node 7: node7
Completed 8 of 10 nodes; batch 2 of 2; sleeping for 1 seconds...
node 8: node8
node 9: node9
```

另见：

- - help mapfile.
- - 或 help readarray，但它只会指向 mapfile。
- - 关于使用 mapfile 清理 AWS S3 存储桶的一个不错用例，请参阅 https://oreil.ly/xie4t。

### Brute Force

示例 9-5 显示了整个文件如何被“slurp”到内存中。

Example 9-5. Brute force file reading examples

```
for word in $(cat file); do
  echo "word: $word"
done
```

```
### Or, remove the "useless use of cat"
for word in $(< file); do
  echo "word: $word"
done
```

### Fiddling with $IFS for Fun and Profit, to Read Files

$IFS 是“内部字段分隔符”，用于所有单词分割。默认是 <空格><制表符><换行符>，或使用 $' ' ANSI C 引号机制的 IFS='$' \t\n'，这对 bash 习语至关重要。如果你更改 $IFS 而不真正理解自己在做什么，可能会出现奇怪的事情。特别是 $IFS 的第一个字符，默认是空格，也用于单词扩展，如果你更改它，可能会导致混乱。如果你真的认为需要更改 $IFS 的值，要么在函数中使用 local，要么将其本地化到命令（参见第 110 页的“Local Variables”）。示例 9-6 说明了这一点。

Example 9-6. Changing IFS for read example: code

```
#!/usr/bin/env bash
# fiddle-ifs.sh: Fiddling with $IFS for fun and profit, to read files
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch09/fiddle-ifs.sh
#_______________________________________________________________
```




# 创建测试文件（不拼写出“word”以保持输出 < 80 列）
IFS_TEST_FILE='/tmp/ifs-test.txt'
cat <<'EoF' > $IFS_TEST_FILE
line1 wd1 wd2 wd3
line2 wd1 wd2 wd3
line3 wd1 wd2 wd3
EoF

#-------------------------------------------------------------------------------
echo 'Normal $IFS and `read` operation; split into words:'
printf 'IFS before: %q\n' "$IFS"
while read line w1 w2 w3; do
    printf 'IFS during: %q\nline = %q, w1 = %q, w2 = %q, w3 = %q\n' \
        "$IFS" "$line" "$w1" "$w2" "$w3"
done < $IFS_TEST_FILE
printf 'IFS after: %q\n' "$IFS"

#-------------------------------------------------------------------------------
echo ''
echo 'Temporary $IFS change for `read` inline:'
echo 'Words are NOT split, yet $IFS appears unchanged, because only the read'
echo 'line has the changed $IFS. We also shortened "line" to "ln" to make'
echo 'it fit a book page.'
printf 'IFS before: %q\n' "$IFS"
while IFS='' read line w1 w2 w3; do
    printf 'IFS during: %q\nln = %q, w1 = %q, w2 = %q, w3 = %q\n' \
        "$IFS" "$line" "$w1" "$w2" "$w3"
done < $IFS_TEST_FILE
printf 'IFS after: %q\n' "$IFS"

#-------------------------------------------------------------------------------
function Read_A_File {
    local file="$1"
    local IFS=''

    while read line w1 w2 w3; do
    printf 'IFS during: %q\nline = %q, w1 = %q, w2 = %q, w3 = %q\n' \
        "$IFS" "$line" "$w1" "$w2" "$w3"
    done < $IFS_TEST_FILE
}

echo ''
echo 'Temporary $IFS change for `read` in a function; NOT split, $IFS changed:'
printf 'IFS before: %q\n' "$IFS"
Read_A_File
printf 'IFS after: %q\n' "$IFS"

#-----------------------------
echo ''
echo 'But you may not need to change $IFS at all... See `help read` and'
echo 'note the parts about:'
echo '    ...leftover words assigned to the last NAME'
echo '    ...[read line until] DELIM is read, rather than newline'
echo 'Normal $IFS and `read` operation using only 1 variable:'
printf 'IFS before: %q\n' "$IFS"
while read line; do
    printf 'IFS during: %q\tline = %q\n' "$IFS" "$line"
done < $IFS_TEST_FILE
printf 'IFS after: %q\n' "$IFS"

The output is shown in Example 9-7.

# Example 9-7. Changing IFS for read example: output

```
Normal $IFS and `read` operation; split into words:
$IFS before: $' \t\n'
IFS during: $' \t\n'      line = line1, w1 = wd1, w2 = wd2, w3 = wd3
IFS during: $' \t\n'      line = line2, w1 = wd1, w2 = wd2, w3 = wd3
IFS during: $' \t\n'      line = line3, w1 = wd1, w2 = wd2, w3 = wd3
IFS after:  $' \t\n'
```

Temporary $IFS change for `read` inline:
Words are NOT split, yet $IFS appears unchanged, because only the read line has the changed $IFS. We also shortened "line" to "ln" to make it fit a book page.

```
IFS before: $' \t\n'
IFS during: $' \t\n'      ln = line1\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS during: $' \t\n'      ln = line2\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS during: $' \t\n'      ln = line3\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS after:  $' \t\n'
```

Temporary $IFS change for `read` in a function; NOT split, $IFS changed:

```
IFS before: $' \t\n'
IFS during: '' line = line1\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS during: '' line = line2\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS during: '' line = line3\ wd1\ wd2\ wd3, w1 = '', w2 = '', w3 = ''
IFS after:  $' \t\n'
```

But you may not need to change $IFS at all... See `help read` and note the parts about:
    ...leftover words assigned to the last NAME
    ...[read line until] DELIM is read, rather than newline
Normal $IFS and `read` operation using only 1 variable:

```
IFS before: $' \t\n'
IFS during: $' \t\n'      line = line1\ wd1\ wd2\ wd3
IFS during: $' \t\n'      line = line2\ wd1\ wd2\ wd3
IFS during: $' \t\n'      line = line3\ wd1\ wd2\ wd3
IFS after:  $' \t\n'
```

See also:

+   - “Local Variables” on page 110
+   - “Reading Files” on page 85
+   - “ANSI-C Quoting” on the bash man page
