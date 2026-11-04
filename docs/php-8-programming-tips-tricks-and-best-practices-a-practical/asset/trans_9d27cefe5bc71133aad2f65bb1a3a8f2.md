
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
