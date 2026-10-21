## 列表和哈希处理

我们所知道的，计算机非常擅长计数和组织数据。我们可以使用代码来计数和组织数据，使用 *数据结构*，而这些的构建块是 *数组*。Bash 从一开始就有了数组，并在 4.0 版本中加入了 *关联数组*。你在 Bash 数组中会遇到难以阅读的代码，部分原因是 Bash 有很多历史和向后兼容性需要维护，正如我们之前提到的，但也因为一些开发者倾向于过度复杂化。数组实际上并不难实现，只要稍加思考，你就可以清晰地书写它们。

作为复习，在计算机科学和编程中，数组是包含多个元素并由整数索引或引用的变量。或者说，数组是一个包含列表而不是标量或单个值的变量。关联数组是一种由字符串而不是整数索引的列表类型。所以它是一个键值对列表，基本上形成了一个字典或查找表，其中内部将键哈希以形成内存位置。大致如此。

Bash 文档使用 *数组* 和 *关联数组* 术语，但根据你的背景，*列表* 和 *哈希*，或者可能是 *字典* 或 *dict*，可能更熟悉，而且它们肯定更容易输入和说。Bash 文档还使用 *下标*，而其他人可能会说 *索引*。我们通常为了保持一致而遵循 Bash 文档，但对于这些，我们将使用更常见、更易于理解的 *列表*、*哈希* 和 *下标* 术语。

即使列表（数组）出现得更早，哈希（关联数组）稍微简单一些，因为永远不会有关于提供索引（下标）的疑问，因为它是必需的。列表的整数索引可能是隐含的，虽然它并不复杂，但有一些操作适用于它们，而对哈希来说毫无意义。

正如 Bash 参考手册所说，“Bash 提供一维索引和关联数组变量。” 虽然可以创建非常丑陋的多维结构，但在 Bash 中这样做很可能会以眼泪告终。如果你真的需要这样做，请考虑在另一种语言中实现。

![](img/553ffa1bc15093729b691f706befc0ac_75_0.png)

## 使用哈希时注意 Bash 版本

你在这里真的需要小心你的 Bash 版本。正如我们刚才所说，Bash 直到 4.0 版本才获得哈希（即关联数组），并且花了几个版本才理清一些细节，比如允许 $list[-1] 引用最后一个整数元素（v4.3），而不是需要 $mylist[${#mylist[*]}-1]（其中 ${#mylist[*]} 是元素计数）。太糟糕了。

正如我们在第 ix 页“Mac 上的 bash”中所说，要注意 Mac 上的库存 bash；它相当老旧。你可以在 MacPorts、Homebrew 或 Fink 中找到更新的版本。

![](img/553ffa1bc15093729b691f706befc0ac_75_1.png)

## 非 POSIX

此外，数组（列表和哈希）未被 POSIX 指定，因此如果可移植性超出 Bash 的范围是一个问题，你在使用它们时需要非常小心，因为它们可能无法工作。例如，Zsh 的语法有点不同，所以这些示例在 Mac 上在该 shell 上也无法工作。

## 共同点

在 Bash 中，列表和哈希非常相似，所以我们将从它们共同的部分开始，然后再谈到它们不同的地方。事实上，你可以将列表视为哈希的一个子集，只是恰好具有有序整数索引。我们不认为你应该这样做，但你可以。

列表本质上是有序的，而哈希本质上是无序的，因此像 shift 或 push 这样的操作只对有序列表有意义。另一方面，你永远不需要对有序列表的键进行排序，但对哈希可能需要。

![](img/553ffa1bc15093729b691f706befc0ac_75_2.png)

## 意外赋值

没有下标的意外赋值会影响零号元素，因此 myarray=foo 会导致创建或覆盖 $myarray[0] 为 foo，即使它是一个哈希！

来自 Bash 文档：

If the subscript is @ or *, the word expands to all members of the array name. These subscripts differ only when the word appears within double quotes. If the word is double-quoted, "${name[*]}" expands to a single word with the value of each array member separated by the first character of the IFS variable (see “Fiddling with $IFS for Fun and Profit, to Read Files” on page 89), whereas "${name[@]}" expands each element of name to a separate word.

这不是一句话吗？我们已经在第 14 页的“引号和空格”中谈到过这个问题，但它很重要，因为，正如我们将展示的，如果你做错了，你可能会伤害自己。我们已在示例 7-1 中使用 printf "%q" 和管道（|）分隔符来显示“shell 引用”的字符串（技术上是“单词”），以便在屏幕输出或本页上可见。引用规则实际上与我们在第 2 章中介绍的相同，只是放在列表或哈希的上下文中。

## 列表

正如我们所说，数组，也称为列表，是包含多个元素并由整数索引或引用的变量。

在 Bash 中，它们从零开始，可以使用 declare -a、local -a、readonly -a 声明，或通过直接分配给新变量如 mylist[0]=foo 或 mylist=()（空列表）来声明。一旦变量被声明为列表，像 mylist+=(bar) 这样的简单赋值就等同于 push，它将项目添加到列表末尾，但请注意 + 和 ()，两者都很关键。表 7-1 给出了一个示例。

表 7-1. 示例 bash 列表

| 元素 | 值 |
|---|---|
| mylist[0] | foo |
| mylist[1] | bar |
| mylist[2] | baz |

数组或列表的常见操作是：

- 声明变量为列表
- 为其分配一个或多个值
- 如果将其视为栈（想想自助餐厅的盘子；FIFO：先进先出，或先入先出）：
  - push
  - pop
- 显示（转储）所有值以供调试或重用
- 引用一个或所有值（for，或 for each）
- 引用值的一个子集（切片）
- 删除一个或多个值
- 删除整个列表

与其谈论所有这些，我们将向你展示，你可以根据需要挑选所需的习惯用法（示例 7-1）。

## 示例 7-1. bash 列表示例：代码

```
#!/usr/bin/env bash
# lists.sh: bash list example code
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch07/lists.sh
# ______________________________________________________________________________
# Does not work on Zsh 5.4.2!

# Books are not as wide as some screens!
FORMAT='fmt --width 70 --split-only'

# Declare a list
# declare -a mylist   # Can do this, or `local -a` or `readonly -a` or:
mylist[0]='foo'      # This both declares and assigns to mylist[0]

# OR Both declares & assigns:
#mylist=(foo bar baz three four "five by five" six)

# Push or assign, note the += and ()
###mylist=(bar)          # Would overwrite mylist[0]
mylist+=(bar)          # mylist[1]
mylist+=(baz)          # mylist[2]
mylist+=(three four)   # mylist[3] AND mylist[4]
mylist+=("five by five")  # mylist[5] Note spaces and quotes
mylist+=("six")       # mylist[6]

# OR APPEND, note the "+" and we're assuming foo was already assigned
#mylist+=(bar baz three four "five by five" six)

# Display or dump the values
echo -e "\nThe element count is: ${#mylist[@]}"
echo -e "\nThe length of element [4] is: ${#mylist[4]}"

echo -e "\nDump or list:"
declare -p mylist | $FORMAT
echo -n "  ${mylist[@]}  = "; printf "%q|"  ${mylist[@]}
echo -en "\n${mylist[*]}  = "; printf "%q|"  ${mylist[*]}
echo -en "\n\n${mylist[@]}\" = "; printf "%q|" "${mylist[@]}"
echo -en "\n\"${mylist[*]}\" = "; printf "%q|" "${mylist[*]}"
```

```
echo -e "  # Broken!"  # Previous line is bad and no newline
# See `help printf` or chapter 6 "printf for reuse or debugging", we need
# this to show the correct words:
# %q quote the argument in a way that can be reused as shell input

# "Join" the values
function Join { local IFS="$1"; shift; echo "$*"; } # One character delimiter!
# Note that the Join above requires "$*" and not "${0}"!
echo -en "\nJoin ',' ${mylist[@]} = "; Join ',' "${mylist[@]}"
function String_Join {
    local delimiter="$1"
    local first_element="$2"
    shift 2
    printf '%s ' "$first_element" "${@/#/$delimiter}"
    # Print first element, then reuse the '%s' format to display the rest of
    # the elements (from the function args $@), but add a prefix of $delimiter
    # by "replacing" the leading empty pattern (/#) with $delimiter.
}
echo -n "String_Join '<>' ${mylist[@]} = " ; String_Join '<>' "${mylist[@]}"

# Iterate over the values
echo -e "\nforeach \"\${!mylist[@]}\":"
for element in "${!mylist[@]}"; do
    echo -e "\tElement: $element; value: ${mylist[$element]}"
done

echo -e "\nBut don't do this: ${mylist[*]}"
for element in ${mylist[*]}; do
    echo -e "\tElement: $element; value: ${mylist[$element]}"
done

# Handle slices (subsets) of the list, shift and pop
echo -e "\nStart from element 5 and show a slice of 2 elements:"
printf "%q " "${mylist[@]:5:2}"
echo ''  # No newline in above

echo -e "\nShift FIRST element [0] (dumped before and after):"
declare -p mylist | $FORMAT      # Display before
mylist=("${mylist[@]:1}")        # First element, needs quotes
#mylist=("${mylist[@]:$count}")  # First #count elements
declare -p mylist | $FORMAT      # Display after

echo -e "\nPop LAST element (dumped before and after):"
declare -p mylist | $FORMAT
unset -v 'mylist[-1]'            # bash v4.3+
#unset -v "mylist[${#mylist[*]}-1]"  # Older
declare -p mylist

# Delete slices
echo -e "\nDelete element 2 using unset (dumped before and after):"
declare -p mylist
unset -v 'mylist[2]'
```





```
declare -p mylist

# Delete the entire list
unset -v mylist
```

- 1. 声明一个变量为数组（我们在这里说数组而不是列表，因为标志是 -a）。
- 2. 为其分配一个或多个值。
- 3. 显示（转储）所有值用于调试或重用；参见示例 4-1。
- 4. 两种不同的连接函数；另请参阅第 38 页的“逗号分隔列表”。
- 5. 遍历值；参见示例 4-1。
- 6. 处理列表的切片（子集）、移位和弹出。
- 7. 删除切片。
- 8. 删除整个列表。注意 unset，因为有一个微妙的陷阱。如果你恰好有一个与变量同名的文件，globbing¹ 会伤害你，并且你可能会破坏意外的东西。为避免这种情况，最好引用你的变量。使用 -v 更安全，以强制 unset 将你的参数视为 shell 变量，如 unset -v 'list'。

它看起来像运行 Example 7-2 时的样子。

## Example 7-2. bash list example: output

```
The element count is: 7 or 7

The length of element [4] is: 4

Dump or list:
declare -a mylist=([0]="foo" [1]="bar" [2]="baz" [3]="three" [4]="four" [5]="five by five" [6]="six")
${mylist[0]} = foo|bar|baz|three|four|five|by|five|six|
${mylist[*]} = foo|bar|baz|three|four|five|by|five|six|
"${mylist[@]}" = foo|bar|baz|three|four|five\ by\ five|six|
"${mylist[*]}" = foo\ bar\ baz\ three\ four\ five\ by\ five\ six|  # Broken!

Join ',' ${mylist[@]} = foo,bar,baz,three,four,five by five,six
```

> 1 Interpolating and expanding wildcard patterns; see https://oreil.ly/0gNay.
> 2 Accidentally overwriting; see https://oreil.ly/xEZhz.

```
String_Join '<>' ${mylist[@]} = foo<>bar<>baz<>three<>four<>five by five<>six
foreach "${mylist[@]}":
    Element: 0; value: foo
    Element: 1; value: bar
    Element: 2; value: baz
    Element: 3; value: three
    Element: 4; value: four
    Element: 5; value: five by five
    Element: 6; value: six
```

```
But don't do this: ${mylist[*]}
    Element: foo; value: foo
    Element: bar; value: foo
    Element: baz; value: foo
    Element: three; value: foo
    Element: four; value: foo
    Element: five; value: foo
    Element: by; value: foo
    Element: five; value: foo
    Element: six; value: foo
```

```
Start from element 5 and show a slice of 2 elements:
five\ by\ five|six|
```

```
Shift FIRST element [0] (dumped before and after):
declare -a mylist=([0]="foo" [1]="bar" [2]="baz" [3]="three" [4]="four" [5]="five by five" [6]="six")
declare -a mylist=([0]="bar" [1]="baz" [2]="three" [3]="four" [4]="five by five" [5]="six")
```

```
Pop LAST element (dumped before and after):
declare -a mylist=([0]="bar" [1]="baz" [2]="three" [3]="four" [4]="five by five" [5]="six")
declare -a mylist=([0]="bar" [1]="baz" [2]="three" [3]="four" [4]="five by five")
```

```
Delete element 2 using unset (dumped before and after):
declare -a mylist=([0]="bar" [1]="baz" [2]="three" [3]="four" [4]="five by five")
declare -a mylist=([0]="bar" [1]="baz" [3]="four" [4]="five by five")
```

## Hashes

也称为哈希、字典或 dict，关联数组是索引为任意字符串而不是整数的列表。它们在计数或“唯一化”（即忽略或删除重复）字符串等方面非常方便。

与列表不同，这些必须使用 declare -A、local -A 或 readonly -A 声明，并且下标始终是必需的；参见表 7-2 中的示例。

## Table 7-2. Sample bash hash

| Element | Value |
|---|---|
| myhash[oof] | foo |
| myhash[rab] | bar |
| myhash[zab] | baz |

关联数组或哈希或字典的常见操作是：

- 声明一个变量为关联数组（我们在这里说关联数组而不是“哈希”，因为标志是 -A）
- 为其分配一个或多个值
- 显示（转储）所有值用于调试或重用
- 引用一个或所有值（for，或 for each）
- 引用特定值（查找）
- 删除一个或多个值
- 删除整个哈希

再次，我们不打算讨论所有这些，我们只是在示例 7-3 中向你展示，所以挑选出你需要的习惯用法。

## Example 7-3. bash hash example: code

```
#!/usr/bin/env bash
# hashes.sh: bash Hash example code
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch07/hashes.sh
##### ----------------------------------------------------------------------------
# Does not work on Zsh 5.4.2!

# Books are not as wide as some screens!
FORMAT='fmt --width 70 --split-only'

# Declare a hash
declare -A myhash  # MUST do this, or `local -A` or `readonly -A`

# Assign to it, note no "+"
##myhash=(bar)        # Error: needs subscript when assigning associative array
myhash[a]='foo'      # Insertion 1, not 0...sort of
myhash[b]='bar'      # Insertion 2
myhash[c]='baz'      # Insertion 3
myhash[d]='three'    # 4 Different than our list example
myhash[e]='four'     # Insertion 5, note, not 4
myhash[f]='five by five'  # 6 Note spaces
myhash[g]='six'      # Insertion 7
```

```
# OR
#myhash=([a]=foo [b]=bar [c]=baz [d]="three" [e]="four" [f]="five by five" [g]="six")

# Display or dump the details and values
echo -e "\nThe key count is: ${#myhash[@]} or ${#myhash[*]}"

echo -e "\nThe length of the value of key [e] is: ${#myhash[e]}"

echo -e "\nDump or list:"
declare -p myhash | $FORMAT
echo -n "${myhash[@]}" = ; printf "%q" ${myhash[@]}
echo -en "\n${myhash[*]}" = ; printf "%q" ${myhash[*]}
echo -en "\n\[${myhash[@]}\] =" ; printf "%q" "${myhash[@]}"
echo -en "\n\[${myhash[*]}\] =" ; printf "%q" "${myhash[*]}"
echo -e ""  # Broken!  # Previous line is bad and no newline
# See 'help printf' or chapter 6 "printf for reuse or debugging", we need
# this to show the correct words:
# %q    quote the argument in a way that can be reused as shell input

# "Join" the values
function Join { local IFS="$1"; shift; echo "$*"; } # One character delimiter!
# Note the Join above requires "$*" and not "$@"!
echo -en "\nJoin ',' ${myhash[@]} = " ; Join ',' "${myhash[@]}"
function String_Join {
    local delimiter="$1"
    local first_element="$2"
    shift 2
    printf '%s' "$first_element" "${@/#/$delimiter}"
    # Print first element, then reuse the '%s' format to display the rest of
    # the elements (from the function args $@), but add a prefix of $delimiter
    # by "replacing" the leading empty pattern (/#) with $delimiter.
}
echo -n "String_Join '<>' ${myhash[@]} = " ; String_Join '<>' "${myhash[@]}"

# Iterate over the keys and values
echo -e "\nforeach \"${!myhash[@]}\":"
for key in "${!myhash[@]}"; do
    echo -e "\tkey: $key; value: ${myhash[$key]}"
done

echo -e "\nBut don't do this: ${myhash[*]}"
for key in ${myhash[*]}; do
    echo -e "\tkey: $key; value: ${myhash[$key]}"
done

# Handle slices (subsets) of the hash
echo -e "\nStart from hash insertion element 5 and show a slice of 2 elements:"
printf "%q" "${myhash[@]:5:2}"
echo ''  # No newline in above
echo -e "\nStart from hash insertion element 0 (huh?) and show a slice of 3 elements:"
printf "%q" "${myhash[@]:0:3}"
```

```
echo ' ' # No newline in above
echo -e "\nStart from hash insertion element 1 and show a slice of 3 elements:"
printf "%q\n" "${myhash[@]:1:3}"
echo ' ' # No newline in above
```

```
#echo -e "\nShift FIRST key [@]:"   = makes no sense in a hash!
#echo -e "\nPop LAST key:"          = makes no sense in a hash!
```

```
# Delete keys
echo -e "\nDelete key c using unset (dumped before and after):"
declare -p myhash | $FORMAT
unset -v 'myhash[c]'
declare -p myhash | $FORMAT
```

```
# Delete the entire hash
unset -v myhash
```

- 1. 声明一个哈希（必需）。
- 2. 为哈希赋值。
- 3. 显示或转储详细信息和值；参见示例 4-1。
- 4. 两种不同的连接函数；另请参阅第 38 页的“逗号分隔列表”。
- 5. 遍历键和值；另请参阅第 38 页的“逗号分隔列表”。
- 6. 处理哈希的切片（子集），这有点奇怪，因为下标不是有序的整数列表。
- 7. 删除键。
- 8. 删除整个哈希。注意 unset，因为有一个微妙的陷阱。如果你恰好有一个与变量同名的文件，globbing 会伤害你，并且你可能会破坏意外的东西。为避免这种情况，最好引用你的变量。使用 -v 更安全，以强制 unset 将你的参数视为 shell 变量，如 unset -v 'list'。

运行它看起来像示例 7-4。

## Example 7-4. bash hash example: output

The key count is: 7 or 7

The length of the value of key [e] is: 4

Dump or list:

```
declare -A myhash=([a]="foo" [b]="bar" [c]="baz" [d]="three"
```


[e]="four" [f]="five by five" [g]="six" )
${myhash[@]}	= foo|bar|baz|three|four|five|by|five|six|
${myhash[*]}	= foo|bar|baz|three|four|five|by|five|six|
"${myhash[@]}" = foo|bar|baz|three|four|five\ by\ five|six|
"${myhash[*]}" = foo\ bar\ baz\ three\ four\ five\ by\ five\ six|  # Broken!

Join ',' ${myhash[@]} = foo,bar,baz,three,four,five by five,six
String_Join '<>' ${myhash[@]} = foo<>bar<>baz<>three<>four<>five by five<>six
foreach "${!myhash[@]}":
    Key: a; value: foo
    Key: b; value: bar
    Key: c; value: baz
    Key: d; value: three
    Key: e; value: four
    Key: f; value: five by five
    Key: g; value: six

But don't do this: ${myhash[*]}
    Key: foo; value:
    Key: bar; value:
    Key: baz; value:
    Key: three; value:
    Key: four; value:
    Key: five; value:
    Key: by; value:
    Key: five; value:
    Key: six; value:

Start from hash insertion element 5 and show a slice of 2 elements:
four|five\ by\ five|

Start from hash insertion element 0 (huh?) and show a slice of 3 elements:
foo|bar|baz|

Start from hash insertion element 1 and show a slice of 3 elements:
foo|bar|baz|

Delete key c using unset (dumped before and after):
declare -A myhash=([a]="foo" [b]="bar" [c]="baz" [d]="three" [e]="four" [f]="five by five" [g]="six" )
declare -A myhash=([a]="foo" [b]="bar" [d]="three" [e]="four" [f]="five by five" [g]="six" )

## A Simple Word Count Example

As we said, one of the most common uses of a hash is to count and/or “uniq” items, so Example 7-5 is a simple but concrete example.

### Example 7-5. bash word count example: code

```
#!/usr/bin/env bash
# word-count-example.sh: More examples for bash lists and hashes, and $RANDOM
##### Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch07/word-count-example.sh
##### ----------------------------------------------------------------------------
# Does not work on Zsh 5.4.2!
# See also: `man uniq`

WORD_FILE='/tmp/words.txt'
> $WORD_FILE
trap "rm -f $WORD_FILE" ABRT EXIT HUP INT QUIT TERM

declare -A myhash

echo "Creating & reading random word list in: $WORD_FILE"

# Create a list of words to use for the hash example
mylist=(foo bar baz one two three four)

# Loop, and randomly pick elements out of the list
range="${#mylist[@]}"
for ((i=0; i<35; i++)); do
    random_element="$(( $RANDOM % $range ))"
    echo "${mylist[$random_element]}" >> $WORD_FILE
done

# Read the word list into a hash
while read line; do
    (( myhash[$line]++ ))
done < $WORD_FILE

echo -e "\nUnique words from: $WORD_FILE"
for key in "${!myhash[@]}"; do
    echo "$key"
done | sort

echo -e "\nWord counts, ordered by word, from: $WORD_FILE"
for key in "${!myhash[@]}"; do
    printf "%s\t%d\n" $key ${myhash[$key]}
done | sort

echo -e "\nWord counts, ordered by count, from: $WORD_FILE"
for key in "${!myhash[@]}"; do
    printf "%s\t%d\n" $key ${myhash[$key]}
done | sort -k2,2n
```

1 我们将创建一个临时文件并设置一个 trap（第 97 页的“It's a Trap!”）来清理它。

+   2 我们必须用 -A 声明哈希作为 bash 关联数组（再次说明，我们在这里称之为“哈希”而不是“hash”，因为标志的原因）。

+   3 获取元素的计数，或我们想要的随机数的范围。

+   4 使用 bash $RANDOM 变量来挑选列表中的随机元素（示例 4-3）。

+   5 将随机单词回显到临时文件中。我们用了三行（标注 ③、④ 和 ⑤）来做一件本可以用一行完成的事情，比如 echo "${mylist[$[$RANDOM % ${#mylist[@]}]]}" >> $WORD_FILE，但我们认为那一行六个月后会很难重新阅读。

+   6 读取我们刚刚创建的文件。注意在 ⑧ 中 done 关键字之后的文件名位置。

+   7 在哈希中为文件中行中看到的单词递增“键”值。

+   8 注意来自 ⑥ 中循环的 done 关键字之后的文件名位置。

+   9 遍历键以显示单词列表，不重复，且不使用 uniq 外部命令。注意 done 关键字之后我们使用的 sort 命令。

+   10 再次遍历键，但这次同时显示计数的“值”。

+   11 最后一次遍历键以再次显示计数，但这次按数字对第二字段排序（sort -k2,2n）。

示例 7-6 包含输出。

### Example 7-6. bash word count example: output

```
Creating & reading random word list in: /tmp/words.txt

Unique words from: /tmp/words.txt
bar
baz
foo
four
one
three
two

Word counts, ordered by word, from: /tmp/words.txt
bar      7
baz      6
```

```
foo      4
four     3
one      5
three    4
two      6
```

```
Word counts, ordered by count, from: /tmp/words.txt
four     3
foo      4
three    4
one      5
baz      6
two      6
bar      7
```

#### Style and Readability: Recap

In this chapter, we’ve demystified bash array (list and hash) handling and showed idiomatic solutions for the common use cases. While outside the scope of this book, there’s much more to say about lists and hashes in bash. To learn more, see the following resources:

- https://www.gnu.org/software/bash/manual/html_node/Arrays.html#Arrays
- http://wiki.bash-hackers.org/syntax/arrays
- http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_10_02.html
- https://learning.oreilly.com/library/view/bash-cookbook-2nd/9781491975329
- man uniq
- man sort

When you get the data structure correct, the rest of the code practically writes itself. When you get the data structure wrong, everything is a struggle. Bash has the building blocks for simple data structures, and once you get used to a little extra punctuation, they’re not that hard to use and read. Just remember: you almost always want [@] and not [*], and refer back to our cheat sheet examples in this chapter when you need to.
