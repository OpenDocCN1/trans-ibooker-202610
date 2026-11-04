## 6 了解 PHP 8 功能差异

在本章中，你将了解 PHP 8 命令或功能级别上的潜在向后兼容性中断。

本章提供了重要信息，突出了迁移现有代码到 PHP 8 时的潜在陷阱。

本章提供的信息至关重要，以便你能够生成可靠的 PHP 代码。

在学习完本章的概念后，你将更能写出产生精确结果并避免不一致的代码。

本章涵盖的主题包括以下内容：

- 学习关键的高级字符串处理差异
- 了解 PHP 8 字符串到数字比较的改进
- 处理算术、按位和连接操作的差异
- 利用区域设置独立性
- 在 PHP 8 中处理数组
- 精通安全函数和设置的更改

## 技术要求

为了检查和运行本章提供的代码示例，推荐的最低硬件配置如下：

- 基于 x86_64 的台式电脑或笔记本电脑
- 1 GB 的可用磁盘空间
- 4 GB RAM
- 500 千比特每秒（Kbps）或更快的互联网连接

此外，你还需要安装以下软件：

- Docker
- Docker Compose

请参阅 *第1章，介绍新的 PHP 8 OOP 特性* 中的 *技术要求* 部分，了解有关 Docker 和 Docker Compose 安装的更多信息，以及如何构建本书记载的代码演示所使用的 Docker 容器。在本书中，我们将恢复本书示例代码的目录称为 `/repo`。

本章的源代码位于此处：
```
https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices.
```

我们现在可以开始讨论，通过检查 PHP 8 中引入的字符串处理差异来开始。

## 学习关键的高级字符串处理差异

总体而言，PHP 8 中的字符串函数已被收紧并标准化。

你会发现 PHP 8 中的使用受到更严格的限制，这最终迫使你生成更好的代码。

我们可以说 PHP 8 中字符串函数参数的性质和顺序更加统一，这就是为什么我们说 PHP 核心团队已规范化使用的原因。

这些改进在处理数字字符串时尤其明显。

PHP 8 字符串处理中的其他更改涉及参数的细微更改。

在本节中，我们向你介绍 PHP 8 处理字符串的关键变化。

了解 PHP 8 引入的处理改进固然重要，但也需要了解 PHP 8 之前字符串处理的缺陷。



让我们先看一下 PHP 8 中用于搜索嵌入字符串的函数在字符串处理方面的一个特性。

## 对 needle 参数变更的处理

多个 PHP 字符串函数会搜索较大字符串中子字符串的存在。这些函数包括 strpos()、strrpos()、stripos()、strripos()、strstr()、strchr()、strrchr() 和 stristr()。所有这些函数都有两个共同的参数：needle 和 haystack。

## 区分 needle 和 haystack

为了说明 needle 和 haystack 之间的区别，请看 strpos() 的函数签名：

```
strpos(string $haystack,string $needle,int $pos=0):
int|false
```

$haystack 是搜索的目标。$needle 是要查找的子字符串。strpos() 函数返回子字符串在搜索目标中的位置。如果未找到子字符串，则返回布尔值 FALSE。其他 str*() 函数会产生不同类型的输出，这里不详细说明。

PHP 8 处理 needle 参数的两种关键变化可能会破坏迁移到 PHP 8 的应用程序。这些变化适用于 needle 参数不是字符串或 needle 参数为空的情况。让我们先来看非字符串 needle 参数的处理。

## 处理非字符串 needle 参数

你的 PHP 应用程序可能没有采取适当的预防措施来确保此处提到的 str*() 函数的 needle 参数始终是字符串。如果是这种情况，在 PHP 8 中，needle 参数现在将始终被解释为字符串而不是 ASCII 码点。

如果需要提供 ASCII 值，必须使用 chr() 函数将其转换为字符串。在下面的示例中，使用了 LF ("\n") 的 ASCII 值而不是字符串。在 PHP 7 或更早版本中，strpos() 在执行搜索前会进行内部转换。在 PHP 8 中，该数字只是简单地强制转换为字符串，导致出现意外的结果。

这里有一个代码示例，用于搜索字符串中 LF 的存在。然而，请注意，提供的参数不是字符串，而是一个值为 10 的整数：

```php
// /repo/ch06/php8_num_str_needle.php
function search($needle, $haystack) {
    $found = (strpos($haystack, $needle))
        ? 'contains' : 'DOES NOT contain';
    return "This string $found LF characters\n";
}
$haystack = "We're looking\nFor linefeeds\nIn this
    string\n";
$needle = 10;        // ASCII code for LF
echo search($needle, $haystack);
```

以下是在 PHP 7 中运行代码示例的结果：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_num_str_needle.php
This string contains LF characters
```

而以下是在 PHP 8 中运行同一代码块的结果：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_num_str_needle.php
This string DOES NOT contain LF characters
```

如你所见，将 PHP 7 的输出与 PHP 8 的输出进行比较，同一代码块会产生截然不同的结果。这是极难发现的潜在代码破坏，因为不会生成任何警告或错误。

最佳实践是对任何包含 PHP str*() 函数的函数或方法中的 needle 参数应用字符串类型提示。如果我们重写之前的示例，输出在 PHP 7 和 PHP 8 中都是一致的。以下是使用类型提示重写的相同示例：

```php
// /repo/ch06/php8_num_str_needle_type_hint.php
declare(strict_types=1);
function search(string $needle, string $haystack) {
    $found = (strpos($haystack, $needle))
        ? 'contains' : 'DOES NOT contain';
    return "This string $found LF characters\n";
}
$haystack = "We're looking\nFor linefeeds\nIn this
    string\n";
$needle  = 10;    // ASCII code for LF
echo search($needle, $haystack);
```

现在，在任一版本的 PHP 中，输出如下：

```
PHP Fatal error: Uncaught TypeError: search(): Argument #1 ($needle) must be of type string, int given, called in /repo/ch06/php8_num_str_needle_type_hint.php on line 14 and defined in /repo/ch06/php8_num_str_needle_type_hint.php:4
```

通过声明 strict_types=1，并为 $needle 参数添加 string 类型提示，任何误用你代码的开发人员都会收到明确的指示，表明这种做法是不可接受的。

现在让我们看看在 PHP 8 中当 needle 参数缺失时会发生什么。

## 处理空 needle 参数

str*() 函数的另一个重大变化是 needle 参数现在可以为空（例如，任何使 empty() 函数返回 TRUE 的值）。这带来了显著的向后兼容性破坏的潜在风险。在 PHP 7 中，如果 needle 参数为空，strpos() 的返回值将是布尔值 FALSE，而在 PHP 8 中，空值首先被转换为字符串，从而产生完全不同的结果。

如果计划将 PHP 版本更新到 8，了解这种潜在的代码破坏极其重要。空的 needle 参数在手动审查代码时很难发现。这是一个需要一套完善的单元测试来确保 PHP 迁移顺利进行的情况。

为了说明潜在问题，请考虑以下示例。假设 needle 参数为空。在这种情况下，传统的 if () 检查 strpos() 结果是否不与 FALSE 相同，会在 PHP 7 和 8 之间产生不同的结果。以下是代码示例：

- 首先，我们定义一个使用 strpos() 报告 needle 值是否在 haystack 中找到的函数。请注意对布尔值 FALSE 的严格类型检查：

```php
// php7_num_str_empty_needle.php
function test($haystack, $search) {
    $pattern = '%15s | %15s | %10s' . "\n";
    $result  = (strpos($haystack, $search) !== FALSE)
        ? 'FOUND' : 'NOT FOUND';
    return sprintf($pattern,
        var_export($search, TRUE),
        var_export(strpos($haystack, $search),
            TRUE),
        $result);
};
```

- 然后我们将 haystack 定义为包含字母和数字的字符串。needle 参数以一组都被视为空的值数组形式提供：

```php
$haystack = 'Something Anything 0123456789';
$needles = ['', NULL, FALSE, 0];
foreach ($needles as $search)
    echo test($haystack, $search);
```

PHP 7 中的输出如下所示：

```
root@php8_tips_php7 [/repo/ch06 ]#
php php7_num_str_empty_needle.php
PHP Warning:  strpos(): Empty needle in /repo/ch06/php7_num_str_empty_needle.php on line 5
// not all Warnings are shown ...
'' | false | NOT FOUND
NULL | false | NOT FOUND
false | false | NOT FOUND
0 | false | NOT FOUND
```

在一系列警告之后，最终输出出现。如输出所示，strpos ($haystack, $search) 的返回值在 PHP 7 中始终是布尔值 FALSE。

然而，在 PHP 8 中运行同一代码的输出截然不同。以下是 PHP 8 的输出：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php7_num_str_empty_needle.php
'' | 0 | FOUND
NULL | 0 | FOUND
false | 0 | FOUND
0 | 19 | FOUND
```

在 PHP 8 中，空的 needle 参数首先被静默转换为字符串。没有任何 needle 值返回布尔值 FALSE。这导致函数报告 needle 已找到。这显然不是期望的结果。然而，对于数字 0 来说，它包含在 haystack 中，导致返回值为 19。

让我们看看如何解决这个问题。

## 使用 str_contains() 解决问题

前一节所示代码块的意图是确定 haystack 是否包含 needle。strpos() 并不是完成此任务的正确工具！请看改用 str_contains() 的相同函数：

```php
// /repo/ch06/php8_num_str_empty_needle.php
function test ($haystack, $search) {
    $pattern = '%15s | %15s | %10s' . "\n";
    $result  = (str_contains($search, $haystack) !==
                FALSE)
                ? 'FOUND'  : 'NOT FOUND';
    return sprintf($pattern,
        var_export($search, TRUE),
        var_export(str_contains($search, $haystack),
            TRUE),
        $result);
};
```

然后我们在 PHP 8 中运行修改后的代码，得到的结果与 PHP 7 中收到结果相似：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_num_str_empty_needle.php
'' | false | NOT FOUND
NULL | false | NOT FOUND
false | false | NOT FOUND
0 | false | NOT FOUND
```

你可能会问为什么数字 0 在字符串中找不到？答案是 str_contains() 执行了更严格的搜索。整数 0 与字符串 "0" 不同！现在让我们来看一下 v*printf() 系列；PHP 8 中对参数施加更严格控制的另一个字符串函数系列。

## 处理 v*printf() 的变更

v*printf() 函数族是 printf() 函数族的一个子集，其中包括 vprintf()、vfprintf() 和 vsprintf()。该子集与主族之间的区别在于，v*printf() 函数设计为接受数组作为参数，而不是无限制的一系列参数。以下是一个简单的示例来说明这种差异：

- 1. 首先，我们定义一组将被插入到模式 $patt 中的参数：
- 2. 然后我们使用一系列参数执行 printf() 语句：

// /repo/ch06/php8_printf_vs_vprintf.php
$ord = 'third';
$day = 'Thursday';
$pos = 'next';
$date = new DateTime("$ord $day of $pos month");
$patt = "The %s %s of %s month is: %s\n";
```

```
printf($patt, $ord, $day, $pos,
    $date->format('l, d M Y'));
```

3. 然后我们将参数定义为数组 $arr，并使用 vprintf() 生成相同的结果：

```
$arr = [$ord, $day, $pos, $date->format('l, d M Y')];
vprintf($patt, $arr);
```

以下是程序在 PHP 8 中运行的输出。在 PHP 7 中运行的输出相同（未显示）：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_printf_vs_vprintf.php
The third Thursday of next month is: Thursday, 15 Apr 2021
The third Thursday of next month is: Thursday, 15 Apr 2021
```

如你所见，两个函数的输出完全相同。唯一的用法区别是 vprintf() 以数组形式接受参数。

PHP 的早期版本允许开发者对传递给 v*printf() 函数族的参数随意处理。在 PHP 8 中，参数的数据类型现在被严格执行。只有在代码控制不存在以确保传递数组的情况下才会出现问题。另一个更重要的区别是 PHP 7 会允许 v*printf() 使用 ArrayObject，而 PHP 8 则不会。

在这里显示的示例中，PHP 7 会发出警告，而 PHP 8 则抛出错误：

1. 首先，我们定义模式和源数组：

// /repo/ch06/php7_vprintf_bc_break.php
$patt = "\t%s. %s. %s. %s. %s.";
$arr = ['Person', 'Woman', 'Man', 'Camera', 'TV'];
```

2. 然后我们定义一个测试数据数组，以测试 vsprintf() 接受哪些参数：

```
$args = [
    'Array' => $arr,
    'Int'   => 999,
    'Bool'  => TRUE,
    'Obj'   => new ArrayObject($arr)
];
```

3. 然后我们定义一个 foreach() 循环遍历测试数据并调用 vsprintf()：

```
foreach ($args as $key => $value) {
    try {
        echo $key . ': ' . vsprintf($patt, $value);
    } catch (Throwable $t) {
        echo $key . ': ' . get_class($t)
            . ': ' . $t->getMessage();
    }
}
```

以下是 PHP 7 中运行的输出：

```
root@php8_tips_php7 [/repo/ch06 ]#
php php7_vprintf_bc_break.php
Array:      Person. Woman. Man. Camera. TV.
PHP Warning:  vsprintf(): Too few arguments in /repo/ch06/php8_vprintf_bc_break.php on line 14
Int:
PHP Warning:  vsprintf(): Too few arguments in /repo/ch06/php8_vprintf_bc_break.php on line 14
Bool:
Obj:        Person. Woman. Man. Camera. TV.
```

从输出可以看出，在 PHP 7 中，数组和 ArrayObject 参数都被接受。以下是同一代码示例在 PHP 8 中运行的情况：

```
root@php8_tips_php8 [/repo/ch06 ]#
php php7_vprintf_bc_break.php
Array:      Person. Woman. Man. Camera. TV.
Int: TypeError:vsprintf(): Argument #2 ($values) must be of type array, int given
Bool: TypeError:vsprintf(): Argument #2 ($values) must be of type array, bool given
Obj: TypeError:vsprintf(): Argument #2 ($values) must be of type array, ArrayObject given
```

如预期，PHP 8 的输出更加一致。在 PHP 8 中，v*printf() 函数被严格限定为仅接受数组作为参数。不幸的是，你可能使用过 ArrayObject 的可能性很大。这很容易解决，只需在 ArrayObject 实例上使用 getArrayCopy() 方法即可，该方法返回一个数组。

以下是同时适用于 PHP 7 和 PHP 8 的重写代码：

```php
if ($value instanceof ArrayObject)
    $value = $value->getArrayCopy();
echo $key . ':' . vsprintf($patt, $value);
```

现在你已经知道在使用 v*printf() 函数时潜在代码中断的查找位置，让我们把注意力转向 PHP 8 中带空长度参数的字符串函数的工作方式差异。

## 在 PHP 8 中处理空长度参数

在 PHP 7 及更早版本中，NULL 长度参数会导致返回空字符串。在 PHP 8 中，NULL 长度参数现在被视为与省略长度参数相同。受影响的函数包括以下：

- substr()
- substr_count()
- substr_compare()
- iconv_substr()

在下一个示例中，PHP 7 返回空字符串，而 PHP 8 返回字符串的剩余部分。如果操作结果用于确认或否定子字符串的存在，则存在很高的代码中断风险：

1. 首先，我们定义一个干草堆和针。然后我们运行 strpos() 来获取针在干草堆中的位置：

```php
// /repo/ch06/php8_null_length_arg.php
$str = 'The quick brown fox jumped over the fence';
$var = 'fox';
$pos = strpos($str, $var);
```

2. 接下来，我们提取子字符串，故意将长度参数留为空：

```
$res = substr($str, $pos, $len);
$find = ($res) ? '' : ' NOT';
echo "$var is$find found in the string\n";
```

以下是 PHP 7 中运行的输出：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_null_length_arg.php
PHP Notice: Undefined variable: len in /repo/ch06/php8_null_length_arg.php on line 8
Result  : fox is NOT found in the string
Remainder:
```

如预期，PHP 7 发出 Notice。然而，由于 NULL 长度参数返回空字符串，搜索结果是不正确的。以下是同一代码在 PHP 8 中运行的情况：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_null_length_arg.php
PHP Warning: Undefined variable $len in /repo/ch06/php8_null_length_arg.php on line 8
Result  : fox is found in the string
Remainder: fox jumped over the fence
```

PHP 8 发出 Warning 并返回字符串的剩余部分。这与完全省略长度参数的行为一致。如果你的代码依赖于返回空字符串，则在 PHP 8 更新后存在潜在的代码中断。

现在让我们来看一下 PHP 8 在 implode() 函数中使字符串处理更加统一的另一个情况。

## 检查 implode() 的变更

两个广泛使用的 PHP 函数执行数组到字符串的转换及其反向操作：explode() 将字符串转换为数组，而 implode() 将数组转换为字符串。然而，implode() 函数隐藏着一个深深的秘密：它的两个参数可以以任意顺序表达！

请记住，当 PHP 于 1994 年首次推出时，初始目标是尽可能易于使用。这种方法取得了成功，以至于根据 w3techs 进行的最近一次服务器端编程语言调查显示，PHP 已成为今天超过 78% 的所有 Web 服务器的首选语言。（https://w3techs.com/technologies/overview/programming_language）

然而，出于一致性考虑，将 implode() 函数的参数与其镜像孪生函数 explode() 对齐是有意义的。因此，传递给 implode() 的参数现在必须按以下顺序排列：

```
implode(<GLUE STRING>, <ARRAY>);
```

以下是以任一顺序调用 implode() 函数的参数的代码示例：

// /repo/ch06/php7_implode_args.php
$arr = ['Person', 'Woman', 'Man', 'Camera', 'TV'];
echo __LINE__ . ':' . implode(' ', $arr) . "\n";
echo __LINE__ . ':' . implode($arr, ' ') . "\n";
```

如 PHP 7 下面的输出所示，两个 echo 语句都产生结果：

```
root@php8_tips_php7 [ /repo/ch06 ]# php php7_implode_args.php
5:Person Woman Man Camera TV
6:Person Woman Man Camera TV
```

在 PHP 8 中，只有第一个语句成功，如下所示：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php7_implode_args.php
5:Person Woman Man Camera TV
PHP Fatal error: Uncaught TypeError: implode(): Argument #2 ($array) must be of type ?array, string given in /repo/ch06/php7_implode_args.php:6
```

很难发现 implode() 以错误顺序接收参数的地方。在 PHP 8 迁移之前最好的预警方式是记录使用 implode() 的所有 PHP 文件类别。另一个建议是利用 PHP 8 的命名参数功能（在第 1 章《介绍新的 PHP 8 OOP 功能》中介绍）。



## 在 PHP 8 中学习常量的使用

其中一个真正离谱的功能是 PHP 在 8 版本之前能够定义 *大小写不敏感* 的常量。在最初，PHP 首次引入时，许多开发者都在编写大量 PHP 代码，且明显缺乏任何形式的编码标准。当时的目标只是 *使其工作*。

随着普遍趋向于强制执行良好编码标准的趋势，这一功能在 PHP 7.3 中被弃用，并在 PHP 8 中被移除。如果您使用 define() 且第三个参数设置为 TRUE，则可能会出现向后兼容性中断。

这里显示的示例在 PHP 7 中有效，但在 PHP 8 中并非完全有效：

// /repo/ch06/php7_constants.php
define('THIS_WORKS', 'This works');
define('Mixed_Case', 'Mixed Case Works');
define('DOES_THIS_WORK', 'Does this work?', TRUE);
echo __LINE__ . ':' . THIS_WORKS . "\n";
echo __LINE__ . ':' . Mixed_Case . "\n";
echo __LINE__ . ':' . DOES_THIS_WORK . "\n";
echo __LINE__ . ':' . Does_This_Work . "\n";

在 PHP 7 中，所有代码行按原样工作。以下是输出：

```
root@php8_tips_php7 [ /repo/ch06 ]# php php7_constants.php
7:This works
8:Mixed Case Works
9:Does this work?
10:Does this work?
```

请注意，define() 的第三个参数在 PHP 7.3 中已被弃用。因此，如果您在 PHP 7.3 或 7.4 中运行此代码示例，输出与添加弃用通知的情况相同。

然而，在 PHP 8 中，产生了相当不同的结果，如下所示：

```
root@php8_tips_php8 [ /repo/ch06 ]# php php7_constants.php
PHP Warning: define(): Argument #3 ($case_insensitive) is ignored since declaration of case-insensitive constants is no longer supported in /repo/ch06/php7_constants.php on line 6
7:This works
8:Mixed Case Works
9:Does this work?
PHP Fatal error: Uncaught Error: Undefined constant "Does_This_Work" in /repo/ch06/php7_constants.php:10
```

正如您所期望的，第 7、8 和 9 行产生了预期的结果。然而，最后一条行会抛出致命错误，因为 PHP 8 中的常量现在是区分大小写的。此外，对于第三个 define() 语句会发出警告，因为在 PHP 8 中第三个参数被忽略。

您现在对 PHP 8 中引入的关键字符串处理差异有了大致了解。接下来我们将关注数字字符串与数字比较方式的变化。

## 理解 PHP 8 字符串到数字比较的改进

在 PHP 中比较两个数值从未成为问题。比较两个字符串也不是问题。问题出现在字符串和数值数据（硬编码数字，或包含 float 或 int 类型数据的变量）之间的非严格比较中。在这种情况下，如果执行非严格比较，PHP 总会将字符串转换为数值。

字符串到数值的转换只有在字符串仅包含数字（或数值如加号、减号或小数分隔符）时才 100% 成功。在本节中，您将学习如何防止涉及字符串和数值数据的不准确非严格比较。掌握本章介绍的概念至关重要，如果您希望编写具有一致且可预测行为的代码。

在深入了解字符串到数值比较的细节之前，我们首先需要了解什么是非严格比较。

## 学习严格比较和非严格比较

类型转换的概念是 PHP 语言的重要组成部分。这一功能从语言的第一天起就被构建在其中。类型转换涉及在执行操作之前进行内部数据类型转换。这种能力对语言的成功至关重要。

PHP 最初设计用于在 Web 环境中运行，需要一种处理作为 HTTP 数据包一部分传输的数据的方法。HTTP 头部和主体以文本形式传输，并由 PHP 作为存储在一组超全局变量中的字符串接收，包括 $_SERVER、$_GET、$_POST 等。因此，PHP 语言需要在涉及数字的操作中快速处理字符串值。这是类型转换过程的工作。

严格比较首先检查数据类型。如果数据类型匹配，则进行比较。调用严格比较的运算符包括 === 和 !== 等。某些函数具有强制严格数据类型的选项。一个例子是 in_array()。如果第三个参数设置为 TRUE，则会进行严格类型搜索。以下是 in_array() 的方法签名：

```
in_array(mixed $needle, array $haystack, bool $strict = false)
```

非严格比较是在比较之前不进行数据类型检查。执行非严格比较的运算符包括 ==、!=、< 和 > 等。值得注意的是，switch {} 语言结构在其 case 语句中执行非严格比较。如果对不同数据类型的操作数进行非严格比较，则会执行类型转换。

现在让我们详细了解一下数字字符串。

## 检查数字字符串

数字字符串是一个仅包含数字或数字字符的字符串，例如加号 (+) 、减号 (-) 和小数分隔符。

> **重要说明**
> 需要注意的是，PHP 8 内部使用句点字符 (.) 作为小数分隔符。如果您需要在不使用句点作为小数分隔符的区域设置中渲染数字（例如，在法国，使用逗号 (,) 作为小数分隔符），请使用 number_format() 函数（参见 https://www.php.net/number_format）。请参阅本章中 利用区域设置独立性 部分了解更多信息。

数字字符串也可以使用工程符号（也称为科学计数法）组成。格式不正确的数字字符串是包含除数字、加号、减号或小数分隔符以外的值的数字字符串。前导数字字符串以数字字符串开头，但后面跟随非数字字符。PHP 引擎将任何既非数字也非前导数字的字符串视为非数字。

在 PHP 的早期版本中，类型转换不一致地解析包含数字的字符串。在 PHP 8 中，只有数字字符串才能干净地转换为数字：不能存在前导或尾随空白或其他非数字字符。

例如，看看 PHP 7 和 8 如何处理此代码示例中的数字字符串：

// /repo/ch06/php8_num_str_handling.php
$test = [
    0 => '111',
    1 => '   111',
    2 => '111   ',
    3 => '111xyz'
];
$patt = "%d : %3d : '%-s'\n";
foreach ($test as $key => $val) {
    $num = 111 + $val;
    printf($patt, $key, $num, $val);
}

以下是 PHP 7 中运行的输出：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_num_str_handling.php
0 : 222 : '111'
1 : 222 : '   111'
PHP Notice:  A non well formed numeric value encountered in /repo/ch06/php8_num_str_handling.php on line 11
2 : 222 : '111   '
PHP Notice:  A non well formed numeric value encountered in /repo/ch06/php8_num_str_handling.php on line 11
3 : 222 : '111xyz'
```

从输出中可以看到，PHP 7 认为带有尾随空格的字符串是格式不正确的。然而，带有前导空格的字符串被认为格式正确，并且在不生成 Notice 的情况下通过。带有非空白字符的字符串仍会被处理，但会产生 Notice。

以下是 PHP 8 中运行的相同代码示例：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_num_str_handling.php
0 : 222 : '111'
1 : 222 : '  111'
2 : 222 : '111  '
PHP Warning:  A non-numeric value encountered in /repo/ch06/php8_num_str_handling.php on line 11
3 : 222 : '111xyz'
```

PHP 8 在这方面更加一致，即包含前导或尾随空格的数字字符串被同等对待，并且不会生成 Notice 或 Warning。然而，最后一个字符串在 PHP 7 中曾是 Notice，现在会生成 Warning。

> 提示
> 您可以在 PHP 文档中阅读有关数字字符串的信息：
> https://www.php.net/manual/en/language.types.numeric-strings.php
> 有关类型转换的更多信息，请查看以下网址：
> https://www.php.net/manual/en/language.types.type-juggling.php

现在您已经对什么是格式正确和格式不正确的数字字符串有了大致了解，让我们将注意力转向 PHP 8 中处理数字字符串时可能出现的更严重向后兼容性中断问题。

## 检测涉及数字字符串的向后兼容性中断

您必须了解在 PHP 8 升级后代码可能中断的地方。在本小节中，我们向您展示了一些极其微妙的差异，这些差异可能产生重大后果。

潜在的代码中断可能在任何时候使用格式不正确的数字字符串时出现：

- 使用 is_numeric()
- 在字符串偏移量中（例如，$str['4x'])

- 使用按位运算符
- 在递增或递减变量的值为非规范数字字符串时

以下是修复代码的一些建议：

- 考虑对可能包含前导或尾随空白的数字字符串使用 trim()（例如，嵌入在已发布表单数据中的数字字符串）。
- 如果你的代码依赖于以数字开头的字符串，请使用显式类型转换以确保数字被正确插值。
- 不要依赖空字符串（例如，$str = '' ) 来干净地转换为 0。

在下面的代码示例中，一个带有尾随空格的非规范字符串被赋给 $age：

```php
// /repo/ch06/php8_num_str_is_numeric.php
$age = '77  ';
echo (is_numeric($age))
    ? "Age must be a number\n"
    : "Age is $age\n";
```

当我们在 PHP 7 中运行此代码时，is_numeric() 返回 TRUE。以下是 PHP 7 的输出：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_num_str_is_numeric.php
Age is 77
```

另一方面，当我们在 PHP 8 中运行此代码时，is_numeric() 返回 FALSE，因为该字符串不被视为数字。以下是 PHP 8 的输出：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_num_str_is_numeric.php
Age must be a number
```

正如你所见，PHP 7 和 PHP 8 之间字符串处理的差异可能导致应用程序行为不同，并可能产生灾难性的结果。现在让我们来看一下涉及规范字符串的不一致结果。

## 处理不一致的字符串到数字比较结果

为了完成涉及字符串和数字数据的非严格比较，PHP 引擎首先执行类型 juggling 操作，在执行比较之前内部将字符串转换为数字。然而，即使是规范的数字字符串，也可能产生从人类角度来看会认为是不合逻辑的结果。

作为示例，请看一下这个代码示例：

- 1. 首先我们对变量 $zero，其值为零，和变量 $string，其值为 ABC 进行非严格比较：
- 2. 以下非严格比较使用 in_array() 在 $array 数组中查找值为零的元素：
- 3. 最后，我们对前导数字字符串 42abc88 和硬编码数字 42 进行非严格比较：

```
$zero = 0;
$string = 'ABC';
$result = ($zero == $string) ? 'is' : 'is not';
echo "The value $zero $result the same as $string\n"
```

```
$array = [1 => 'A', 2 => 'B', 3 => 'C'];
$result = (in_array($zero, $array))
    ? 'is in' : 'is not in';
echo "The value $zero $result\n"
    . var_export($array, TRUE)
```

```
$mixed = '42abc88';
$result = ($mixed == 42) ? 'is' : 'is not';
echo "\nThe value $mixed $result the same as 42\n";
```

在 PHP 7 中运行的结果违背人类理解！以下是 PHP 7 的结果：

```
root@php8_tips_php7 [/repo/ch06 ]#
php php7_compare_num_str.php
The value 0 is the same as ABC
The value 0 is in
array (1 => 'A', 2 => 'B', 3 => 'C')
The value 42abc88 is the same as 42
```

从人类角度来看，这些结果没有任何意义！从计算机角度来看，另一方面，它完全有意义。字符串 ABC 在转换为数字时最终得到的值为零。同样，当进行数组搜索时，每个数组元素仅具有字符串值，最终会被插值为空。

前导数字字符串的情况有点棘手。在 PHP 7 中，插值算法会将数字字符转换，直到遇到第一个非数字字符。一旦发生这种情况，插值就会停止。因此，字符串 42abc88 在比较时变为整数 42。现在让我们看看 PHP 8 如何处理字符串到数字的比较。

## 了解 PHP 8 中所做的比较更改

在 PHP 8 中，如果将字符串与数字进行比较，只有数字字符串才被视为有效比较对象。使用指数表示法的字符串也被视为有效比较对象，以及带有前导或尾随空白的数字字符串。必须极其注意的是，PHP 8 在转换字符串之前就会做出此判断。

请看一下在 PHP 8 中运行的同一代码示例的输出，该示例在前一小节（处理不一致的字符串到数字比较结果）中描述：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php7_compare_num_str.php
The value 0 is not the same as ABC
The value 0 is not in
array (1 => 'A', 2 => 'B', 3 => 'C')
The value 42abc88 is not the same as 42
```

因此，正如输出所示，在 PHP 8 升级后，你的应用行为发生变化的可能性非常大。作为 PHP 8 字符串处理的最后说明，让我们看看如何避免升级问题。

## 在 PHP 8 升级期间避免问题

你面临的主要问题是 PHP 8 处理涉及不同数据类型操作数的非严格比较的方式不同。如果一个操作数是 int 或 float，另一个操作数是 string，升级后你可能会遇到问题。如果字符串是有效的数字字符串，非严格比较将不会有任何问题地进行。

以下运算符受影响：<==>, ==, !=, >, >=, <, 和 <=。如果选项标志设置为默认值，以下函数受影响：

- `in_array()`
- `array_search()`
- `array_keys()`
- `sort()`
- `rsort()`
- `asort()`
- `arsort()`
- `array_multisort()`

> **Tip**
> 
> 有关 PHP 8 中改进的数字字符串处理的更多信息，请参阅以下链接：https://wiki.php.net/rfc/saner-numeric-strings。相关的 PHP 8 更改记录在这里：https://wiki.php.net/rfc/string_to_number_comparison。

最佳实践是通过为函数或方法提供类型提示来最小化 PHP 类型 juggling。你也可以在比较前强制数据类型。最后，考虑使用严格比较，尽管这在所有情况下可能并不合适。

现在你已经了解了如何在 PHP 8 中正确处理涉及数字字符串的比较，让我们现在来看一下 PHP 8 涉及算术、按位和连接操作的更改。

## 处理算术、按位和连接操作中的差异

算术、按位和连接操作是任何 PHP 应用的核心。在本节中，你将了解在 PHP 8 迁移后这些简单操作中可能出现的隐藏危险。你必须了解 PHP 8 所做的更改，以便避免应用中出现潜在的代码中断。因为这些操作如此普通，没有这些知识，你将很难发现迁移后的错误。

我们先来看看 PHP 如何在算术和按位操作中处理非标量数据类型。

## 处理算术和按位操作中的非标量数据类型

从历史上看，PHP 引擎在算术或按位操作中使用混合数据类型时非常 *宽容*。我们已经查看过涉及 *数字*、*前导数字* 和 *非数字* 字符串和数字的比较操作。正如你所学，当使用非严格比较时，PHP 会调用类型 juggling 在执行比较之前将字符串转换为数字。当 PHP 执行涉及数字和字符串的算术操作时也会发生类似的操作。

在 PHP 8 之前，**非标量数据类型**（`string`、`int`、`float` 或 `boolean` 以外的其他数据类型）在算术操作中是允许的。PHP 8 已遏制这种不良做法，不再允许 `array`、`resource` 或 `object` 类型的操作数。当非标量操作数在算术操作中使用时，PHP 8 会一致地抛出 `TypeError`。此一般更改的唯一例外是，你仍然可以执行所有操作数均为 `array` 类型的算术操作。

> **Tip**
> 有关算术和按位操作中重要更改的进一步信息，请查看这里：https://wiki.php.net/rfc/arithmetic_operator_type_checks。

这里有一个代码示例来说明 PHP 8 中算术运算符处理差异：

- 1. 首先，我们定义示例非标量数据以在算术操作中进行测试：
- 2. 然后我们尝试将整数 99 添加到资源、对象，并对数组执行取模操作：

```php
// /repo/ch06/php8_arith_non_scalar_ops.php
$fn = __DIR__ . '../sample_data/gettysburg.txt';
$fh = fopen($fn, 'r');
$obj = new class() { public $val = 99; };
$arr = [1,2,3];
```

```php
echo "Adding 99 to a resource\n";
try { var_dump($fh + 99); }
catch (Error $e) { echo $e . "\n"; }
```


```
echo "\nAdding 99 to an object\n";
try { var_dump($obj + 99); }
catch (Error $e) { echo $e . "\n"; }
echo "\nPerforming array % 99\n";
try { var_dump($arr % 99); }
catch (Error $e) { echo $e . "\n"; }
```

3. 最后，我们将两个数组相加：

```
echo "\nAdding two arrays\n";
try { var_dump($arr + [99]); }
catch (Error $e) { echo $e . "\n"; }
```

当我们运行代码示例时，注意 PHP 7 如何执行静默转换并允许操作继续进行：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_arith_non_scalar_ops.php
Adding 99 to a resource
/repo/ch06/php8_arith_non_scalar_ops.php:10:
int(104)
Adding 99 to an object
PHP Notice: Object of class class@anonymous could not be converted to int in /repo/ch06/php8_arith_non_scalar_ops.php on line 13
/repo/ch06/php8_arith_non_scalar_ops.php:13:
int(100)
Performing array % 99
/repo/ch06/php8_arith_non_scalar_ops.php:16:
int(1)
Adding two arrays
/repo/ch06/php8_arith_non_scalar_ops.php:19:
array(3) {
  [0] => int(1)
  [1] => int(2)
  [2] => int(3)
}
```

特别令人惊讶的是，我们可以对数组执行取模操作！在 PHP 7 中，给对象添加值时会生成一个 Notice。然而，PHP 将对象类型转换为整数，值为 1，从而使算术运算结果为 100。

在 PHP 8 中运行相同的代码示例，输出结果截然不同：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_arith_non_scalar_ops.php
Adding 99 to a resource
TypeError: Unsupported operand types: resource + int in /repo/ch06/php8_arith_non_scalar_ops.php:10
Adding 99 to an object
TypeError: Unsupported operand types: class@anonymous + int in /repo/ch06/php8_arith_non_scalar_ops.php:13
Performing array % 99
TypeError: Unsupported operand types: array % int in /repo/ch06/php8_arith_non_scalar_ops.php:16
Adding two arrays
array(3) {
  [0]=>  int(1)
  [1]=>  int(2)
  [2]=>  int(3)
}
```

从输出可以看出，除了添加两个数组之外，PHP 8 一直会抛出 TypeError。在两种输出中，你可能会注意到，当添加两个数组时，第二操作数会被忽略。如果目标是合并两个数组，则必须改用 array_merge()。

现在让我们把注意力转向 PHP 8 字符串处理中与运算符优先级相关的潜在重大变化。

## 检查优先级的变更

运算符优先级，也称为运算顺序或运算符优先级，是一种在 18 世纪末和 19 世纪初建立的数学概念。PHP 也采用了数学运算符优先级规则，并增加了一个独特之处：连接运算符。PHP 语言的创始人假设连接运算符与算术运算符具有相同的优先级。这一假设直到 PHP 8 的到来才从未受到质疑。

在 PHP 8 中，算术运算优先于连接。连接运算符的降级现在将其置于位移运算符（<< 和 >>）之下。在任何不使用括号明确定义混合算术和连接运算的地方，都存在潜在的向后兼容性破坏。

这种改变本身不会抛出 Error 或生成 Warnings 或 Notices，因此存在隐藏代码破坏的潜在风险。

> **提示**
> 有关此更改原因的更多信息，请参阅以下链接：
> https://wiki.php.net/rfc/concatenation_precedence

以下示例最清楚地显示了此更改的影响：

```
echo 'The sum of 2 + 2 is: ' . 2 + 2;
```

这是 PHP 7 中此简单语句的输出：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php -r "echo 'The sum of 2 + 2 is: ' . 2 + 2;"
PHP Warning: A non-numeric value encountered in Command line code on line 1
2
```

在 PHP 7 中，由于连接运算符与加法运算符具有相同的优先级，字符串 The sum of 2 + 2 is: 首先与整数值 2 连接。然后新字符串被类型转换为整数，生成一个 Warning。新字符串的值被评估为 0，然后加上整数 2，输出结果为 2。

然而在 PHP 8 中，加法首先执行，然后结果与初始字符串连接。以下是 PHP 8 中运行的结果：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php -r "echo 'The sum of 2 + 2 is: ' . 2 + 2;"
The sum of 2 + 2 is: 4
```

从输出可以看出，结果更符合人类的预期！

另一个示例应能进一步说明降级连接运算符可能带来的差异。请看这行代码：

```
echo '1' . '11' + 222;
```

这是 PHP 7 中运行的结果：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php -r "echo '1' . '11' + 222;"
333
```

PHP 7 首先执行连接，生成字符串 111。然后将其类型转换并加到整数 222，结果为最终整数值 333。以下是 PHP 8 中运行的结果：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php -r "echo '1' . '11' + 222;"
1233
```

在 PHP 8 中，第二字符串 11 被类型转换并加到整数 222，产生中间值 233。然后将其类型转换为字符串并在前面加上 1，最终得到字符串值 1233。

现在你已经了解了 PHP 8 中算术、按位和连接运算的更改，让我们来看一下 PHP 8 中引入的一个新趋势：区域设置独立性。

## 利用区域设置独立性

在 PHP 8 之前的版本中，多个字符串函数和操作与区域设置绑定。净效果是数字的内部存储方式取决于区域设置。这种做法引入了细微的不一致，很难检测。在阅读完本章介绍的内容后，你将更能检测 PHP 8 升级后潜在的应用代码更改，从而避免应用失败。

### 了解与区域设置依赖相关的问题

在早期 PHP 版本中，区域设置依赖的不幸副作用是，从浮点到字符串再转换回浮点时结果不一致。当浮点值连接到字符串时也会出现不一致。OpCache 执行的某些优化操作导致连接操作在设置区域设置之前发生，这又是可能产生不一致结果的一种方式。

在 PHP 8 中，易受影响的操作和函数现在是区域设置无关的。这意味着所有浮点值现在都使用句点作为小数分隔符存储。默认区域设置不再默认从环境继承。如果需要设置默认区域设置，现在必须显式调用 setlocale()。

### 查看受区域设置独立性影响的函数和操作

大多数 PHP 函数不受切换到区域设置独立性的影响，原因很简单，区域设置与该函数或扩展无关。此外，大多数 PHP 函数和扩展已经是区域设置无关的。示例包括 PDO 扩展，以及诸如 var_export() 和 json_encode() 等函数，以及 printf() 系列。

受区域设置独立性影响的函数和操作包括以下内容：

- `(string) $float`
- `strval($float)`
- `print_r($float)`
- `var_dump($float)`
- `debug_zval_dump($float)`
- `settype($float, "string")`
- `implode([$float])`
- `xmlrpc_encode($float)`

以下代码示例说明了由于区域设置独立性而产生的处理差异：

- 1. 首先，我们定义一个要测试的区域设置数组。所选区域设置使用不同的方式表示数字的小数部分：
- 2. 然后我们遍历区域设置，设置区域设置，并执行从浮点到字符串再到浮点的转换，在每个步骤中回显结果：

// /repo/ch06/php8_locale_independent.php
$list = ['en_GB', 'fr_FR', 'de_DE'];
$patt = "%15s | %15s \n";
```

```
foreach ($list as $locale) {
    setlocale(LC_ALL, $locale);
    echo "Locale          : $locale\n";
    $f = 123456.789;
    echo "Original        : $f\n";
    $s = (string) $f;
    echo "Float to String : $s\n";
    $r = (float) $s;
    echo "String to Float : $r\n";
}
```

如果我们在 PHP 7 中运行此示例，请注意结果：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_locale_independent.php
Locale          : en_GB
Original        : 123456.789
Float to String : 123456.789
String to Float : 123456.789
Locale          : fr_FR
Original        : 123456,789
Float to String : 123456,789
String to Float : 123456
Locale          : de_DE
Original        : 123456,789
Float to String : 123456,789
String to Float : 123456
```



如您从输出中可以看到，数字在内部使用句点作为 en_GB 的小数分隔符，而逗号则用于 fr_FR 和 de_DE 区域设置。然而，当字符串转换回数字时，如果小数分隔符不是句点，则字符串会被视为前导数字字符串。在两种区域设置中，逗号的存在会停止转换过程。净效果是小数部分被丢弃，精度丢失。

结果在 PHP 8 中运行相同代码示例如下所示：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_locale_independent.php
Locale          : en_GB
Original        : 123456.789
Float to String : 123456.789
String to Float : 123456.789
Locale          : fr_FR
Original        : 123456.789
Float to String : 123456.789
String to Float : 123456.789
Locale          : de_DE
Original        : 123456.789
Float to String : 123456.789
String to Float : 123456.789
```

在 PHP 8 中，不会丢失精度，并且数字始终使用句点作为小数分隔符表示，无论区域设置如何。

请注意，您仍然可以通过使用 number_format() 函数或使用 NumberFormatter 类（来自 Intl 扩展）根据其区域设置表示数字。有趣的是，NumberFormatter 类在内部以区域设置无关的方式存储数字！

> **提示**
> 
> 有关更多信息，请查看本文： https://wiki.php.net/rfc/locale_independent_float_to_string.
> 
> 有关国际数字格式的更多信息，请参阅以下链接：
> https://www.php.net/manual/en/class.numberformatter.php

现在您已经了解了 PHP 8 中存在的区域设置无关方面，我们需要查看数组处理方面的更改。

## PHP 8 中的数组处理

除了性能改进之外，PHP 8 数组处理的两个主要更改涉及负偏移量的处理和大括号 ({ }) 的使用。由于这两个更改都可能导致 PHP 8 迁移后应用程序代码中断，因此在此涵盖它们很重要。了解此处提出的问题可以让您更快地使损坏的代码恢复工作。

让我们先来看负数组偏移量的处理。

### 处理负偏移量

在 PHP 中为数组赋值时，如果您未指定索引，PHP 会自动为您分配一个。如此选择索引是一个整数，表示比当前分配的最高整数键大一个的值。如果尚未分配整数索引键，则自动索引分配算法从零开始。

然而在 PHP 7 及更早版本中，在负整数索引的情况下，该算法并未一致应用。如果数字数组的索引以负数开头，则自动索引会跳至零 (0)，而不管下一个数字本应是什么。另一方面，在 PHP 8 中，自动索引始终按 +1 的值递增，无论索引是负整数还是正整数。

如果您的代码依赖于自动索引，并且任何起始索引都是负数，则可能存在向后兼容的代码中断。此问题的检测很困难，因为自动索引会静默发生，而不会发出任何警告或通知。

以下代码示例说明了 PHP 7 和 PHP 8 之间行为的差异：

- 1. 首先，我们定义一个仅使用负整数作为索引的数组。我们使用 var_dump() 来显示此数组：

// /repo/ch06/php8_array_negative_index.php
$a = [-3 => 'CCC', -2 => 'BBB', -1 => 'AAA'];
var_dump($a);
```

2. 然后我们定义第二个数组并将第一个索引初始化为 -3。然后我们添加额外的数组元素，但不指定索引。这会导致自动索引发生：

```php
$b[-3] = 'CCC';
$b[] = 'BBB';
$b[] = 'AAA';
var_dump($b);
```

3. 如果我们然后在 PHP 7 中运行程序，请注意第一个数组呈现正确。只要它们是直接分配的，在 PHP 7 及更早版本中完全可以拥有负数组索引。以下是输出：

```bash
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_array_negative_index.php
/repo/ch06/php8_array_negative_index.php:6:
array(3) {
  [-3] => string(3) "CCC"
  [-2] => string(3) "BBB"
  [-1] => string(3) "AAA"
}
/repo/ch06/php8_array_negative_index.php:12:
array(3) {
  [-3] => string(3) "CCC"
  [0] => string(3) "BBB"
  [1] => string(3) "AAA"
}
```

4. 然而，正如您从第二个 var_dump() 输出中看到的那样，自动数组索引会跳至零，而不管之前的高值如何。

5. 另一方面，在 PHP 8 中，您可以看到输出是一致的。以下是 PHP 8 的输出：

```bash
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_array_negative_index.php
array(3) {
  [-3]=> string(3) "CCC"
  [-2]=> string(3) "BBB"
  [-1]=> string(3) "AAA"
}
}
array(3) {
    [-3]=> string(3) "CCC"
    [-2]=> string(3) "BBB"
    [-1]=> string(3) "AAA"
}
```

6. 如输出所示，数组索引会自动分配，按 1 的值递增，使两个数组相同。

> 提示
> 有关此增强功能的更多信息，请查看本文：
> https://wiki.php.net/rfc/negative_array_index.

现在您已经了解了涉及负值的索引自动分配的潜在代码中断问题，让我们将注意力转向另一个感兴趣的领域：大括号的使用。

### 处理大括号使用更改

大括号 ({ }) 对于创建 PHP 代码的任何开发人员来说都是熟悉的景象。PHP 语言以 C 语言编写，大量使用 C 语法，包括大括号。众所周知，大括号用于在控制结构（例如 if { }）中分隔代码块，在循环（例如 for () { }）中，在函数（例如 function xyz() { }）中，以及类中。

然而在本小节中，我们将把对大括号使用的检查限制在与变量相关联的使用上。PHP 8 中一个可能重大的更改是使用大括号来标识数组元素。自 PHP 8 起，使用大括号指定数组偏移量现已弃用。

旧用法存在高度争议，原因如下：

- 其使用很容易与双引号字符串内的大括号使用混淆。
- 大括号不能用于进行数组赋值。

因此，PHP 核心团队需要么使大括号的使用与方括号 ([ ]) 一致……要么就去掉这种大括号用法。最终决定是移除对数组中使用大括号的支持。

> 提示
> 有关更改背景的更多信息，请参阅以下链接： https://wiki.php.net/rfc/deprecate_curly_braces_array_access.

以下是说明该点的代码示例：

- 1. 首先，我们定义一个回调数组，说明已删除或非法的 大括号用法：
- 2. 然后我们使用 try/catch 块循环遍历回调以捕获抛出的错误：

// /repo/ch06/php7_curly_brace_usage.php
$func = [
    1 => function () {
        $a = ['A' => 111, 'B' => 222, 'C' => 333];
        echo 'WORKS: ' . $a{'C'} . "\n";
    },
    2 => function () {
        eval('$a = {"A","B","C"};');
    },
    3 => function () {
        eval('$a = ["A","B"]; $a{} = "C";');
    }
];
```

```php
foreach ($func as $example => $callback) {
    try {
        echo "\nTesting Example $example\n";
        $callback();
    } catch (Throwable $t) {
        echo $t->getMessage() . "\n";
    }
}
```

如果我们在 PHP 7 中运行示例，第一个回调有效。第二个和第三个会导致抛出 ParseError：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php7_curly_brace_usage.php
Testing Example 1
WORKS: 333
Testing Example 2
syntax error, unexpected '{'
Testing Example 3
syntax error, unexpected '}'
```

当我们在 PHP 8 中运行相同的示例时，然而，没有任何示例有效。以下是 PHP 8 的输出：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php7_curly_brace_usage.php
PHP Fatal error: Array and string offset access syntax with curly braces is no longer supported in /repo/ch06/php7_curly_brace_usage.php on line 8
```

此潜在代码中断很容易检测到。然而，由于您的代码中有许多大括号，您可能需要等待致命错误被抛出才能捕获代码中断。

现在您已经了解了 PHP 8 中数组处理的更改，让我们来看看与安全相关的函数的更改。

## 精通安全函数和设置的更改

PHP 安全功能的任何更改都值得注意。不幸的是，鉴于当今世界的状况，对任何面向 Web 的代码的攻击都是必然的。因此，在本节中，我们讨论了 PHP 8 中与安全相关的 PHP 函数的几项更改。受影响的更改函数包括以下内容：

- assert()
- password_hash()
- crypt()



此外，PHP 8 在使用 `php.ini` 文件中的 `disable_functions` 指令定义任何函数时的处理方式发生了变化。让我们先来看看这个指令。

## 理解禁用函数处理的变更

网站托管公司通常提供大幅折扣的 **共享托管** 套餐。一旦客户注册，托管公司的 IT 人员就会在共享服务器上创建账户，为控制磁盘空间使用分配磁盘配额，并在 Web 服务上创建 **虚拟主机** 定义。然而，这类托管公司面临的问题是，允许 PHP 无限制访问会给共享托管公司以及同一服务器上的其他用户带来安全风险。

为解决此问题，IT 人员通常会将一个用逗号分隔的函数列表分配给 `php.ini` 指令，**disable_functions**。这样一来，列表中的任何函数都无法在该服务器上运行的 PHP 代码中使用。 通常会出现在该列表上的函数是那些允许操作系统访问的函数，例如 `system()` 或 `shell_exec()`。

只有内部 PHP 函数才能出现在该列表中。内部函数是指包含在 PHP 核心中的函数以及通过扩展提供的函数。用户定义的函数不受此指令影响。

## 检查禁用函数处理的差异

在 PHP 7 及更早版本中，禁用函数无法重新定义。在 PHP 8 中，禁用函数被视为从未存在过，这意味着可以重新定义。

> **重要提示**
> 仅仅因为你可以在 PHP 8 中重新定义禁用函数 *并不* 意味着原始功能已被恢复！

为说明这一概念，我们首先在 `php.ini` 文件中添加这一行：
`disable_functions=system`。

请注意，我们需要在 *两个* Docker 容器（PHP 7 和 PHP 8）中都添加此项以完成说明。更新 `php.ini` 文件的命令如下所示：

```
root@php8_tips_php7 [ /repo/ch06 ]#
echo "disable_functions=system">>/etc/php.ini
root@php8_tips_php8 [ /repo/ch06 ]#
echo "disable_functions=system">>/etc/php.ini
```

如果我们随后尝试使用 system() 函数，则在 PHP 7 和 PHP 8 中都会失败。这里我们展示 PHP 8 的输出：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php -r "system('ls -l');"
PHP Fatal error: Uncaught Error: Call to undefined function system() in Command line code:1
```

我们随后定义一些程序代码来重新定义被禁止的函数：

// /repo/ch06/php8_disabled_funcs_redefine.php
function system(string $cmd, string $path = NULL) {
    $output = '';
    $path = $path ?? __DIR__;
    if ($cmd === 'ls -l') {
        $iter = new RecursiveDirectoryIterator($path);
        foreach ($iter as $fn => $obj)
            $output .= $fn . "\n";
    }
    return $output;
}
echo system('ls -l');
```

正如代码示例所示，我们创建了一个函数，它模仿 ls -l Linux 系统调用的行为，但仅使用安全的 PHP 函数和类。然而，如果我们尝试在 PHP 7 中运行它，则会抛出致命错误。以下是 PHP 7 的输出：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_disabled_funcs_redefine.php
PHP Fatal error: Cannot redeclare system() in /repo/ch06/php8_disabled_funcs_redefine.php on line 17
```

然而在 PHP 8 中，我们的函数重新定义成功了，如下所示：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_disabled_funcs_redefine.php
/repo/ch06/php8_printf_vs_vprintf.php
/repo/ch06/php8_num_str_non_wf_extracted.php
/repo/ch06/php8_vprintf_bc_break.php
/repo/ch06/php7_vprintf_bc_break.php
```

```
... not all output is shown ...
/repo/ch06/php7_curly_brace_usage.php
/repo/ch06/php7_compare_num_str_valid.php
/repo/ch06/php8_compare_num_str.php
/repo/ch06/php8_disabled_funcs_redefine.php
```

你现在已经对如何处理禁用函数有了一个概念。接下来，让我们来看看重要的 crypt() 函数的变更。

## 了解 crypt() 函数的变更

crypt() 函数自 PHP 4 版本以来一直是 PHP 哈希生成的支柱。其持久存在的一个原因是因为它有许多选项。如果你直接在代码中使用 crypt()，你会很高兴地注意到，如果提供了不可用的 salt 值，长期以来被认为已损坏的数据加密标准 (DES)，不再是 PHP 8 中的后备方案！salt 有时也被称为初始化向量 (IV)。

另一个重要变更涉及 rounds 值。一次轮次就像洗牌：洗的次数越多，随机化程度越高（除非你面对的是拉斯维加斯的赌牌高手！）。在密码学中，块类似于牌。在每一轮中，密码学函数会应用于每个块。如果密码学函数很简单，哈希可以更快地生成；然而，需要更多的轮次才能完全随机化块。

SHA-1（安全哈希算法）家族使用快速但简单的算法，因此需要更多轮次。另一方面，SHA-2 家族使用更复杂的哈希函数，消耗更多资源，但轮次更少。

在使用 PHP crypt() 函数与 CRYPT_SHA256（SHA-2 家族）结合时，PHP 8 不再静默地将 rounds 参数解析为最接近的限制。相反，crypt() 将以 *0 返回值失败，与 glibc 行为一致。此外，在 PHP 8 中，第二个参数（salt）现在是强制性的。

以下示例说明了使用 crypt() 函数时 PHP 7 和 PHP 8 之间的差异：

- 1. 首先，我们定义表示不可用 salt 值和非法轮次数的变量：

// /repo/ch06/php8_crypt_sha256.php
$password = 'password';
$salt = str_repeat('+x=', CRYPT_SALT_LENGTH + 1);
$rounds = 1;
```

- 2. 然后我们使用 crypt() 函数创建两个哈希。在第一次使用中，$default 是在提供无效 salt 参数后的结果。第二次使用中，$sha256 提供了有效的 salt 值，但轮次数无效：

```
$default = crypt($password, $salt);
$sha256 = crypt($password,
'$5$rounds=' . $rounds . '$' . $salt . '$');
echo "Default : $default\n";
echo "SHA-256 : $sha256\n";
```

以下是代码示例在 PHP 7 中运行的输出：

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_crypt_sha256.php
PHP Deprecated: crypt(): Supplied salt is not valid for DES. Possible bug in provided salt format. in /repo/ch06/php8_crypt_sha256.php on line 7
Default : +xj31ZMTZkVA
SHA-256 : $5$rounds=1000$+x=+x=+x=+x=+x=+
$3Si/vFn6/xmdTdyleJ17Rb9Heg6DWgkRVKS9T0Zzy/B
```

注意 PHP 7 如何静默修改原始请求。在第一种情况下，crypt() 回退到 DES（！）。在第二种情况下，PHP 7 静默地将 rounds 值从 1 修改为最接近的限制 1000。

另一方面，同一代码在 PHP 8 中运行会失败并返回 *0，如下所示：

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_crypt_sha256.php
Default : *0
SHA-256 : *0
```

正如我们在本书中反复强调的，当 PHP 为你做出假设时，最终你会得到产生不一致结果的糟糕代码。在刚才显示的代码示例中，最佳实践是定义一个类方法或函数，以对参数施加更大的控制。通过这种方式，你可以验证参数，避免依赖 PHP 的假设。

接下来，我们来看看 password_hash() 函数的变更。

## 处理 password_hash() 的变更

多年来，太多开发者误用了 crypt()，PHP 核心团队决定添加一个包装函数 password_hash()。这被证明是一个巨大的成功，现在是使用最广泛的安全函数之一。以下是 password_hash() 的函数签名：

```
password_hash(string $password, mixed $algo, array $options=?)
```

当前支持的算法包括 bcrypt、Argon2i 和 Argon2id。建议你使用预定义的算法常量：PASSWORD_BCRYPT、PASSWORD_ARGON2I 和 PASSWORD_ARGON2ID。PASSWORD_DEFAULT 算法目前设置为 bcrypt。选项因算法而异。如果使用 PASSWORD_BCRYPT 或 PASSWORD_DEFAULT 算法，选项包括 cost 和 salt。

常规 wisdom 建议最好使用 password_hash() 函数创建的随机生成的 salt。在 PHP 7 中，salt 选项已被弃用，并在 PHP 8 中被忽略。这不会导致向后兼容性中断，除非你因其他原因依赖 salt。

在此代码示例中，使用了非随机 salt 值：

// /repo/ch06/php8_password_hash.php
$salt = 'xxxxxxxxxxxxxxxxxxxxxxxx';
$password = 'password';
$hash = password_hash(
    $password, PASSWORD_DEFAULT, ['salt' => $salt]);
echo $hash . "\n";
var_dump(password_get_info($hash));
```

在 PHP 7 输出中，会发出弃用 Notice：

```
root@php8_tips_php7 [ /repo/ch06 ]# php php8_password_hash.php
PHP Deprecated: password_hash(): Use of the 'salt' option to password_hash is deprecated in /repo/ch06/php8_password_hash.php on line 6
$2y$10$xxxxxxxxxxxxxxxxxxxxxxu0d9YtxiLKHM/198x//sqUV1V2XTZEZ.
/repo/ch06/php8_password_hash.php:8:
array(3) {
  'algo' => int(1)
```



```
'algoName' => string(6) "bcrypt"
'options' => array(1) { 'cost' => int(10) }
}
```

你还会注意到，从 PHP 7 的输出中，非随机盐值清晰可见。还有一点需要注意的是，当执行 password_get_info() 时，algo 键显示的值是一个整数，对应于其中一个预定义的算法常量。

PHP 8 的输出有所不同，如下所示：

```
root@php8_tips_php8 [ /repo/ch06 ]# php php8_password_hash.php
PHP Warning: password_hash(): The "salt" option has been ignored, since providing a custom salt is no longer supported in /repo/ch06/php8_password_hash.php on line 6
$2y$10$HQNRjL.kCkXaR1ZAOFI3TuBJd11k4YCRWmtrIlB7ZDaX1Jngh9UNW
array(3) {
["algo"]=> string(2) "2y"
["algoName"]=> string(6) "bcrypt"
["options"]=> array(1) { ["cost"]=> int(10) }
}
```

你可以看出盐值被忽略了，而是使用了随机盐。与 Notice 不同，PHP 8 会针对使用 salt 选项发出 Warning。输出的另一个需要注意的点是，在 PHP 8 中调用 password_get_info() 时，algorithm 键返回的是字符串而不是整数。这是因为预定义的算法常量现在是字符串值，对应于在 crypt() 函数中使用时的签名。

我们将在下一小节中检查的最后一个函数是 assert()。

## 了解 assert() 的更改

assert() 函数通常与测试和诊断相关。我们将其包含在此小节中，因为它通常具有安全影响。开发人员在尝试追踪潜在安全漏洞时有时会使用此函数。

要使用 assert() 函数，你必须首先通过在 php.ini 文件中添加设置 zend.assertions=1 来启用它。启用后，你可以在应用程序代码中的任何位置放置一个或多个 assert() 函数调用。

## 理解 assert() 用法的更改

从 PHP 8 开始，不再可能向 `assert()` 提供要求值的字符串参数：相反，你必须提供一个表达式。这会导致潜在的代码中断，因为在 PHP 8 中，字符串被视为表达式，因此始终解析为布尔值 `TRUE`。此外，`assert.quiet_eval` php.ini 指令以及与 `assert_options()` 一起使用的预定义常量 `ASSERT_QUIET_EVAL` 在 PHP 8 中已被移除，因为它们现在没有效果。

为了说明潜在问题，我们首先通过设置 php.ini 指令 `zend.assertions=1` 来激活断言。然后我们按如下方式定义一个示例程序：

- 1. 我们使用 `ini_set()` 使 `assert()` 抛出异常。我们还定义了一个变量 `$pi`：
- 2. 然后我们尝试将断言作为表达式 `$pi === M_PI`：
- 3. 在最后一个 try/catch 块中，我们尝试将断言作为字符串：

```php
// /repo/ch06/php8_assert.php
ini_set('assert.exception', 1);
$pi = 22/7;
echo 'Value of 22/7: ' . $pi . "\n";
echo 'Value of M_PI: ' . M_PI . "\n";
```

```php
try {
    $line   = __LINE__ + 2;
    $message = "Assertion expression failed ${line}\n";
    $result  = assert($pi === M_PI,
        new AssertionError($message));
    echo ($result) ? "Everything's OK\n"
        : "We have a problem\n";
} catch (Throwable $t) {
    echo $t->getMessage() . "\n";
}
```

```php
try {
    $line   = __LINE__ + 2;
    $message = "Assertion string failed ${line}\n";
    $result  = assert('$pi === M_PI',
        new AssertionError($message));
    echo ($result) ? "Everything's OK\n"
        : "We have a problem\n";
} catch (Throwable $t) {
    echo $t->getMessage() . "\n";
}
```

- 4. 当我们在 PHP 7 中运行程序时，一切按预期工作：

```
root@php8_tips_php7 [ /repo/ch06 ]# php php8_assert.php
Value of 22/7: 3.1428571428571
Value of M_PI: 3.1415926535898
Assertion as expression failed on line 18
Assertion as a string failed on line 28
```

- 5. M_PI 的值来自数学扩展，比简单地将 22 除以 7 要准确得多！因此，两个断言都会抛出异常。然而在 PHP 8 中，输出明显不同：

```
root@php8_tips_php8 [ /repo/ch06 ]# php php8_assert.php
Value of 22/7: 3.1428571428571
Value of M_PI: 3.1415926535898
Assertion as expression failed on line 18
Everything's OK
```

字符串形式的断言被解释为表达式。因为字符串不为空，布尔结果为 TRUE，从而返回假阳性。如果你的代码依赖于字符串形式的断言结果，它注定会失败。然而从 PHP 8 的输出可以看出，表达式形式的断言在 PHP 8 中与 PHP 7 中工作方式相同。

Tip
提示
最佳实践：不要在生产代码中使用 assert()。如果你确实使用 assert()，始终提供表达式，而不是字符串。

现在你对安全相关函数的更改有了一定的了解，我们就此结束本章。

## 摘要

在本章中，你了解了 PHP 8 与早期版本之间字符串处理方面的差异，以及如何开发解决字符串处理差异的变通方法。正如你所学，PHP 8 对字符串函数参数的数据类型施加了更大的控制，并引入了在参数缺失或为空时的行为一致性。正如你所学，PHP 早期版本的一个大问题是，代表你做了几个静默假设，导致出现大量意外结果的潜在风险。

在本章中，我们还强调了涉及数字字符串与数字数据比较的问题。你不仅了解了数字字符串、类型转换和非严格比较，还了解了 PHP 8 如何纠正早期版本中存在的数字字符串处理固有缺陷。 本章涵盖的另一个主题展示了与若干运算符在 PHP 8 中行为不同的相关潜在问题。你学习了如何发现潜在问题，并被提供了改进代码健壮性的最佳实践。

本章还讨论了许多 PHP 函数对区域设置依赖的问题，以及 PHP 8 如何解决此问题。你了解到在 PHP 8 中，浮点表示现在是统一的，不再依赖于区域设置。你还了解了 PHP 8 处理数组元素的方式的更改以及若干安全相关函数的更改。

本章涵盖的提示、技巧和技术提高了人们对 PHP 早期版本不一致行为的认识。有了这种新的认识，你能更好地控制 PHP 代码的使用。你现在也更能发现可能导致 PHP 8 迁移后代码中断的情况，这使你比其他开发人员更具优势，最终能编写可靠且一致的 PHP 代码。

下一章将向你展示如何避免涉及 PHP 扩展更改的潜在代码中断。
