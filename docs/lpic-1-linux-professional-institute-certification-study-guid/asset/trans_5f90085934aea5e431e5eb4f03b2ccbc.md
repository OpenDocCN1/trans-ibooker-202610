
read 命令等待输入的秒数。当计时器到期时，read 命令将返回非零退出状态：

```
$ cat test9.sh
#!/bin/bash
# timing the data entry

if read -t 5 -p "Please enter your name: " name
then
    echo "Hello $name, welcome to my script"
else
    echo
    echo "Sorry, too slow!"
fi
$ ./test9.sh
Please enter your name: Rich
Hello Rich, welcome to my script
$ ./test9.sh
Please enter your name:
Sorry, too slow!
$
```

由于如果计时器到期 read 命令将以非零退出状态退出，因此可以轻松使用标准结构化语句（例如 if 语句或 while 循环）来跟踪发生的情况。在此示例中，当计时器到期时，if 语句失败，shell 将执行 else 部分中的命令。

除了对输入计时之外，您还可以将 read 命令设置为计算输入字符数。当输入了预设数量的字符后，它会自动退出，并将输入的数据分配给变量。如清单 9.13 所示。

## 清单 9.13：计算输入字符数

```
$ cat test10.sh
#!/bin/bash
# getting just one character of input

read -n1 -p "Do you want to continue [Y/N]? " answer
case $answer in
Y | y) echo
    echo "fine, continue on...";;
N | n) echo
    echo OK, goodbye
    exit;;
esac
echo "This is the end of the script"
$ ./test10.sh
Do you want to continue [Y/N]? Y
fine, continue on...
This is the end of the script
$ ./test10.sh
Do you want to continue [Y/N]? n
OK, goodbye
$
```

在清单 9.13 中，read 命令将 -n 选项的值设置为 1，指示其在接受单个字符后即退出。一旦您按下单个字符来回答，read 命令就会接受输入并将其传递给变量。无需按 Enter 键。

## 静默读取

有时您需要脚本用户的输入，但又不希望该输入显示在监视器上。典型的例子是输入密码时，但还有许多其他类型的数据需要隐藏。

-s 选项可防止 read 命令中输入的数据在监视器上显示（实际上，数据仍会显示，但 read 命令将文本颜色设置为与背景颜色相同）。以下是在脚本中使用 -s 选项的示例：

```
$ cat test11.sh
#!/bin/bash
# hiding input data from the monitor

read -s -p "Enter your password: " pass
echo
echo "Is your password really $pass?"
$ ./test11.sh
Enter your password:
Is your password really T3sting?
$
```

在输入提示符处键入的数据不会显示在监视器上，但会正常分配给变量以供脚本使用。

## 退出状态

当 shell 脚本结束时，它会将退出状态返回给启动它的父 shell。退出状态告诉我们 shell 脚本是否成功完成。

Linux 为我们提供了特殊的 $? 变量，用于保存上一条执行命令的退出状态值。要检查命令的退出状态，必须在命令结束后立即查看 $? 变量。它会根据 shell 执行的上一条命令的退出状态而更改值：

```
$ who
rich        :0           2019-02-20 23:16 (:0)
$ echo $?
0
$
```

按照惯例，成功完成的命令的退出状态为 0。如果命令在出错时完成，则退出状态将显示为正整数值。

您可以使用 exit 命令更改 shell 脚本的退出状态。只需在 exit 命令中指定所需的退出状态值即可：

```
$ /bin/bash
$ exit 120
exit
$ echo $?
120
$
```

在此示例中，我们使用 /bin/bash 命令启动了一个新的子 shell，然后使用 exit 命令以退出状态代码 120 退出子 shell。回到父 shell 中，我们显示了 $? 变量的值，以查看其是否与 exit 命令中设置的值匹配。在编写更复杂的脚本时，可以通过更改退出状态值来指示错误。这样，通过检查退出状态，您可以轻松调试 shell 脚本。

## 编写脚本程序

迄今为止，我们已探讨如何在 shell 脚本中组合常规命令行命令，以自动执行系统管理员可能执行的常见任务。但 shell 脚本允许我们做更多事情。Bash shell 提供了更多类似编程的命令，允许我们在 shell 脚本中编写功能齐全的程序，例如捕获命令输出、执行数学运算、检查变量和文件条件以及循环处理命令。本节将介绍 Bash shell 提供给我们的一些高级编程功能。

### 命令替换

shell 脚本最有用的功能之一很可能是存储和处理数据的能力。迄今为止，我们已讨论如何使用输出重定向将命令的输出存储到文件，以及使用管道将命令的输出重定向到另一个命令。然而，还有一种技术可以让您在脚本中存储和使用数据时更具灵活性。

*命令替换*允许您将命令的输出分配给 shell 脚本中的用户变量。输出存储在变量后，您可以使用标准 Linux 字符串操作命令（例如 sort 或 grep）在显示数据之前对其进行处理。

要将命令的输出重定向到变量，需要使用以下两种命令替换格式之一：

- 将反引号 (`) 放在命令周围
- 在 $() 函数中使用命令

两种方法产生相同的结果：将命令的输出重定向到用户变量。清单 9.14 演示了使用这两种方法。

## 清单 9.14：演示命令替换

```
$ var1=`date`
$ echo $var1
Fri Feb 21 18:05:38 EST 2019
$ var2=$(who)
$ echo $var2
rich :0 2019-02-21 17:56 (:0)
$
```

命令替换的输出存储在相应的变量中。然后，您可以在脚本程序中的任何位置将这些变量用作标准字符串值。

反引号字符与单引号不同。它通常是美国键盘上与波浪号字符 (~) 位于同一键上的字符。由于反引号与单引号之间容易混淆，在 Linux 世界中使用 $() 函数格式变得更加流行。

## 执行数学运算

最终，您会希望在 shell 脚本中做的不仅仅是操作文本字符串。世界围绕数字运转，到了某个时刻，您可能需要对数据进行一些数学运算。不幸的是，Bash shell 在此方面显得有些老旧。Bash shell 中的数学功能不如较新 shell（例如 Z shell）中的功能先进。然而，在 Bash shell 脚本中使用简单数学函数有几种方法。

要在 shell 脚本中包含数学表达式，请使用一种特殊格式。此格式将方程放在括号内：

```
result=$[ 25 * 5 ]
```

使用此方法可以对数据执行多种不同的数学运算，但存在限制。$[] 格式仅允许使用整数；它不支持浮点值。

如果需要进行浮点计算，在 Bash shell 中事情会复杂得多。一种解决方案是使用 bc 命令行计算器程序。bc 计算器是 Linux 中的一个工具，可以执行浮点运算：

```
$ bc
bc 1.07.1
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006, 2008, 2012-2017 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type 'warranty'.
12 * 5.4
64.8
3.156 * (3 + 5)
25.248
quit
$
```

不幸的是，bc 计算器也有其自身的局限性。浮点运算由名为 scale 的内置变量控制。必须将此变量设置为所需的小数位数，否则无法获得想要的结果：

```
$ bc -q
3.44 / 5
0
scale=4
3.44 / 5
.6880
quit
$
```

要将 bc 计算嵌入到脚本中，事情会变得有点复杂。必须使用命令替换将计算输出捕获到变量中，但这里有个诀窍。需要使用的基本格式是：

```
variable=$(echo "options; expression" | bc)
```

第一个参数 options 允许我们设置 bc 变量，例如 scale 变量。expression 参数定义要使用 bc 计算的数学表达式。尽管这看起来很奇怪，但它有效：

```
$ var1=$(echo "scale=4; 3.44 / 5" | bc)
$ echo $var1
.6880
$
```
