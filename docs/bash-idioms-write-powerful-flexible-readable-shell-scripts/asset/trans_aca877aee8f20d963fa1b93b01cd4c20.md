

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
# Original Author & date: _bash Idioms_ 2022
# bash Idioms filename: examples/ch07/word-count-example.sh
# ----------------------------------------------------------------------------
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

## Style and Readability: Recap

In this chapter, we’ve demystified bash array (list and hash) handling and showed idiomatic solutions for the common use cases. While outside the scope of this book, there’s much more to say about lists and hashes in bash. To learn more, see the following resources:

- https://www.gnu.org/software/bash/manual/html_node/Arrays.html#Arrays
- http://wiki.bash-hackers.org/syntax/arrays
- http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_10_02.html
- https://learning.oreilly.com/library/view/bash-cookbook-2nd/9781491975329
- man uniq
- man sort

When you get the data structure correct, the rest of the code practically writes itself. When you get the data structure wrong, everything is a struggle. Bash has the building blocks for simple data structures, and once you get used to a little extra punctuation, they’re not that hard to use and read. Just remember: you almost always want [@] and not [*], and refer back to our cheat sheet examples in this chapter when you need to.

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

