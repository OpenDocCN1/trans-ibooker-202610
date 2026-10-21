## Functional Framework

The two preceding chapters have described the bash idioms in using variables and then combining those variables into expressions. The next level up is grouping those expressions and statements into functions that can be called from various locations within a bash shell script. Even though bash does support the useful construct of functions, it does so in a very bashy way. Let’s see how it differs from what you know from other languages.

## Calling Functions

Here are three statements that are examples of calling bash functions (which we are making up for this example):

```
Do_Something
Find_File 25 $MPATH $ECODE
Show_All $*
```

Those don’t look like function calls, you might be thinking. They look just like any command line invocation of a command.

Exactly.

In other languages, you might say something like Find_File(25, MPATH, ECODE), but that is not bash. In bash, a function is called much like any command; you invoke it like you would invoke a command or a shell script. As with a bash keyword or builtin, the shell doesn’t need to create a separate process to run the function. That makes a function more efficient than calling a separate command binary or shell script.

You might also notice that these function calls are not returning a value that can be assigned to another variable. More on that in the following sections. First, let’s look at how we define functions and their parameters.

## Defining Functions

The syntax for bash includes some optional elements. Let’s say, for example, that you want to define a function called helper. Here are three different ways to begin that definition:

```
function Helper ()
function Helper
Helper ()
```

All three are equivalent. The reserved word function is optional, but if you do use it, then the parentheses are optional. We prefer to use the simple two-word version: function and then the name of our function. Not only does it clearly state what we’re doing, but it reminds us that, unlike other languages, our functions parameters are not put in parentheses. And it’s quite easy to grep for.

The body of the function follows this defining of the function’s name, usually enclosed in braces:

```
function Say_So {
    echo 'Here we are in the function named Say_So'
}
```

## Function Parameters

How are parameters defined for a bash function? They are not. Instead, when you call a bash function (described in the following paragraphs), you can supply as many parameters as you want. Typically you have a certain fixed set of parameters in mind and will write your function accordingly, but there’s nothing special that you would need to do to have a variable number of arguments.

The arguments, however many are provided, are available to the function as $1, $2, $3, and so forth, using the same syntax used for parameters to the bash script itself. Since these parameters don’t have names but only their order number, it is a very good idea to put a comment at the head of the function to document what parameters the function expects and in what order. It is also quite common to see, in the first few lines of a script, that these positional parameters are assigned to variables with more descriptive names.

Since the function’s parameters are using the $1, etc., names, you may be wondering what happens to the shell script’s parameters that are referenced using that same syntax. Calling the function doesn’t change the script’s parameters; the script’s parameters simply are not visible inside the function (unless, of course, you send them in as parameters to the function).

There is one exception, however. The $0 parameter, the one that holds the name of the script, is still there and still holds the name of the script, however it was invoked. You can still find the name of the function, though. There is an array variable named FUNCNAME that holds the call stack of the functions that have been invoked. Element 0 is always the current function, so just use $FUNCNAME (since using the array name without an index always returns the first element, i.e., the one at index zero). That’s useful in a debugging prompt; see Example 10-3.

The following function will echo out the two parameters passed in to the function:

```
function See2it {
  echo "First arg: $1"
  echo "Second arg: $2"
}
```

If insufficient arguments are supplied in calling the function, the corresponding parameters are null.

This is a simple example of echoing the parameters with which the function is called:

```
function Tell_All {
  echo "You called $FUNCNAME"
  echo "The $# args supplied are:"
  for arg in "$@"; do
    echo "$arg"
  done
}

Tell_All 47 tree /tmp
```

When that script is run, here is what the output looks like:

```
You called Tell_All
The 3 args supplied are:
47
tree
/tmp
```

## Function Return Values

Here, too, bash functions are more like scripts than they are like your typical computer language functions. The return value from bash functions is really just an exit status, the value that you can check using $? after calling the function. Just as you might check $? after calling another script or an executable, so, too, does a bash function indicate the success or failure of the function. It actually returns the exit status of the last command executed in the function.

So how is the script writer supposed to get any useful results back from a bash function? There are two typical approaches. One way is to treat the function just like you would any shell script—have its output be the return values. You might pipe that output into the next part of your script that needs the results from the function, or you might capture that output using $() notation. The $() will run the function in a subshell and you can assign the result (the function’s output) to a variable, and then use that variable on another command line.

Another way to return results from a function in a shell script is to use something not available to an external script—global variables.

## Local Variables

Consider a function that has some looping inside it, done by using a for loop, and that for loop uses an index variable i to count the iterations. Now consider what would happen if the function itself were called from within the body of a for loop, a loop that also happened to be controlled using i as an index variable. Since bash variables are global, then the (inner) function will affect the outer loop index. Not good. What to do?

Bash provides a syntax to let you declare variables as “local” so that they are not seen outside the function. Variables like i in a for loop should be declared as local variables:

```
function Summer {
    local i
    SUM=0
    for((i=0; i<$1; i++)) {
        let SUM+=i;
    }
}
```

Problem solved. It might be even better to use local -i i or declare -i i to declare i as an integer value, avoiding conversions to/from string. The declare inside a function acts just like the local in hiding any global variable by that name. The use of the -i option (in either case) allows you to specify the integer aspect of the variable.

The variable SUM in this example is not declared local because that is how the result of the function call is being returned to the caller. Should all variables in a function be declared local unless they are needed outside the script? Quite possibly, but since there is no way to enforce it, few programmers are that thorough. If you are writing functions that you expect to be shared and portable, then it is well worth the effort and will avoid unexpected side effects.





![](img/553ffa1bc15093729b691f706befc0ac_70_0.png)

## 动态作用域

如果你在一个函数中声明一个局部变量，然后该函数调用另一个函数，第二个函数会看到第一个函数的局部变量，而不会看到同名全局变量。如果你从脚本的主体部分调用第二个函数，它会看到并使用全局变量。欢迎来到动态作用域的危险之处，这也是保持脚本简洁且文档完善的另一个原因。

## 函数特殊情况

请记住，函数定义必须在其被调用之前出现。

如果你在函数定义中使用括号（与我们推荐的风格不同），那么你可以用任何复合语句语法选项来包含函数体。你可以使用双括号进行算术求值，或使用双方括号如果你的函数只是想做条件表达式求值。但这些用法极不可能出现。

你可以在函数定义上放置一个 I/O 重定向。它在调用该函数时生效。以下是该功能的一个常见用法，可轻松将整个使用信息重定向到 STDERR（通过 1>&2）：

```
function Usage_Message {
    echo "usage: $0 value pathname"
    echo "where value must be positive"
    echo "and pathname must be an existing file"
    echo "for example: $0 25 /tmp/scratch.csv"
} 1>&2
```

当脚本检查发现未提供正确数量的参数或提供的文件名不存在时，可能会调用此函数（我们将在第 79 页的“HELP!”中更详细地讨论这个问题）。需要注意的重要一点是，输出被重定向到 STDERR，但重定向不必放在脚本的每一行上——只需一次，放在包围的大括号之外即可。这可以为你节省大量输入，并且在修改脚本时无需记住为任何行添加额外的重定向。

请注意，为了清晰起见并逐步引入，我们分解了一些关于函数的风格建议；有关详细信息，请参阅第 128 页的“Functions”。

## 是时候使用 printf 了

如果你从 C 和 Java 等语言中了解 printf，那么你就有了先发优势。我们认为在这一章关于函数的章节中介绍它是有意义的，因为虽然它在 bash 中不是函数而是内置命令，但感觉足够熟悉，可以像对待函数一样对待它。

*Print formatted*，或 `printf`，在 bash 中被低估了，因为大多数人只是使用 echo，但 bash 内置版本有一些惯用的扩展，你需要能够阅读和编写。它也在 POSIX 中定义，与 bash 内置版本的 echo 不同，因此如果你的脚本在非 Linux 系统上运行，它的可移植性更强。

我们只会讨论 bash 内置版本，但你可能有一个与 bash 无关的外部二进制文件，并且选项不会相同：

```
$ type -a printf
printf is a shell builtin
printf is /usr/bin/printf
```

除非你使用完整路径（/usr/bin/printf）或在前面加上 env，否则会使用 bash `printf`。比较 `printf --help` 和 `env printf --help` 以查看差异。

我们也不会列出标准 `printf` 格式的长列表；这些在许多其他地方都有介绍，包括 *bash Cookbook* 的附录 A。

有关你的 printf 版本的更多详细信息，请参阅：

- `help printf` 或 `printf --help`
- `man 1 printf`
- 可能 `/usr/bin/printf --help` 或 `env printf --help`

## POSIX 输出

如果你的脚本将在 Linux 以外的操作系统上运行，你应考虑使用 `printf` 而不是 echo 以保持一致性。它很简单，但比使用 echo 需要更多的输入，而且与 echo 不同，不会自动包含尾随换行符：

```
printf '%s\n'   # 简单字符串，换行符不会自动包含
printf '%b\n'   # 还会展开转义字符
```

你可以在 `printf` 格式周围使用单引号或双引号，这些引号将遵循通常的变量插值规则（单引号不会插值；双引号会），但像 \n 这样的转义在两种类型的引号内都有效。如我们在其他地方所讨论的，我们更喜欢单引号来显示程序员的意图并防止插值，除非你需要在格式中插值变量，这可能表明你做错了什么。

有关此主题的更多信息，请参阅：

- https://unix.stackexchange.com/a/65819
- https://www.in-ulm.de/~mascheck/various/echo+printf

## 获取或使用日期和时间

Bash 4.2 添加了 printf %(datefmt)T，但默认输出是 Unix 纪元（1970-01-01 00:00:00 -0000），这可能不是你想要的。默认在 bash 4.3 中改为“now”，这更有意义。有两个特殊参数，-1，表示“现在”，和“-2”，表示“shell 被调用的时间”。你可以提供一个可选的纪元秒时间整数来显示其他时间，这对于将纪元秒转换为人类可读时间非常方便。我们建议如果你指的是“现在”，请使用 -1 以保持一致性和意图清晰：

```
### 使用 -v 设置 $today 变量
$ printf -v today '%(%F)T' '-1'
$ echo $today
2021-08-13
```

```
### 简单日志记录（两者相同）
$ printf '%(%Y-%m-%d %H:%M:%S %z)T: %s\n' '-1' 'Your log message here'
$ printf '%(%F %T %z)T: %s\n' '-1' 'Your log message here'
2021-08-13 12:48:33 -0400: Your log message here
```

```
### 1628873101 是什么时候？
$ printf '%(%F %T)T = %s\n' '1628873101' 'Epoch 1628873101 to human readable'
2021-08-13 12:45:01 = Epoch 1628873101 to human readable
```

![](img/553ffa1bc15093729b691f706befc0ac_72_0.png)

## 将 printf 输出到变量

如果你查看了 help printf，你可能会注意到 -v var 选项，它将输出分配给变量而不是显示它，类似于 C 中的 sprintf。我们接下来会看到它的用途。

有关此主题的更多详细信息，请参阅第 111 页的“Using logger from bash”。

我们更喜欢 bash 内置的 printf，但有一件事 GNU¹ 版本的 date 仍然比 printf %(datefmt)T 做得更好，那就是显示其他时间的日子，比如 date -d '2 months ago' '+%B'，它告诉我们两个月前的月份名称，例如：

```
$ date -d '2 months ago' '+%B'
August
```

![](img/553ffa1bc15093729b691f706befc0ac_72_1.png)

## Mac 上的默认 bash 太旧了

请注意，printf %(datefmt)T 至少需要 bash 4.2，推荐 4.3+，因此这在 Mac 上较旧但非常有用的默认 bash 3.2 上不起作用！请参阅第 ix 页的“bash on Mac”。

1 GNU 是一个递归缩写，代表“GNU’s Not Unix。”

## printf 用于复用或调试

Bash help 说“%q 以可重新用作 shell 输入的方式引用参数”，我们将在第 7 章中使用它来显示“shell 引用的”字符串，但这里有一个简单示例：

```
$ printf '%q' "This example is from $today\n"
This\ example\ is\ from\ 2021-08-13\\n
```

这对于在其他地方复用输出、创建格式化输出（另请参阅第 38 页的“Comma-Separated Lists”）以及在需要能够看到隐藏或控制字符或字段的调试中很有用。另请参阅第 120 页的“bash Debugging”。

#### 风格和可读性：回顾

bash 中的函数非常类似于内部 shell 脚本。它们的调用方式非常类似于运行任何命令，它们的参数引用方式也与任何 shell 脚本的参数相同（$1、$2 等）。你需要在脚本前面放置函数，以便在对函数进行任何调用之前看到其定义。如同任何语言一样，函数应保持简短且主要为单一目的。对函数使用单个 I/O 重定向可以帮助你避免重复的重定向。也许函数最大的危险是变量引用。函数可以通过变量或通过写入 STDOUT 并让调用者将函数的输出管道传输到下一个命令来返回值。你可能需要引用（全局）变量来从函数返回值，但有一些变量你希望避免更改。使用 local 可以帮助解决这个问题，但要警惕动态类型——那些局部变量在你调用另一个函数时不会保持局部。务必在函数的开头注释中记录此类内容。

printf 内置命令的行为与其它语言中熟悉的函数非常相似。除了标准格式外，bash 的 printf 还有一些有用的惯用扩展。我们特别欣赏不必创建子 shell 来运行 date 命令只是为了知道现在几点钟，尤其是在你频繁调用它进行日志记录时。




