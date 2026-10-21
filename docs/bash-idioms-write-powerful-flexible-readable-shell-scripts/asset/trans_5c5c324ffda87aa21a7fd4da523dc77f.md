

它可能在另一个命令中表示“快速”。在另一个命令中它可能根本不是一个有效选项。这些都是基于现有命令的约定——可以说是传统。

除非有令人信服的理由放弃它，否则遵守传统是值得的。原因之一是它降低了学习曲线。你在运行某些命令或脚本时学到的知识可以迁移到其他命令中。它还意味着你可以使用他人使用的相同技术来解析选项。

## 解析选项

使用 getopts 内置命令来解析 shell 脚本的选项。它会被重复调用（通常通过 while 循环），直到找到所有选项。它假定所有选项都出现在其他参数之前。它可以识别单独的选项 (-a -v) 以及组合在一起的选项 (-av)。你可以指定某个选项必须提供额外的参数。例如，你可能希望 -o 选项指定输出文件，因此用户会使用 -o filename 或 -ofilename 调用它，这两种格式都由 getopts 支持。

让我们看一下示例 8-1，它处理了这类选项。

### 示例 8-1. getopts 参数解析：简单

```
#!/usr/bin/env bash
# parseit.sh: Use getopts to parse these arguments
# Original Author & date: _bash Idioms_ 2022
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
# Original Author & date: _bash Idioms_ 2022
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

