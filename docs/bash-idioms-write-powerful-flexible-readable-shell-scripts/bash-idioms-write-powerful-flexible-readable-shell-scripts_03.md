## 变量俗语

看到包含惯用语 `${0##*/}` 的错误信息或赋值语句并不罕见，它看起来像是某种对 `$0` 的引用，但实际上还有更多内容在发生。让我们更仔细地看看变量引用以及这些额外字符为我们做了什么。我们会发现一整套字符串操作，只用几个特殊字符就能提供相当大的能力。

### 变量引用

在大多数编程语言中，引用变量的值非常直接。你要么直接使用变量名，要么在名称前加上一个字符以明确表示你想要检索值。在 bash 中也是如此：你通过名称赋值给变量，`VAR=something`，并通过美元符号前缀检索值：`$VAR`。如果你想知道为什么需要美元符号，请考虑 bash 大多处理字符串，所以：

```
MSG="Error: FILE not found"
```

会给你一个由所示四个词组成的简单字面字符串，而：

```
MSG="Error: $FILE not found"
```

会将 `$FILE` 替换为该变量的值（该变量 presumably 保存着它正在查找的文件名）。

![](img/553ffa1bc15093729b691f706befc0ac_46_0.png)

### 变量插值

请确保使用双引号以使这种字符串替换发生。使用单引号会将所有字符视为字面值，不会发生任何替换。

为了避免变量名称何时结束的混淆（在这个例子中空格使其变得容易），变量引用的更完整语法使用大括号括住变量名 `${FILE}`，并且可以在我们的示例中使用。

这种带大括号的语法是变量引用周围许多特殊语法的基础。例如，我们可以在变量名前加上井号 `${#VAR}`，以返回的不是其值而是值的字符串长度。

| ${VAR} | ${#VAR} |
|---|---|
| oneword | 7 |
| /usr/bin/longpath.txt | 21 |
| many words in one string | 24 |
| 3 | 1 |
| 2356 | 4 |
| 1427685 | 7 |

但 bash 可以做的不止于简单地检索值或其长度。

### 参数扩展

在检索变量值时，可以指定某些替换或编辑，影响返回的值（除一种情况外，不会影响变量中的值）。语法涉及在大括号中界定变量名称的特殊字符序列，如这些大括号内的字符：`${VAR##*/}`。以下是一些值得了解的扩展。

### basename 的简写

当你调用脚本时，你可能只使用其文件名作为调用脚本的命令，但这假设脚本具有执行权限并且位于你的 PATH 变量中目录之一的目录中。你可能使用 `./scriptname` 调用脚本，如果脚本在当前目录中。你可能使用完整路径名 `/home/smith/utilities/scriptname` 调用它，甚至如果当前工作目录在附近，可以使用相对路径名。

无论你以何种方式调用脚本，`$0` 都将包含你用来调用脚本的字符序列——相对路径或绝对路径，以你表达的方式为准。

当你想在用法消息中打印该脚本的名称时，你可能只想要 basename，即文件本身的名称，而不是到达那里的任何路径：

```
echo "usage: ${0##*/} namesfile datafile"
```

你可能会在用法消息中看到它，告诉用户运行脚本的正确语法，或者它可能是赋值右侧的变量。在后一种情况下，我们希望变量被称为 `PROGRAM` 或 `SCRIPT` 之类的名称，因为这就是该表达式返回的内容——正在执行的脚本的名称。

让我们更仔细地看看 `$0` 上的这个特定参数扩展，你可以用它来获取仅限 basename 而不带路径的其他部分。

### 路径或前缀移除

你可以从值的前面（前缀或左侧）或尾部（后缀或右侧）移除字符。要从字符串的左侧移除一组字符，你在参数引用上加上 # 和 shell 模式，该模式匹配你想要移除的字符。

表达式 `${MYVAL#img_}` 会在 `MYVAL` 变量的字符串以 `img_` 开头时移除字符 `img_`。使用更复杂的模式，我们可以写 `${MYVAL#*_}`。这将移除到下划线为止的任何字符序列，包括下划线本身。（如果没有匹配的模式，则其完整值将保持不变返回。）

单个 # 表示它将使用可能的最短匹配（非贪婪）。双重 ## 表示使用可能的最长匹配（贪婪）。

现在，也许你可以看出表达式 `${0#*/}` 会做什么？

它将从 `$0` 中的值开始，即用于调用脚本的路径名。然后，从值的左侧，它将移除任何数量的字符以斜杠结尾的最长匹配。因此，它正在移除调用脚本时使用的路径的所有部分，只留下脚本本身的名称。

以下是 `$0` 的一些可能值以及我们讨论的这个模式，以了解短 (#) 和长 (##) 匹配在结果上可能有何不同：

| Value in $0 | Expression | Result returned |
|---|---|---|
| ./ascript | ${0#*/} | ascript |
| ./ascript | ${0##*/} | ascript |
| ../bin/ascript | ${0#*/} | bin/ascript |
| ../bin/ascript | ${0##*/} | ascript |
| /home/guy/bin/ascript | ${0#*/} | home/guy/bin/ascript |
| /home/guy/bin/ascript | ${0##*/} | ascript |

请注意，`*/` 的最短匹配模式可能只匹配斜杠本身。

![](img/553ffa1bc15093729b691f706befc0ac_49_0.png)

### Shell 模式，而非正则表达式

参数扩展中使用的模式不是正则表达式。它们只是 shell 模式匹配，其中 * 匹配 0 个或多个字符，? 匹配单个字符，[chars] 匹配大括号内字符中的任意一个。

### dirname 或后缀移除的简写

与 # 会移除前缀，即从左侧移除类似，我们可以通过使用 % 从右侧移除后缀，即从右侧移除。双百分号表示移除可能的最长匹配。以下是一些显示如何移除后缀的示例。第一个示例显示变量 `$FN`，它保存图像文件的名称。它可能以 .jpg 或 .jpeg 或 .png 或 .gif 结尾。看看不同的模式如何移除字符串右侧的各个部分。最后几个示例显示如何从 `$0` 参数获取类似于 dirname 的内容：

| Value in shell variable | Expression | Result returned |
|---|---|---|
| img.1231.jpg | ${FN%.*} | img.1234 |
| img.1231.jpg | ${FN%%.*} | img |
| ./ascript | ${0%/*} | . |
| ./ascript | ${0%%/*} | . |
| /home/guy/bin/ascript | ${0%/*} | /home/guy/bin |
| /home/guy/bin/ascript | ${0%%/*} |  |

这种用于 dirname 的参数替换并不是命令输出的精确复制。它在路径为 /file 的情况下有所不同，因为 dirname 会返回仅一个斜杠，而我们的参数替换会将其全部移除。你可以在脚本中用一些额外的逻辑检查这种情况，如果你不期望看到这种情况可以忽略它，或者你可以简单地在参数末尾加上一个斜杠，如 `${0%/*}/`，这样所有结果都会以斜杠结尾。

![](img/553ffa1bc15093729b691f706befc0ac_49_1.png)

### 前缀和后缀移除

你可以记住 # 移除左侧部分而 % 移除右侧部分，因为至少在标准美国键盘上，# 是 shift-3，位于 % 的 shift-5 的左侧。

### 其他修饰符

除了 # 和 % 之外，还有一些其他修饰符可以改变参数扩展的值。你可以通过 ^ 或 ^^ 分别将字符串中的第一个字符或所有字符转换为大写，或通过 , 或 ,, 转换为小写，如这些示例所示：

| Value in shell variable TXT | Expression | Result returned |
|---|---|---|
| message to send | ${TXT^} | Message to send |
| message to send | ${TXT^^} | MESSAGE TO SEND |
| Some Words | ${TXT,} | some Words |
| Do Not YELL | ${TXT,,} | do not yell |

你也可以考虑 `declare -u UPPER` 和 `declare -l lower`，它们声明这些 shell 变量，使其内容在分配给这些变量的任何文本时转换为大写或小写。

最灵活的修饰符是在字符串的任何位置进行替换，而不仅仅是在字符串的前端或尾部。类似于 sed 命令，它使用斜杠 / 来指示要匹配的模式和要替换它的值。单个斜杠表示单个替换（第一个出现）。使用两个斜杠表示替换所有出现。以下是一些示例：

| Value in shell variable FN | Expression | Result returned |
|---|---|---|
| FN="my filename with spaces.txt" | ${FN/ /_} | my_filename with spaces.txt |
| FN="my filename with spaces.txt" | ${FN// /_} | my_filename_with_spaces.txt |
| FN="my filename with spaces.txt" | ${FN// /} | myfilenamewithspaces.txt |
| FN="/usr/bin/filename" | ${FN//\/ } | usr bin filename |
| FN="/usr/bin/filename" | ${FN//\/ } | usr/bin/filename |

![](img/553ffa1bc15093729b691f706befc0ac_50_0.png)

### 无尾部斜杠

请注意，与 sed 或 vi 等其他类似命令中会找到的尾部斜杠不同。闭合大括号结束替换。

为什么不总是使用这种替换机制？为什么还要费心使用 # 或 % 从字符串两端进行替换？考虑这个文件名：frank.gifford.gif，假设你想使用 Image Magick 的 convert 命令将此文件名更改为 jpg 文件（那是另一个故事）。使用 / 的替换没有办法将搜索锚定到字符串的一端或另一端。如果你读取了文件名并尝试将 .gif 替换为 .jpg，你最终会得到 frank.jpgford.gif。对于这种情况，从字符串末尾取值的 % 替换效果要好得多。

另一个有用的修饰符将提取变量的子字符串。在变量名之后，放置一个冒号，然后是子字符串的第一个字符的偏移量


要提取的内容。 由于这是一个偏移量，从字符串的第一个字符开始计数为 0。 接下来，放置另一个冒号和你想要的子字符串的长度。 如果省略这个第二个冒号和长度，则会得到从该点开始的字符串的剩余全部内容。 这里有几个例子：

| shell 变量 FN 中的值 | 表达式 | 返回结果 |
|---|---|---|
| /home/bin/util.sh | ${FN:0:1} | / |
| /home/bin/util.sh | ${FN:1:1} | h |
| /home/bin/util.sh | ${FN:3:2} | me |
| /home/bin/util.sh | ${FN:10:4} | util |
| /home/bin/util.sh | ${FN:10} | util.sh |

示例 4-1 展示了使用参数扩展从某些输入中解析数据，以创建并处理用于自动创建防火墙规则配置时使用的特定字段。 我们还在代码中加入了 bash 参数扩展的更大表格，就像本书中经常做的那样，作为“真实代码可读性”示例。 输出在示例 4-2 中。

## 示例 4-1. 使用参数扩展进行解析：代码

```
#!/usr/bin/env bash
# parameter-expansion.sh: parameter expansion for parsing, and a big list
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch04/parameter-expansion.sh
# ______________________________________________________________
# Does not work on Zsh 5.4.2!

customer_subnet_name='Acme Inc subnet 10.11.12.13/24'

echo ''
echo "Say we have this string: $customer_subnet_name"

customer_name=${customer_subnet_name%subnet*}    # Trim from 'subnet' to end
subnet=${customer_subnet_name##* }               # Remove leading 'space*'
ipa=${subnet%/*}                                 # Remove trailing '/*'
cidr=${subnet#*/}                                # Remove up to '/*'
fw_object_name=${customer_subnet_name// /_}      # Replace space with '_'
fw_object_name=${fw_object_name//-/}             # Replace '/' with '-'
fw_object_name=${fw_object_name,,}               # Lowercase

echo ''
echo 'When the code runs we get:'
echo ''
echo "Customer name: $customer_name"
echo "Subnet:        $subnet"
echo "IPA            $ipa"
echo "CIDR mask:     $cidr"
```

```
echo "FW Object:    $fw_object_name"
```

## bash Shell Parameter Expansion: https://oreil.ly/Af8lw

# ${var#pattern}          Remove shortest (nongreedy) leading pattern
# ${var##pattern}         Remove longest (greedy) leading pattern
# ${var%pattern}          Remove shortest (nongreedy) trailing pattern
# ${var%%pattern}         Remove longest (greedy) trailing pattern

# ${var/pattern/replacement}   Replace first +pattern+ with +replacement+
# ${var//pattern/replacement}   Replace all +pattern+ with +replacement+

# ${var^pattern}      Uppercase first matching optional pattern
# ${var^^pattern}      Uppercase all matching optional pattern
# ${var,pattern}       Lowercase first matching optional pattern
# ${var,,pattern}      Lowercase all matching optional pattern

# ${var:offset}          Substring starting at +offset+
# ${var:offset:length}    Substring starting at +offset+ for +length+

# ${var:-default}        Var if set, otherwise +default+
# ${var:=default}        Assign +default+ to +var+ if +var+ not already set
# ${var:?error_message}   Barf with +error_message+ if +var+ not set
# ${var:+replaced}        Expand to +replaced+ if +var+ _is_ set

# ${#var}                Length of var
# ${!var[*]}             Expand to indexes or keys
# ${!var[@]}              Expand to indexes or keys, quoted

# ${!prefix*}            Expand to variable names starting with +prefix+
# ${!prefix@}            Expand to variable names starting with +prefix+, quoted

# ${var@Q}               Quoted
# ${var@E}               Expanded (better than `eval`!)
# ${var@P}               Expanded as prompt
# ${var@A}               Assign or declare
# ${var@a}               Return attributes

## 示例 4-2. 使用参数扩展进行解析：输出

Say we have this string: Acme Inc subnet 10.11.12.13/24

When the code runs we get:

```
Customer name: Acme Inc
Subnet:        10.11.12.13/24
IPA            10.11.12.13
CIDR mask:     24
FW Object:     acme_inc_subnet_10.11.12.13-24
```

## 条件替换

其中一些变量替换是条件性的，即只有在满足某些条件时才会发生。 你可以使用围绕赋值的 if 语句来实现同样的效果，但这些习惯用法对于某些常见情况可以使代码更简短。 这些条件替换在这里以冒号后跟另一个特殊字符显示：减号、加号或等号。 它们检查的条件是：变量是否为空或未设置？ 空变量是其值为 null 字符串的变量。 未设置变量是指尚未分配或已明确使用 unset 命令取消设置（想想“丢弃”）的变量。 对于位置参数（如 $1、$2 等），如果用户未在该位置提供参数，则它们未设置。

如果你在这些条件替换中不包含冒号，则它们只考虑未设置变量的情况；空值将原样返回。

## 默认值

一个常见场景是脚本有一个可选的单个参数。 如果在调用脚本时未提供该参数，则应使用默认值。 在 bash 中，我们可能会这样写：

```
LEN=${1:-5}
```

这将设置变量 LEN 为第一个参数 ($1) 的值——如果提供了参数——否则为值 5。 这里有一个示例脚本：

```
LEN="${1:-5}"
cut -d',' -f2-3 /tmp/megaraid.out | sort | uniq -c | sort -rn | head -n "$LEN"
```

它从名为 /tmp/megaraid.out 的逗号分隔值文件中取出第二和第三个字段，对这些值进行排序，提供每个值对出现的次数计数，然后显示列表中的前 5 个。 你只需将该计数指定为脚本的唯一参数，即可覆盖默认值 5 并显示前 3 个或 10 个（或你想要的任意数量）。

## 逗号分隔列表

另一个条件替换，使用加号，也会检查变量是否有值，如果有，则是否会返回不同的值。 也就是说，只有当变量不为空时，才返回指定的不同值。 是的，这听起来很奇怪；如果它有值，为什么要返回不同的值？

这种看似奇怪的逻辑的一个方便用途是构造逗号分隔列表。 你通常通过对每个值重复追加 “，value” 或 “value，” 来构造这样的列表。 在这样做时，你通常需要一个 if 语句来避免在列表的前面或后面出现多余的逗号——但在使用此连接习惯用法时不需要：

```
for fn in * ; do
    S=${LIST:+,}        # S for separator
    LIST="${LIST}${S}${fn}"
done
```

另见示例 7-1。

## 修改后的值

到目前为止，这些替换都没有修改变量的底层值。 然而，有一个会修改。 如果我们写 ${VAR:=value}，它将与我们之前默认值的习惯用法非常相似，但有一个很大的例外。 如果 VAR 为空或未设置，它将把该值分配给变量（因此，等号），并返回该值。 （如果 VAR 已设置，它将简单地返回其现有值。）请注意，此赋值对于位置参数（如 $1）不起作用，这就是为什么你几乎看不到它被使用的原因。

## $RANDOM

Bash 有一个非常方便的 $RANDOM 变量。 正如 Bash 参考手册中“Bash Variables”部分所述：

> 每次引用此参数时，都会生成一个介于 0 和 32767 之间的随机整数。 给此变量赋值会为随机数生成器设置种子。

虽然这不适合加密功能，但它对于掷骰子或在否则过于可预测的操作中添加一点噪声很有用。 我们在第 69 页的“一个简单的词数示例”中稍后会使用它。

如示例 4-3 所示，你可以从列表中挑选一个随机元素。

示例 4-3. 挑选一个随机列表元素

```
declare -a mylist
mylist=(foo bar baz one two "three four")

range=${#mylist[@]}
random=$(($RANDOM % $range))  # 0 to list length count
echo "range = $range, random = $random, choice = ${mylist[$random]}"

# Shorter but less readable 6 months from now:
# echo "choice = ${mylist[$(($RANDOM % ${#mylist[@]}))]}"
```

你也可能会看到类似这样的内容：

```
TEMP_DIR="$TMP/myscript.$RANDOM"
[ -d "$TEMP_DIR" ] || mkdir "$TEMP_DIR"
```





然而，这会受到竞态条件的影响，而且显然是一个简单的模式。它在某种程度上是可预测的，但有时你想知道是什么代码在污染 $TMP。不要忘记设置一个陷阱（参见第 97 页的“It’s a Trap!”）来清理你自己的残留物。我们建议你考虑使用 mktemp，不过这是一个超出 bash 惯用法范围的大问题。

![](img/553ffa1bc15093729b691f706befc0ac_55_0.png)

## $RANDOM 和 dash

$RANDOM 在 dash 中不可用，而 dash 在某些 Linux 发行版中是 /bin/sh。值得注意的是，当前版本的 Debian 和 Ubuntu 使用 dash，因为它比 bash 更小更快，从而有助于更快启动。但这意味着 /bin/sh，这个曾经是 bash 的符号链接，现在改为指向 dash，各种 bash 特定功能将无法工作。它在 Zsh 中是可以工作的。

## 命令替换

我们在第 2 章中已经大量使用了命令替换，但我们还没有谈论过它。旧的 Bourne 方式是使用 ``（反引号/反引号），但我们更喜欢更易读的 POSIX $( )。你会看到这两种形式很多，因为这就是你将输出拉入变量的方式；例如：

```
unique_lines_in_file="$(sort -u "$my_file" | wc -l)"
```

请注意，这两者是相同的，但第二个是内部的并且更快：

```
for arg in $(cat /some/file)
for arg in $(</some/file)    # Faster than shelling out to cat
```

![](img/553ffa1bc15093729b691f706befc0ac_55_1.png)

## 命令替换

命令替换对于云和其他 DevOps 自动化至关重要，因为它允许你收集并使用仅在运行时才存在的各种 ID 和详细信息；例如：

```
instance_id=$(aws ec2 run-instances --image $base_ami_id ... \
    --output text --query 'Instances[*].InstanceId')
```

```
state=$(aws ec2 describe-instances --instance-ids $instance_id \
    --output text --query 'Reservations[*].Instances[*].State.Name')
```

![](img/553ffa1bc15093729b691f706befc0ac_56_0.png)

## 嵌套命令替换

使用 `` 嵌套命令替换会变得非常丑陋，很快，因为你必须在每一层嵌套中转义内部的反引号。如果可以的话，使用 $( ) 会容易得多，如下所示：

```
### Just Works
$ echo $(echo $(echo $(echo inside)))
inside
```

```
### Broken
$ echo `echo `echo `echo inside```
echo inside
```

```
### "Works" but very ugly
$ echo `echo \`echo \\\`echo inside\\\`\`
inside
```

感谢我们的审阅者 Ian Miell 指出这一点并提供示例。

#### 风格和可读性：回顾

在 bash 中引用变量时，你有机会在设置或检索值时编辑该值。变量引用末尾的几个特殊字符可以从字符串值的开头或结尾删除字符，改变其字符为大写或小写，替换字符，或仅给你原始值的子字符串。这些方便功能的常见使用导致了默认值的惯用法，basename 和 dirname 的替代，以及在不使用显式 if 语句的情况下创建逗号分隔列表。

变量替换是 bash 中的一个很棒的功能，我们建议充分利用它们。然而，我们也强烈建议你对这些语句进行注释，以明确你试图进行的替换类型。你的代码的下一个阅读者会感谢你的。
