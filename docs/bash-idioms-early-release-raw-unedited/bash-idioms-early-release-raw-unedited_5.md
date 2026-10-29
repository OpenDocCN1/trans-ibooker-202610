# Chapter 4. List and Hash Handling

## A NOTE FOR EARLY RELEASE READERS

对于 Early Release 电子书，你可以在官方发布这些书之前很久就获得书籍的早期形式——作者在写作时的原始未编辑内容——从而利用这些技术。

这将是最终书籍的第 7 章。请注意 GitHub 仓库将在稍后激活。

如果你对如何改进本书的内容和/或示例有任何评论，或者如果你注意到本章中缺少材料，请联系作者 bash-idioms@jpsdomain.org。

正如我们所知，计算机在计数和组织数据方面非常擅长。我们可以使用代码来计数和组织数据，使用 *数据结构*，而其构建块就是数组。Bash 从一开始就有了数组，并在 4.0 版本中添加了“关联数组”。你在 bash 数组中会遇到难以阅读的代码，部分原因是 bash 有很多历史和向后兼容性需要维护，正如我们前面提到的，但也因为一些开发者倾向于过度复杂化。数组实际上并不难实现，只要稍加思考，你就可以清晰地写出它们。

作为复习，在计算机科学和编程中，数组是包含多个元素并由整数索引或引用的变量。换句话说，数组是一个包含

列表而不是标量或单个值的变量。关联数组是一种按字符串而不是整数索引的列表类型。所以它是一个键值对列表，从根本上形成了一个字典或查找表，其中键在内部被哈希以形成内存位置。差不多就是这样。

bash 文档使用“array”和“associative arrays”术语，但根据你的背景，“lists”和“hashes”，或者可能是“dictionaries”或“dicts”，可能更熟悉，而且它们肯定更容易输入和说。bash 文档还使用“subscript”，而其他人可能会说“index”。我们通常为了保持一致而遵循 bash 文档，但对于这些，我们将使用更常见且更易于理解的“list”、“hash”和“subscript”术语。

尽管列表（数组）出现得更早，但哈希（关联数组）稍微简单一些，因为永远不会对提供索引（下标）产生疑问，因为它是必需的。列表的整数索引可能是隐含的，虽然它并不复杂，但有一些操作适用于它们，而对哈希则毫无意义。

正如 bash 参考手册所说，*Bash provides one-dimensional indexed and associative array variables.* 虽然可以创建非常丑陋的多维结构，但在 bash 中这样做很可能会以眼泪告终。如果你真的需要这样做，请考虑在另一种语言中实现。

## WATCH OUT FOR BASH VERSION WITH HASHES

你真的需要小心你的 bash 版本。正如我们刚才所说，bash 直到 4.0 版本才获得关联数组，并且花了几个版本才理清一些细节，例如允许 `$list[-1]` 引用最后一个整数元素（v4.3），而不是需要 `mylist[${#mylist[*]}-1]`. 哎呀。

正如我们在 *earlier content* 中所说，要注意 Mac 上的 stock `bash`，它相当老旧。你可以在 MacPorts、Homebrew 或 Fink 中找到更新的版本。

此外，数组（列表和哈希）未被 POSIX 指定，因此如果可移植性超出 bash 的范围是一个问题，你在使用它们时需要非常小心，因为它们可能无法工作。

## Commonalities

在 bash 中，列表和哈希非常相似，所以我们将从它们共同之处开始，然后再讨论它们的不同之处。事实上，你可以将列表视为哈希的一个子集，只是恰好具有有序的整数索引。我们不认为你*应该*这样做，但你*可以*。

列表本质上是有序的，而哈希本质上是无序的，因此像 `shift` 或 `push` 这样的操作只对有序列表有意义。另一方面，你永远不需要对有序列表的键进行排序，但对于哈希你可能需要。

> **ACCIDENTAL ASSIGNMENT**
> 
> 没有 *subscript* 的意外赋值会影响元素零，因此 `myarray=foo` 会导致创建或覆盖 `$myarray[0]` 为 `foo`，即使它是一个哈希！

来自 bash 文档：

> *If the subscript is @ or *, the word expands to all members of the array name. These subscripts differ only when the word appears within double quotes. If the word is double-quoted, `${name[*]}` expands to a single word with the value of each array member separated by the first character of the IFS variable, and `${name[@]}` expands each element of name to a separate word.*

这不是很拗口吗？但它很重要，因为，正如我们将展示的那样，如果你做错了，你可能会伤害自己。我们在下面的示例代码中使用了 printf "%q" 并用管道（|）分隔符来显示“shell quoted”字符串（技术上是“words”），以便在屏幕上或本页上的输出中可见。引用规则实际上与我们在第 2 章中介绍的相同，只是放在列表或哈希的上下文中。

## BEWARE OF UNSET

我们下面使用了 unset，并且有一个微妙的陷阱。如果恰好有一个与变量同名的文件，globbing ¹ 可能会伤害你，你可能会破坏意想不到的东西。为了避免这种情况，最好引用你的变量。使用 -v 来强制 unset 将你的参数视为 shell 变量会更安全，例如 unset -v 'list'。

## Lists

正如我们所说，数组，也称为列表，是包含多个元素并由整数索引或引用的变量。

在 bash 中，它们从零开始，可以使用 declare -a、local -a、readonly -a 声明，或通过简单地为新变量赋值来声明，如 mylist[0]=foo 或 mylist=()（空列表）。一旦变量被声明为列表，像 mylist+=(bar) 这样的简单赋值就相当于 push，但请注意 \+ 和 () 两者都很关键。

Table 4-1. Sample bash List

| Element | Value |
|---|---|
| mylist[0] | foo |
| mylist[1] | bar |
| mylist[2] | baz |





对数组或列表的常见操作有：

- 声明一个变量为列表
- 为其分配一个或多个值
- 如果将其作为栈处理（想想自助餐厅里的盘子；FIFO：先进先出，或先入先出）
  - push
  - pop
- 显示（dump）所有值用于调试或复用
- 引用一个或所有值（`for`，或 `for each`）
- 引用一个子集（slice）的值
- 删除一个或多个值
- 删除整个列表

与其谈论所有这些，我们将直接向你展示，你可以根据需要挑选所需的惯用法：

### Example 4-1. Bash List Example: Code

```
#!/usr/bin/env bash
#### lists.sh: bash list example code
#### Original Author & date: _bash Idioms_ 2022
#### bash Idioms filename: examples/ch07/lists.sh
#
____________________________________________________________
________________________________

#### Declare a list
①
#### declare -a mylist    #### Can do this, or `local -a` or
`readonly -a` or:
mylist[0]='foo'        # This both declares and assigns to
mylist[0]

#### Push or assign, note the +=
②
```

```
###mylist=(bar)                # Would overwrite mylist[0]
mylist+=(bar)                  # mylist[1]
mylist+=(baz)                  # mylist[2]
mylist+=(one two)              # mylist[3] AND mylist[4]
mylist+=("three four")       # mylist[5]

#### OR
#### mylist=(bar baz one two "three four")  #### Note "foo" is
already there

#### Display or dump the values
③
echo -e "\nThe element count is: ${#mylist[@]} or ${#mylist[*]}"

echo -e "\nThe length of element [5] is: ${#mylist[5]}"

echo -e "\nDump or list:"
declare -p mylist
echo -n      "${mylist[@]}	 = " ; printf "%q|" ${mylist[@]}
echo -en  "\n\${mylist[*]}	 = " ; printf "%q|" ${mylist[*]}
echo -en "\n\"${mylist[@]}\" = " ; printf "%q|" "${mylist[@]}"
echo -en "\n\"${mylist[*]}\" = " ; printf "%q|" "${mylist[*]}"
echo -e "\t# But this is broken!"  # Previous line is bad and no newline
#### See `help printf` or chapter 6, we need this to show the correct words:
#### %q     quote the argument in a way that can be reused as shell input

#### Iterate over the values
④
echo -e "\nforeach \"\${!mylist[@]}\":"
for element in "${!mylist[@]}"; do
    echo -e "\tElement: $element; value: ${mylist[$element]}"
done

echo -e "\nBut don't do this: \${mylist[*]}"
for element in ${mylist[*]}; do
    echo -e "\tElement: $element; value: ${mylist[$element]}"
done
```

```
#### Handle slices (sub-sets) of the list, shift and pop
⑤
echo -e "\nStart from element 3 and show a slice of 2 elements:"
printf "%q| " "${mylist[@]:3:2}"
echo '' # No newline in above

echo -e "\nShift FIRST element [0] (dumped before and after):"
declare -p mylist                # Display before
mylist=("${mylist[@]:1}")       # First element, needs quotes
#mylist=("${mylist[@]:$count}")  # First #count elements
declare -p mylist                # Display after

echo -e "\nPop LAST element (dumped before and after):"
declare -p mylist
unset -v 'mylist[-1]'            # Bash v4.3+
#unset -v "${mylist[${#mylist[*]}-1]}"  # Older
declare -p mylist

#### Delete slices or the entire list
⑥
echo -e "\nDelete element 2 using unset (dumped before and after):"
declare -p mylist
unset -v 'mylist[2]'
declare -p mylist

#### Delete the entire list
unset -v mylist
```

- ① 声明一个变量为数组（我们在这里称之为“数组”而不是“列表”，因为标志是 -a）
- ② 为其分配一个或多个值
- ③ 显示（dump）所有值用于调试或复用
- ④ 遍历值

- ⑤ 处理列表的切片（子集）、shift 和 pop
- ⑥ 删除切片或整个列表

运行时的效果如下：

### Example 4-2. Bash List Example: Output

```
The element count is: 6 or 6

The length of element [5] is: 10

Dump or list:
declare -a mylist=([0]="foo" [1]="bar" [2]="baz" [3]="one" [4]="two" [5]="three four")
${mylist[@]}	= foo|bar|baz|one|two|three|four|
${mylist[*]}	= foo|bar|baz|one|two|three|four|
"${mylist[@]}" = foo|bar|baz|one|two|three\ four|
"${mylist[*]}" = foo\ bar\ baz\ one\ two\ three\ four|  # But this is broken!

foreach "${!mylist[@]}":
    Element: 0;  value: foo
    Element: 1;  value: bar
    Element: 2;  value: baz
    Element: 3;  value: one
    Element: 4;  value: two
    Element: 5;  value: three four

But don't do this: ${mylist[*]}
    Element: foo; value: foo
    Element: bar; value: foo
    Element: baz; value: foo
    Element: one; value: foo
    Element: two; value: foo
    Element: three; value: foo
    Element: four; value: foo

Start from element 3 and show a slice of 2 elements:
one|two|

Shift FIRST element [0] (dumped before and after):
declare -a mylist=([0]="foo" [1]="bar" [2]="baz" [3]="one"
[4]="two" [5]="three four")
declare -a mylist=([0]="bar" [1]="baz" [2]="one" [3]="two" [4]="three four")
```

Pop LAST element (dumped before and after):

```
declare -a mylist=([0]="bar" [1]="baz" [2]="one" [3]="two" [4]="three four")
declare -a mylist=([0]="bar" [1]="baz" [2]="one" [3]="two")
```

Delete element 2 using unset (dumped before and after):

```
declare -a mylist=([0]="bar" [1]="baz" [2]="one" [3]="two")
declare -a mylist=([0]="bar" [1]="baz" [3]="two")
```

## Hashes

也称为哈希、字典或 dict，关联数组是索引为任意字符串而不是整数的列表。它们在计数或“唯一化”（即忽略或删除重复）字符串等方面非常方便。

与列表不同，这些*必须*使用 `declare -A`、`local -A` 或 `readonly -A` 声明，并且下标始终是必需的。

*Table 4-2. Sample bash Hash*

| Element | Value |
|---|---|
| myhash[oof] | foo |
| myhash[rab] | bar |
| myhash[zab] | baz |

对关联数组或哈希或 dict 的常见操作有：

- 声明一个变量为关联数组（我们在这里称之为“哈希”因为标志是 -A）
- 为其分配一个或多个值
- 显示（dump）所有值用于调试或复用
- 引用一个或所有值（`for`，或 `for each`）
- 引用特定值（查找）
- 删除一个或多个值
- 删除整个哈希

同样，与其谈论所有这些，我们只是向你展示以便挑选所需的惯用法：

### Example 4-3. Bash Hash Example: Code

```
#!/usr/bin/env bash
#### hashes.sh: bash Hash example code
#### Original Author & date: _bash Idioms_ 2022
#### bash Idioms filename: examples/ch07/hashes.sh
#
________________________________________________________

#### Declare a hash
declare -A myhash   # MUST do this, or `local -A` or `readonly -A`

#### Assign to it, note no "+"
##myhash=(bar)               # Would create or overwrite myhash[0]
myhash[a]='foo'
myhash[b]='bar'
myhash[c]='baz'
myhash[d]='one two'          # Different than our list example
myhash[e]='three four'

#### OR
#myhash=( [a]=foo [b]=bar [c]=baz [d]="one two" [e]="three four")

#### Display or dump the details and values
echo -e "\nThe key count is: ${#myhash[@] } or ${#myhash[*]}"
```

```
echo -e "\nThe length of the value of key [e] is:
${#myhash[e]}"

echo -e "\nDump or list:"
declare -p myhash
echo -n "\${myhash[@}]      = " ; printf "%q|"
${myhash[@]}
echo -en "\n\${myhash[*]}      = " ; printf "%q|"
${myhash[*]}
echo -en "\n\"\${myhash[@]}\" = " ; printf "%q|"
"${myhash[@]}"
echo -en "\n\"\${myhash[*]}\" = " ; printf "%q|"
"${myhash[*]}"
echo ''  # No newline in above
#### See `help printf` or chapter 6, we need this to show the correct words:
#### %q    quote the argument in a way that can be reused as shell input

#### Iterate over the keys and values
④
echo -e "\nforeach \"\${!myhash[@]}\":"
for key in "\${!myhash[@]}"; do
    echo -e "\tKey: $key; value: ${myhash[$key]}"
done

echo -e "\nBut don't do this: \${myhash[*]}"
for key in ${myhash[*]}; do
    echo -e "\tKey: $key; value: ${myhash[$key]}"
done

#### Handle slices (sub-sets) of the hash
⑤
echo -e "\nStart from element 3 and show a slice of 2 elements:"
printf "%q|" "${myhash[@]:3:2}"
echo ''  # No newline in above

#echo -e "\nShift FIRST key [0]:"   = makes no sense in a hash!
#echo -e "\nPop LAST key:"          = makes no sense in a hash!

#### Delete keys or the entire hash
⑥
echo -e "\nDelete key c using unset (dumped before and after):"
```





declare -p myhash
unset -v 'myhash[c]'
declare -p myhash

#### Delete the entire hash
unset -v myhash

- 1. Declare a hash
- 2. Assign to a hash
- 3. Display or dump the details and values
- 4. Iterate over the keys and values
- 5. Handle slices (sub-sets) of the hash
- 6. Delete elements or the entire hash

That looks like this when you run it:

### Example 4-4. Bash Hash Example: Output

```
The key count is: 5 or 5

The length of the value of key [e] is: 10

Dump or list:
declare -A myhash=([a]="foo" [b]="bar" [c]="baz" [d]="one two" [e]="three four")
${myhash[@] }    = foo|bar|baz|one|two|three|four|
${myhash[*] }    = foo|bar|baz|one|two|three|four|
"${myhash[@]}" = foo|bar|baz|one\ two|three\ four|
"${myhash[*]}" = foo\ bar\ baz\ one\ two\ three\ four|

foreach "${!myhash[@]}":
    Key: a; value: foo
    Key: b; value: bar
    Key: c; value: baz
Key: d; value: one two
Key: e; value: three four

But don't do this: ${myhash[*]}
    Key: foo; value:
    Key: bar; value:
    Key: baz; value:
    Key: one; value:
    Key: two; value:
    Key: three; value:
    Key: four; value:

Start from element 3 and show a slice of 2 elements:
baz|one\ two|

Delete key c using unset (dumped before and after):
declare -A myhash=( [a]="foo" [b]="bar" [c]="baz" [d]="one two" [e]="three four" )
declare -A myhash=( [a]="foo" [b]="bar" [d]="one two" [e]="three four" )

## A simple word count example

As we said, one of the most common uses of a hash is to count and/or “uniq” items, so here is a simple but concrete example.

### Example 4-5. Bash word count example: Code

```
#!/usr/bin/env bash
#### word-count-example.sh: More examples for bash lists and hashes, and $RANDOM
#### Original Author & date: _bash Idioms_ 2022
#### bash Idioms filename: examples/ch07/word-count-example.sh
#### ________________________________
#### See also: `man uniq`
```

```
WORD_FILE='/tmp/words.txt'
> $WORD_FILE
①
trap "rm -f $WORD_FILE" ABRT EXIT HUP INT QUIT TERM

declare -A myhash
②
```

```
echo "Creating & reading random word list in: $WORD_FILE"

#### Create a list of words to use for the hash example
mylist=(foo bar baz one two three four)

#### Loop, and randomly pick elements out of the list
range="${#mylist[@]}"
for ((i=0; i<35; i++)); do
  random_element="$(( $RANDOM % $range ))"
  echo "${mylist[$random_element]}" >> $WORD_FILE
done

#### Read the word list into a hash
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

We’ll create a temporary file and set a trap (Chapter 9) to clean it up.

2 We have to `declare -A` the hash as a bash *associative array* (again, we say that instead of “hash” here because of the flag).

3 Get the count of elements, or the *range* for the random number we want.

4 Use the bash `$RANDOM` variable to pick a random list element (Chapter 4).

5 Echo the random word into the temp file. We used three lines (<3>, <4>, and <5>) to do something we could do in one line like `echo "${mylist[$ $RANDOM % ${#mylist[@]}]}" >> $WORD_FILE` but we think that one line would be a lot harder to re-read six months from now.

6 Read the file we just created, note the location of the file name *after* the `done` keyword <8>.

7 Increment the “key” value in hash for the word we saw in the line in the file.

8 Note the location of the file name *after* the `done` keyword, from the loop in <6>.

9 Iterate over the keys to display a list of the words, without duplicates, and without using the `uniq` external command. Note the `sort` command we did use after the `done` keyword.

10

Iterate over the keys again, but this time display the “value” of the count as well.

- 10. Iterate over the keys one last time to display the count again, but this time sorted the second field as a number (sort -k2,2n).

### Example 4-6. Bash word count example: Output

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
foo      4
four     3
one      5
three    4
two      6

Word counts, ordered by count, from: /tmp/words.txt
four     3
foo      4
three    4
one      5
baz      6
two      6
bar      7
```

## Style and Readability

When you get the data structure correct, the rest of the code practically writes itself. When you get the data structure wrong,

everything is a struggle. Bash has the building blocks for simple data structures, and once you get used to a little extra punctuation, they’re not that hard to use and read. Just remember you almost always want [@] and not [*], and refer back to your cheat sheet examples when you need to.

## Summary

In this chapter we’ve demystified bash array (list and hash) handling and showed idiomatic solutions for the common use-cases.

See also:

- - https://www.gnu.org/software/bash/manual/html_node/Arrays.html#Arrays
- - http://wiki.bash-hackers.org/syntax/arrays
- - http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_10_02.html
- - man uniq
- - man sort

understanding. Sure, modern systems and circumstances are complex, so try hard not to make it any worse than it already is.

- - The corollary, as Brian Kernighan famously said, is that debugging is twice as hard as writing the code in the first place, so if your code is as clever as you can make it, you are—by definition—not smart enough to debug it.
- - Try not to re-invent the wheel. Whatever you are trying to do has probably been done before, and there’s likely a tool or library for it. If that tool is already installed, just use it. No matter how how hard you try you are never going to be able to match the effort and testing that went into `rsync`, so just use it. If you find random code on the internet…well, think about that one for a bit.
- - Plan ahead for special cases or overrides, since they will happen. Take a page out of Linux distribution packaging systems and provide an `/etc/thing/global.cfg` for defaults that you can blindly overwrite, then allow for overrides in `/etc/thing/config.d/` or similar, see *The dropin directories section in Chapter 9*.
- - If it’s not in revision control it doesn’t exist! (And sooner or later, it will be lost and then *really* won’t exist.)
- - Document everything. (But you don’t have to write a book about it…oh wait…) Write your code, your comments, and your docs for the new person who will join the team a year from now, when you’ve forgotten *why* you did it *that* way, so you can’t explain it to them. Document what *didn’t* work, and maybe why, and cross-reference things, especially things that you can predict can hurt you. (Yes, `rm -rf /$undefined_variable` turned out to be a *really bad idea!*)

- - Keep your code and documentation DRY; Don’t Repeat Yourself. As soon as you end up with more than one copy, it’s guaranteed they will get out of sync sooner or later, the only question is when. Use functions and libraries, don’t be WET (We Enjoy Typing).

## BASH IDIOMS STYLE GUIDE IS NOT PORTABLE

This Bash Idioms Style Guide is specifically for Bash, so it is not portable to POSIX, Bourne, Dash or other shells.! If you need to write for those shells you will need to test and tweak this guide to account for the supported syntax and feature of those shells.

Getting down into more detail, what types of things should be captured in your style guide? We’ll cover some of them in the following sections.

## Readability

Readability of your code is important! Or as Python says, *Readability counts*. You only write it once, but you (and others) will probably read it many times. Spend the extra few seconds or minutes thinking about the poor clueless person trying to read the code next year…it’s very likely to be you. There’s a balance and a tension between abstraction (Don’t Repeat Yourself) and readability.

- - KISS (Keep it Simple, Stupid!)
- - READABILITY: don’t be “clever,” be clear
- - Good names are critical!
- - ALWAYS USE A HEADER
- - If at all possible, emit something useful for `-h`, `--help` and incorrect arguments!





- 优先使用“here”文档（以前导 TAB 开头）而不是一堆 echo 行
- 使用 `source`（而不是 `.`）来包含配置文件，这些文件应以 `.cfg`（或 `.conf` 或你使用的标准）结尾
- 如果可能，对所有内容都使用 ISO-8601 日期
- 如果可能，保持事物列表（例如 ip 地址、主机名、要安装的软件包等）按字母顺序排列；这可以防止重复并使添加或删除项目更容易
  - 这适用于 `case` 语句、变量或数组/列表的内容，以及任何有意义的地方
- 如果可能，使用命令的长参数以提高可读性，例如使用 `diff --quiet` 而不是 `diff -q`，不过要注意在非 GNU/Linux 系统上的可移植性
  - 如果任何选项很短或晦涩，请添加注释。
  - 还应强烈考虑记录你选择或需要这些选项的原因，甚至记录你考虑过但因某种原因未使用的选项。
  - 一定要记录那些看起来像是好主意但实际上可能导致问题的选项，尤其是那些你在其他地方经常使用的选项。

对于变量命名，要注意像以下这样的通用变量名：

```
`${GREP} ${pattern} ${file}`
```

这被抽象得非常厉害，也许可以重用，但它也非常缺乏上下文。我们已将旧的 `` 替换为更新且更

添加 *有用* 的注释来解释 *为什么* 你这样做以及你的意图！例如：

```
continue # to the next file in for loop
```

理论上，如果你写出清晰的代码，就不需要解释 *什么* 东西在做什么。但有时 bash 本身就很繁琐或晦涩，例如变量替换：

```
PROGRAM=${0##*/}   # Basename in bash, see "Prefix Removal" in Chapter 4
```

分隔符对于分隔代码的逻辑块通常很有用。但不要在底部添加关闭分隔符来“框”住它，这只会增加混乱并减少你屏幕上的代码行数 1 行。

无论如何，*不要* 在右侧用字符构建框！这完全没有必要，你会浪费大量时间让它“看起来正确”。更糟糕的是，这会强烈阻碍以后修复或更新注释，因为你又得重新修复框。

不要这样做：

```
################################################################################################
######
#### Please don't build boxes like this!
#
#
#
#### They make it VERY painful to edit the comments later,
because #
#### now you have to worry about the closing character on the
#
#### right-hand-side.  This example isn't too bad, but it gets
#
#### out of hand quickly.
#
################################################################################################
######
```

这样做：

```
######################################
######
#### Please DO build boxes like this!
#
#### This one is easier to edit, because while you still have to
#### worry about text wrapping, it's just a simple leading "####" when
#### do do re-wrap.
```

## Names

命名 **很重要**。我们不能再强调这一点。`$file` 和 `$critical_process_daily_log_file` 之间的区别现在看起来除了多打字 *之外* 什么都没有，当所有细节都在你脑子里时。但我们保证，如果你花额外的时间认真思考你命名的对象以及它在代码中读起来如何，这很快就会得到回报，减少编码错误，并在将来重读、调试和增强时受益。以下是与名称相关的风格指南：

- 好的名字至关重要！
- 全局变量和常量使用大写
  - 尽量不要更改全局变量，但有时这样做要简单得多
  - 对常量使用 `readonly` 或 `declare -r`
- 其他变量使用小写
- 函数使用 Mixed_Case
- 使用 "_" 代替空格或 CamelCase（记住 "-" 不允许在变量名中使用）

- 函数必须在被使用前定义
  - 将它们分组在顶部，并在每个函数之间使用两行空白和函数分隔符。
  - 不要在函数之间穿插代码！
- 使用 Camel_Case 和 "_" 使函数名与变量名区分开来。
- 使用 `function My_Func_Name {` 而不是 `My_Func_Name() {` 因为它更清晰，更容易 `grep -P '^\s*function '`。
- 每个函数都应有注释定义：它做什么、输入（包括 GLOBALS）和输出
- 当你有有用的、独立的代码片段时，将其做成函数。
- 只要你多次使用相同（或基本相似）的代码块，就使用函数。如果它们很常见，比如日志记录或邮件发送，请考虑创建一个“库”，即一个你可以按需 source 的单一公共文件。
  - 为“库”函数加上前缀 "_"，如 `_Log`，或其他前缀。
- 如果有意义，考虑在参数中使用“填充”词以提高可读性，然后将其定义为 `local junk1="$2" # unused filler`，例如：
  - `_Create_File_If_Needed "/path/to/$file" containing important value`
- 在函数中设置变量时请使用 `local` 内置命令。

- 但要注意它会掩盖返回码，因此如果使用命令替换，请在单独的行上声明和赋值，例如 `local my_input` 然后 `my_input="$(some-command)"`。
- 对于任何超过约 25 行的函数，使用 `}  # end of function MyFuncName` 来关闭它，以便在屏幕上更容易跟踪你在代码中的位置。对于少于 25 行的函数这是可选的，但鼓励使用，除非变得太混乱。
- 不要使用 `main` 函数，它几乎总是多余的一层。
  - 尽管如此，使用“main”对 Python 和 C 程序员来说是有意义的，或者如果代码也用作库，并且如果你做大量单元测试时可能需要。
- 考虑在主部分上方使用 2 行空白和主分隔符，尤其是在顶部有很多函数和定义时。

另外，在你的库中定义一个单一的日志函数（例如 `_Log`），并使用它！否则你最终会得到各种各样的日志函数、样式和目的地。理想情况下，正如我们上面所说，将日志记录到 syslog，让操作系统担心最终目的地、日志轮转等。

## Quoting

引用很简单，直到它变得不简单然后你就头疼了。我们知道这是怎么回事，我们仍然有时会做试错，尤其是尝试创建作为另一用户通过 `sudo` 运行或通过 `ssh` 在另一节点上运行的一行命令时。添加一些 `echo` 行，小心。以下是与引用相关的风格指南：

- 确实要在变量和字符串周围加上引号，因为它使它们更突出并澄清你的意图
  - 除非变得太混乱
  - 或它们需要不加引号以进行展开
- 不要引用整数
- 除非需要插值，否则使用单引号
- 不要使用 `${var}` 除非需要，它太混乱了
  - 但有时确实需要，例如
    `${variable}_suffix` 或
    `${being_lower_cased,,}`
- 对命令替换加引号，例如：`var="$(command)"`
- 对任何测试语句的两侧 *始终* 加引号，例如 [[
  "$foo" == bar ]]
  - FIXME-TOOLS: AsciiDoc missing single quotes again!
  - 除非你使用 ~=，在这种情况下你不能引用正则表达式！
- 考虑在 echo 语句中对变量使用单引号，例如 echo "cd to $DIR failed." 因为当变量意外未定义或为空时更清晰
  - FIXME-TOOLS: AsciiDoc missing single quotes again!
  - 或 echo "cd to [$DIR] failed." 随你喜欢
- 优先在 printf 格式周围使用单引号（参见第 6 章）

和一个 IDE。首先，这取决于团队和个人，可能并非如此，其次即使是这样，当它 *真的* 坏掉并且你最终在 80x24 控制台上的 `vi` 中在火线调试时，你不会欣赏 200+ 列的代码。

换行。

在重要部分 *之前* 换行，这样当你扫描列的左侧时，续行会跳出来。我们还喜欢对换行的行使用通常缩进的一半。我们无法在此书中做一个好的（坏的？）长行示例，因为它只会在奇怪的地方断开。但我们可以提供一个简单且我们认为可读的示例：

```
... Lots of code, indented a ways...
    /long/path/to/some/interesting/command \
      | grep "$re_stuff_we_care_about \
      | grep -v "$re_stuff_to_ignore \
      | /path/to/email_or_log_command ...
...more code
```

## Syntax

以下是与语法相关的风格指南：

- 编写 bash 代码时使用 `#!/bin/bash` 或 `#!/usr/bin/env bash`，而不是 `#!/bin/sh`
  - FIXME-TOOLS: there’s a Markdown/Asciidoctor rendering issue with the three # ! above
- 除非你 *真的* 确定需要 `$*`，否则使用 `$@`
- 使用 `==` 而不是 `=` 进行相等比较，以减少与赋值的混淆
- 使用 `$()` 而不是反引号/反引号





- FIXME-TOOLS: Markdown/Asciidoctor 渲染在反引号处存在问题
- 使用 [[ 而不是 [
- 使用 0 1 和 2 3 进行整数运算时按需使用，避免使用 let 和 expr
- 当简单且可读时使用 [[ expression ]] && block 或 [[ expression ]] || block。不要使用 [[ expression ]] && block || block，因为它不会按你想的那样工作，对于这种情况请使用 if .. (elif ..) then .. else。

## Other

其他指南：

- 对于“system”脚本，将日志记录到 syslog，让操作系统处理最终目的地、日志轮转等。
- 错误信息应输出到 STDOUT，如 echo 'A Bad Thing happened' >&2
- 检查外部工具是否可用
- 失败时提供有用的信息
- 设置退出码，尤其是在失败时

## Script Template

这是一个示例骨架或模板脚本，你可以将其复制作为提醒，并在创建新脚本时节省一些输入。

```
#!/bin/bash -
#### Or possibly: ####!/usr/bin/env bash
#### <Name>: <description>
```
```
#### Original Author & date:
#### Current maintainer?
#### Copyright/License?
#### Where this code belongs?  (Hosts, paths, etc.)
#### Project/repo?
#### Caveats/gotchas?
#### Usage?  (Better to have `-h` and/or `--help` options!)
#### $URL$  #### If using SVN
ID=' '    # If using SVN
#
________________________________________________________
________________________________________________________
PROGRAM=${0##*/}  # bash version of `basename`

#### GLOBAL and constant variables are in UPPER case
LOG_DIR='/path/to/log/dir'

#### Functions are in Mixed Case
#####################################################################
################
#### Define functions

#--------------------------------------------------------------------
----------------
#### Example function
#### Globals: none
#### Input:   nothing
#### Output: nothing
function foo {
    local var1="$1"
    ...
} # end of function foo

#--------------------------------------------------------------------
----------------
#### Another example function
#### Globals: none
#### Input:   nothing
#### Output: nothing
function bar {
    local var1="$1"
    ...
} # end of function bar
```
```
##############################
################
#### Main
...
```

## Other Style Guides

我们强烈建议你制定一份风格指南并使用它！如果你不喜欢我们的，并且你也不想自己调整，你可以从别处偷一个。

- https://google.github.io/styleguide/shellguide.html
  - 虽然我们对其中一些内容有一些异议，但它非常好且相当全面。最好你可以直接使用它，最差的情况它也是一个起点。有很多值得喜欢的地方，你可以做得更差（比如什么都没有），而且很多其他项目也采用了它，所以…
  - 除其他事项外，我们不同意：
    - 100 行限制。我们看到了这一点，尤其是在 Google 这样的环境中，但我们有很多超过该限制的脚本，它们“主要是调用其他实用程序并且数据操作相对较少。”
    - “缩进 2 个空格”对我们来说有点浅，我们喜欢对续行使用 2 个空格，其他情况使用 4 个空格。（哎呀，我们本来不想承认的，是吧？）
    - "${var}" 太繁琐了，我们在可能的情况下更喜欢 "$var"。
    - function cleanup() {? 哎呀，不行。

- } } 关闭 +case..esac 块，应与块的开头保持相同的缩进，而不是与块中代码的缩进相同

- 以下是可以参考的其他指南：

  - https://linuxcommand.org/lc3_adv_standards.php

  - https://www.ovirt.org/develop/infra/infra-bash-style-guide.html

  - https://wiki.bash-hackers.org/scripting/style

  - http://mywiki.wooledge.org/BashGuide/Practices

  - 很好的信息
    http://mywiki.wooledge.org/BashPitfalls

- 或在网上搜索

  - "Shell Style Guide"

  - "Bash Style Guide"

  - "Shell script coding standards"

## Bash Linter

使用“linter”作为风格指南的替代或补充也很方便。说实话，我们没有使用，但我们认为应该涵盖一下。我们知道的是 shellcheck，我们对结果有 mixed feelings，它对引号特别挑剔，我们也不总是同意它关于引号或其他方面的建议，但这些东西或多或少是可以调整的。话虽如此，它仍然非常酷，值得一看。

- Bash linter: https://www.shellcheck.net/

  - https://github.com/koalaman/shellcheck (Haskell)

- History: https://www.vidarholen.net/contents/blog/?p=859
- Checks:
  https://github.com/koalaman/shellcheck/wiki/Checks
- Tweaking:
  https://github.com/koalaman/shellcheck/wiki/Directive
- Ignoring errors:
  https://github.com/koalaman/shellcheck/wiki/Ignore
- 注意，Eol (2020) CentOS-6 太旧了：
  https://github.com/koalaman/shellcheck/wiki/CentOS6

如果你通过包管理系统安装，请记住版本可能相当旧。Linux 版本是一个包含单个编译二进制文件的 tarball，所以很容易将当前版本放到你的路径中的某个位置。

## Summary

希望，这本书能让你更好地理解如何阅读和编写“bashistic”代码，本章也为你开始定义一个适合你的风格（和指南）提供了一个起点。

Happy bashing!
