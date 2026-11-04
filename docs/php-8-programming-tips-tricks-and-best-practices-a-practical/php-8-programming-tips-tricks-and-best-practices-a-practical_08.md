## 8 了解 PHP 8 中已弃用或移除的功能

This chapter walks you through functionality that has been deprecated or removed in PHP Hypertext Preprocessor 8 (PHP 8). This information is extremely important for any developer to know. Any code that uses removed functionality must be rewritten before an upgrade to PHP 8. Likewise, any deprecation is a clear signal to you that you must rewrite any code that depends upon such functionality, or risk problems in the future.

After you have read the material in this chapter and followed the example application code, you can detect and rewrite code that has been deprecated. You can also develop workarounds for functionality that has been removed and learn how to refactor code that uses removed functionality involving extensions. Another important skill you will learn from this chapter is how to improve application security by rewriting code depending on removed functions.

Topics covered in this chapter include the following:

- Discovering what has been removed from the core
- Examining core deprecations
- Working with removed functionality in PHP 8 extensions
- Dealing with deprecated or removed security-related functionality

## 技术要求

To examine and run the code examples provided in this chapter, the minimum recommended hardware is outlined here:

- x86_64-based desktop PC or laptop
- 1 gigabyte (GB) free disk space
- 4 GB of random-access memory (RAM)
- 500 kilobits per second (Kbps) or faster internet connection

In addition, you will need to install the following software:

- Docker
- Docker Compose

Please refer to the Technical requirements section of Chapter 1, Introducing New PHP 8 OOP Features, for more information on Docker and Docker Compose installation, as well as how to build the Docker container used to demonstrate the code explained in this book. In this book, we refer to the directory in which you restored the sample code for the book as /repo.

The source code for this chapter is located here:

```
https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices
```

We can now begin our discussion by examining the core functionality removed in PHP 8.

## 发现核心中已移除的内容

In this section, we consider not only functions and classes that have been removed from PHP 8, but we will also have a look at usage that has been removed as well. We will then have a look at class methods and functions that still exist but no longer serve any useful purpose due to other changes in PHP 8. Knowing which functions have been removed is extremely important in order to protect against a potential code break following a PHP 8 migration.

Let's start by examining functions removed in PHP 8.

## 检查 PHP 8 中移除的函数

There are a number of functions in the PHP language that have only been retained thus far in order to maintain backward compatibility. However, maintenance of such functions drains resources away from core language development. Further, for the most part, such functions have been superseded by better programming constructs. Accordingly, there has been a slow process whereby such commands have been slowly dropped from the language as evidence has mounted that they are no longer being used.

> **提示**
> The PHP core team occasionally runs statistical analysis on PHP repositories based on GitHub. In this way, they are able to determine the frequency of usage of the various commands in the PHP core.

The table shown next summarizes the functions that have been removed in PHP 8 and what to use in their place:

| 已移除函数 | 建议替代方案 |
|---|---|
| __autoload() | spl_autoload_register() |
| convert_cyr_string() | mb_convert_encoding() |
| create_function() | function () {}<br>fn () => <expression> |
| each() | ArrayIterator |
| ezmlm_hash() | none other than running an OS command |
| fgetss() | strip_tags(fgets()) |
| get_magic_quotes_gpc()<br>get_magic_quotes_runtime() | none: the magic quotes feature itself has been removed from PHP |
| hebrevc() | hebrev() |
| is_real() | is_float() |
| money_format() | NumberFormatter::formatCurrency |
| restore_include_path() | ini_restore('include_path') |

表 8.1 – PHP 8 移除的函数及建议替代方案

For the remainder of this section, we cover a few of the more important removed functions and give you suggestions on how to refactor your code to achieve the same results. Let's start by examining each().

## 使用 each()

each() was introduced in PHP 4 as a way of walking through an array, producing key/value pairs upon each iteration. The syntax and usage of each() is extremely simple and is oriented toward procedural usage. We'll show a short code example that demonstrates each() usage, as follows:

- 1. In this code example, we first open a connection to a data file containing city data from the GeoNames (https://geonames.org) project, as follows:

```
// /repo/ch08/php7_each.php
$data_src = __DIR__
    . '../sample_data/cities15000_min.txt';
$fh       = fopen($data_src, 'r');
$pattern  = "%30s : %20s\n";
$target   = 1000000;
$data     = [];
```



2. 我们随后使用 fgetcsv() 函数将一行数据提取到 $line 中，并将纬度和经度信息打包到 $data 数组中。请注意，在下面的代码片段中，我们过滤掉了人口少于 $target 的城市的数据行（在本例中，少于 10 million）：

```
while ($line = fgetcsv($fh, '', "\t")) {
    $popNum = $line[14] ?? 0;
    if ($popNum > $target) {
        $city = $line[1] ?? 'Unknown';
        $data[$city] = $line[4] . ',' . $line[5];
    }
}
```

3. 我们随后关闭文件句柄并按城市名称对数组进行排序。为了呈现输出，我们使用 each() 遍历数组，生成键/值对，其中城市是键，经纬度是值。代码在下面的片段中说明：

```
fclose($fh);
ksort($data);
printf($pattern, 'City', 'Latitude/Longitude');
printf($pattern, '----', '------------------');
while ([$city, $latLon] = each($data)) {
    $city = str_pad($city, 30, ' ', STR_PAD_LEFT);
    printf($pattern, $city, $latLon);
}
```

以下是它在 PHP 7 中显示的输出：

```
root@php8_tips_php7 [/repo/ch08]# php php7_each.php
                City :   Latitude/Longitude
                ---- :   ------------------
            Beijing :    39.9075,116.39723
        Buenos Aires :   -34.61315,-58.37723
              Delhi :    28.65195,77.23149
              Dhaka :    23.7104,90.40744
          Guangzhou :    23.11667,113.25
           Istanbul :    41.01384,28.94966
            Karachi :    24.8608,67.0104
```

| 城市 | 坐标 |
|---|---|
| Mexico City : | 19.42847,-99.12766 |
| Moscow : | 55.75222,37.61556 |
| Mumbai : | 19.07283,72.88261 |
| Seoul : | 37.566,126.9784 |
| Shanghai : | 31.22222,121.45806 |
| Shenzhen : | 22.54554,114.0683 |
| São Paulo : | -23.5475,-46.63611 |
| Tianjin : | 39.14222,117.17667 |

此代码示例在 PHP 8 中将无法工作，因为 each() 已被移除。*最佳实践*是转向 **面向对象编程（OOP）** 方法：改用 ArrayIterator 而不是 each()。下一个代码示例产生的结果与之前完全相同，但使用对象类而不是过程式函数：

- 1. 而不是使用 fopen()，我们改为创建 SplFileObject 实例。你还会注意到在下面的代码片段中，我们不是创建数组，而是创建 ArrayIterator 实例来保存最终数据：
- 2. 我们随后使用 fgetcsv() 方法遍历数据文件以检索一行，并使用 offsetSet() 将其追加到迭代中，如下所示：

// /repo/ch08/php8_each_replacements.php
$data_src = __DIR__
    . '../sample_data/cities15000_min.txt';
$fh       = new SplFileObject($data_src, 'r');
$pattern  = "%30s : %20s\n";
$target   = 10000000;
$data     = new ArrayIterator();
```

```
while ($line = $fh->fgetcsv("\t")) {
    $popNum = $line[14] ?? 0;
    if ($popNum > $target) {
        $city = $line[1] ?? 'Unknown';
        $data->offsetSet($city, $line[4] . ',' . $line[5]);
    }
}
```

3. 最后，我们按键排序，回绕到顶部，并在迭代仍有更多值时循环。我们使用 key() 和 current() 方法检索键/值对，如下所示：

```php
$data->ksort();
$data->rewind();
printf($pattern, 'City', 'Latitude/Longitude');
printf($pattern, '----', '------------------');
while ($data->valid()) {
    $city = str_pad($data->key(), 30, ' ', STR_PAD_LEFT);
    printf($pattern, $city, $data->current());
    $data->next();
}
```

此代码示例实际上可在任何版本的 PHP 中运行，从 PHP 5.1 直到包括 PHP 8！输出与前面 PHP 7 的输出完全相同，此处不再重复。

现在让我们来看看 create_function()。

## 使用 create_function()

在 PHP 5.3 之前，将函数分配给变量的唯一方法是使用 create_function()。从 PHP 5.3 开始，首选方法是定义匿名函数。匿名函数虽然在技术上属于过程式编程应用程序编程接口（API）的一部分，但实际上是 Closure 类的实例，因此也属于 OOP 的范畴。

> **提示**
> 如果你需要的功能可以浓缩为单个表达式，在 PHP 8 中你也可以选择使用 **箭头函数。**

当由 create_function() 定义的函数被执行时，PHP 内部会执行 eval() 函数。然而，这种架构的结果是别扭的语法。匿名函数的性能相当且更直观易用。

以下示例演示了 create_function() 的用法。此示例的目标是扫描 Web 服务器访问日志并按 Internet 协议（IP）地址对结果进行排序：

1. 我们首先以微秒为单位记录开始时间。稍后，我们使用此值来确定性能。以下是你需要的代码：

// /repo/ch08/php7_create_function.php
$start = microtime(TRUE);
```

2. 接下来，使用 create_function() 定义一个回调，将每行开头的 IP 地址重新组织为每个恰好三位数字的统一段。我们需要这样做以便执行正确的排序（稍后定义）。create_function() 的第一个参数是一个表示参数的字符串。第二个参数是要执行的实际代码。代码在下面的片段中说明：

```
$normalize = create_function(
    '&$line, $key',
    '$split = strpos($line, " ");'
    . '$ip = trim(substr($line, 0, $split));'
    . '$remainder = substr($line, $split);'
    . '$tmp = explode(".", $ip);'
    . 'if (count($tmp) === 4)'
    . '    $ip = vsprintf("%03d.%03d.%03d.%03d", $tmp);'
    . '$line = $ip . $remainder;'
);
```

请注意大量使用字符串。这种别扭的语法很容易导致语法或逻辑错误，因为大多数代码编辑器不会费力解释嵌入在字符串中的命令。

3. 接下来，我们定义一个用于与 usort() 一起使用的排序回调，如下所示：

```
$sort_by_ip = create_function(
    '$line1, $line2',
    'return $line1 <=> $line2;' );
```

4. 我们随后使用 file() 函数将访问日志的内容提取到数组中。我们还将 $sorted 移动到文件中以保存排序后的访问日志条目。代码在下面的片段中说明：

```
$orig  = __DIR__ . '../sample_data/access.log';
$log   = file($orig);
$sorted = new SplFileObject(__DIR__ .
    './access_sorted_by_ip.log', 'w');
```

5. 然后我们能够使用 array_walk() 规范化 IP 地址，并使用 usort() 执行排序，如下所示：

```
array_walk($log, $normalize);
usort($log, $sort_by_ip);
```

6. 最后，我们将排序后的条目写入备用日志文件并显示开始和停止之间的时间差，如下所示：

```
foreach ($log as $line) $sorted->fwrite($line);
$time = microtime(TRUE) - $start;
echo "Time Diff: $time\n";
```

我们没有展示完整的备用访问日志，因为它太长无法包含在书中。相反，这里是从列表中间提取的十几行，以让你了解输出的大致情况：

```
094.198.051.136 - - [15/Mar/2021:10:05:06 -0400]    "GET /
courses HTTP/1.0" 200 21530
094.229.167.053 - - [21/Mar/2021:23:38:44 -0400]
    "GET /wp-login.php HTTP/1.0" 200 34605
095.052.077.114 - - [10/Mar/2021:22:45:55 -0500]
    "POST /career HTTP/1.0" 200 29002
095.103.103.223 - - [17/Mar/2021:15:48:39 -0400]
    "GET /images/courses/php8_logo.png HTTP/1.0" 200 9280
095.154.221.094 - - [25/Mar/2021:11:43:52 -0400]
    "POST / HTTP/1.0" 200 34546
095.154.221.094 - - [25/Mar/2021:11:43:52 -0400]
    "POST / HTTP/1.0" 200 34691
095.163.152.003 - - [14/Mar/2021:16:09:05 -0400]
    "GET /images/courses/mongodb_logo.png HTTP/1.0" 200 11084
```

```
095.163.255.032 - - [13/Apr/2021:15:09:40 -0400]
"GET /robots.txt HTTP/1.0" 200 78
095.163.255.036 - - [18/Apr/2021:01:06:33 -0400]
"GET /robots.txt HTTP/1.0" 200 78
```

在 PHP 8 中，要完成相同的任务，我们定义匿名函数而不是使用 create_function()。以下是重写后的代码示例在 PHP 8 中可能的样子：

1. 再次，我们首先记录开始时间，就像前面描述的 PHP 7 代码示例一样。以下是完成此操作所需的代码：

// /repo/ch08/php8_create_function.php
$start = microtime(TRUE);
```

2. 接下来，我们定义一个回调，将 IP 地址规范化为四个各为三位数字的块。我们使用与前一个示例完全相同的逻辑；然而，这次我们以匿名函数的形式定义命令。这利用了代码编辑器助手，并且代码编辑器将每一行视为实际的 PHP 命令。代码在下面的片段中说明：

```
$normalize = function (&$line, $key) {
    $split = strpos($line, ' ');
    $ip = trim(substr($line, 0, $split));
    $remainder = substr($line, $split);
    $tmp = explode(".", $ip);
    if (count($tmp) === 4)
        $ip = vsprintf("%03d.%03d.%03d.%03d", $tmp);
    $line = $ip . $remainder;
};
```

因为匿名函数中的每一行都被视为定义普通 PHP 函数一样处理，所以你不太可能出现拼写错误或语法错误。

3. 以类似的方式，我们以箭头函数的形式定义排序回调，如下所示：

```
$sort_by_ip = fn ($line1, $line2) => $line1 <=> $line2;
```



The remainder of the code example is exactly the same as described earlier and is not shown here. Likewise, the output is exactly the same. The performance time is also approximately the same.

We now turn our attention to money_format().

## Working with money_format()
使用 money_format()

The money_format() function, first introduced in PHP 4.3, is designed to display monetary values using international currencies. If you are maintaining an international PHP-based website that has any financial transactions, you might be affected by this change after a PHP 8 update.

money_format() 函数首次在 PHP 4.3 中引入，旨在使用国际货币显示货币值。如果您维护的是具有任何金融交易的国际 PHP 网站，在 PHP 8 更新后您可能会受到此更改的影响。

The latter was introduced in PHP 5.3, and should thus not cause your code to break. Let's have a look at a simple example involving money_format() and how it can be rewritten to work in PHP 8, as follows:

后者在 PHP 5.3 中引入，因此不应导致您的代码中断。让我们看一个涉及 money_format() 的简单示例，以及如何将其重写以在 PHP 8 中工作，如下所示：

- 1. We first assign an amount to a $amt variable. We then set the monetary locale to en_US (**United States**, or **US**) and echo the value using money_format(). We use the %n format code for national formatting, followed by the %i code for international rendering. In the latter case, the **International Organization for Standardization (ISO)** currency code (**US Dollars**, or **USD**) is displayed. The code is illustrated in the following snippet:
- 1. 我们首先将一个金额赋给 $amt 变量。然后我们将货币区域设置设为 en_US（**美国**，或 **US**）并使用 money_format() 输出该值。我们使用 %n 格式代码进行国家/地区格式化，然后使用 %i 代码进行国际渲染。在后一种情况下，**国际标准化组织（ISO）**货币代码（**美元**，或 **USD**）将被显示。代码在以下片段中说明：
- 2. We then change the monetary locale to de_DE (Germany) and echo the same amount in both national and international formats, as follows:
- 2. 然后我们将货币区域设置更改为 de_DE（德国）并按如下方式以国家和国际格式输出相同金额：

```php
// /repo/ch08/php7_money_format.php
$amt = 1234567.89;
setlocale(LC_MONETARY, 'en_US');
echo "Natl: " . money_format('%n', $amt) . "\n";
echo "Intl: " . money_format('%i', $amt) . "\n";
```

```php
setlocale(LC_MONETARY, 'de_DE');
echo "Natl: " . money_format('%n', $amt) . "\n";
echo "Intl: " . money_format('%i', $amt) . "\n";
```

Here is the output in PHP 7.1:

以下是 PHP 7.1 中的输出：

```
root@php8_tips_php7 [ /repo/ch08 ]# php php7_money_format.php
Natl: $1,234,567.89
Intl: USD 1,234,567.89
Natl: 1.234.567,89 EUR
Intl: 1.234.567,89 EUR
```

You might note from the output that money_format() did not render the Euro symbol, only the ISO code (EUR). It did, however, properly format the amounts, using a comma for the thousands separator, a period for the decimal separator for the en_US locale, and the reverse for the de_DE locale.

您可能会注意到，从输出中 money_format() 并未渲染欧元符号，仅显示 ISO 代码（EUR）。不过，它确实正确地格式化了金额，对于 en_US 区域设置，使用逗号作为千位分隔符，句点作为小数分隔符，而对于 de_DE 区域设置则相反。

A best practice is to replace any usage of money_format() with NumberFormatter::formatCurrency(). Here is the preceding example, rewritten to work in PHP 8. Please note that the same example will also work in any version of PHP from 5.3 onward! We'll proceed as follows:

最佳实践是用 NumberFormatter::formatCurrency() 替换任何 money_format() 的使用。以下是前述示例，重写后可在 PHP 8 中工作。请注意，相同的示例也适用于 PHP 5.3 及更高版本！我们将按如下方式进行：

- 1. First, we assign the amount to $amt and create a NumberFormatter instance. In creating this instance, we supply arguments that indicate the locale and type of number—in this case, currency. We then use the formatCurrency() method to produce the national representation of this amount, as illustrated in the following code snippet:
- 1. 首先，我们将金额赋给 $amt 并创建一个 NumberFormatter 实例。在创建此实例时，我们提供参数来指示区域设置和数字类型——在本例中为货币。然后我们使用 formatCurrency() 方法生成该金额的国家表示形式，如以下代码片段所示：
- 2. In order to produce the ISO currency code—in this case, USD—we need to use the setSymbol() method. Otherwise, the default is to produce the $ currency symbol instead of the USD ISO code. We then use the format() method to render the output. Note the trailing space after USD in the following code snippet. This is to prevent the ISO code from running into the number when echoed!
- 2. 为了生成 ISO 货币代码——在本例中为 USD——我们需要使用 setSymbol() 方法。否则，默认会生成 $ 货币符号而不是 USD ISO 代码。然后我们使用 format() 方法渲染输出。请注意以下代码片段中 USD 后面的尾随空格。这是为了防止 ISO 代码在回显时与数字连在一起！

```php
// /repo/ch08/php8_number_formatter_fmt_curr.php
$amt = 1234567.89;
$fmt = new NumberFormatter('en_US',
    NumberFormatter::CURRENCY );
echo "Natl: " . $fmt->formatCurrency($amt, 'USD') . "\n";
```

```php
$fmt->setSymbol(NumberFormatter::CURRENCY_SYMBOL,'USD ');
echo "Intl: " . $fmt->format($amt) . "\n";
```

- 3. We then format the same amount using the de_DE locale, as follows:
- 3. 然后我们使用 de_DE 区域设置格式化相同的金额，如下所示：

```php
$fmt = new NumberFormatter( 'de_DE',
    NumberFormatter::CURRENCY );
echo "Natl: " . $fmt->formatCurrency($amt, 'EUR') . "\n";
$fmt->setSymbol(NumberFormatter::CURRENCY_SYMBOL, 'EUR');
echo "Intl: " . $fmt->format($amt) . "\n";
```

Here is the output from PHP 8:

以下是来自 PHP 8 的输出：

```
root@php8_tips_php8 [ /repo/ch08 ]#
php php8_number_formatter_fmt_curr.php
Natl: $1,234,567.89
Intl: USD 1,234,567.89
Natl: 1.234.567,89 €
Intl: 1.234.567,89 EUR
```

As you can see from the output, the comma decimal is reversed between the en_US and de_DE locales, as expected. You also see that both the currency symbols, as well as the ISO currency codes, are correctly rendered.

正如您从输出中看到的，逗号小数在 en_US 和 de_DE 区域设置之间是反转的，正如预期的那样。您还可以看到货币符号以及 ISO 货币代码都已正确渲染。

Now that you have an idea of how to replace money_format(), let's have a look at other programming code usage that has been removed in PHP 8.

现在您已经了解了如何替换 money_format()，让我们来看一下在 PHP 8 中已删除的其他编程代码用法。

## Discovering other PHP 8 usage changes
发现其他 PHP 8 用法更改

There are a number of program code usage changes that you need to be aware of in PHP 8. We'll start with a look at two typecasts that are no longer allowed.

在 PHP 8 中有许多程序代码用法更改您需要了解。我们将从查看两种不再允许的类型转换开始。

### Removed typecasts
已移除的类型转换

Developers often use forced typecasts in order to ensure the data type of a variable is appropriate for a particular usage. As an example, when processing a HyperText Markup Language (HTML) form submission, for the sake of argument, let's say one of the form elements represents a monetary amount. A quick and easy way to sanitize this data element is to typecast it to a float data type, as follows:

开发人员经常使用强制类型转换以确保变量的数据类型适合特定用途。例如，在处理超文本标记语言（HTML）表单提交时，为便于说明，假设表单元素之一代表货币金额。快速简便的清理此数据元素的方法是将其类型转换为 float 数据类型，如下所示：

```
$amount = (float) $_POST['amount'];
```

However, rather than typecast to float, some developers prefer to use real or double. Interestingly, all three produce exactly the same result! In PHP 8, the typecast to real has been removed. If your code uses this typecast, a best practice is to change it to float.

然而，与其类型转换为 float，一些开发人员更喜欢使用 real 或 double。有趣的是，这三种方式产生的结果完全相同！在 PHP 8 中，类型转换为 real 已被移除。如果您的代码使用此类型转换，最佳实践是将其更改为 float。

The unset typecast has also been removed. The purpose of this typecast is to unset a variable. In the following code snippet, the value of $obj becomes NULL:

unset 类型转换也被移除了。此类型转换的目的是取消设置变量。在以下代码片段中，$obj 的值变为 NULL：

```php
$obj = new ArrayObject();
/* some code (not shown) */
$obj = (unset) $obj;
```

A best practice in PHP 8 is to use either of the following:

在 PHP 8 中，最佳实践是使用以下任一方法：

```php
$obj = NULL;
// or this:
unset($obj);
```

Let's now turn our attention to anonymous functions.

现在让我们将注意力转向匿名函数。

### Changes in generating anonymous functions from class methods
从类方法生成匿名函数的更改

In PHP 7.1, a new Closure::fromCallable() method was added that allows you to return a class method as a Closure instance (for example, an anonymous function). ReflectionMethod::getClosure() was also introduced and is also able to convert a class method into an anonymous function.

在 PHP 7.1 中，新增了 Closure::fromCallable() 方法，允许您将类方法作为 Closure 实例（例如匿名函数）返回。ReflectionMethod::getClosure() 也被引入，并且也能将类方法转换为匿名函数。

To illustrate this, we define a class that returns Closure instances able to perform hashing using different algorithms. We'll proceed as follows:

为了说明这一点，我们定义一个类，该类返回能够使用不同算法执行哈希的 Closure 实例。我们将按如下方式进行：

- 1. First, we define a class and a public $class property, as follows:
- 1. 首先，我们定义一个类和一个公共 $class 属性，如下所示：
- 2. We then define a method that produces one of three callbacks, each designed to produce a different type of hash, as follows:
- 2. 然后我们定义一个方法，该方法生成三个回调之一，每个回调都设计用于生成不同类型的哈希，如下所示：

```php
// /repo/src/Services/HashGen.php
namespace Services;
use Closure;
class HashGen {
    public $class = 'HashGen: ';
}
```

```php
public function makeHash(string $type) {
    $method = 'hashTo' . ucfirst($type);
    if (method_exists($this, $method))
        return Closure::fromCallable(
            [$this, $method]);
    else
```

```php
return Closure::fromCallable(
    [$this, 'doNothing']);
}
}
```

- 3. Next, we define three different methods, each producing a different form of hash (not shown): hashToMd5 (), hashToSha256 (), and doNothing ().
- 3. 接下来，我们定义三种不同的方法，每种方法生成不同形式的哈希（未显示）：hashToMd5()、hashToSha256() 和 doNothing()。
- 4. In order to make use of the class, a calling program is devised that first includes the class file and creates an instance, as follows:
- 4. 为了使用该类，调用程序首先包含类文件并创建实例，如下所示：

```php
// /repo/ch08/php7_closure_from_callable.php
require __DIR__ . '../src/Services/HashGen.php';
use Services\HashGen;
$hashGen = new HashGen();
```

- 5. The callback is then executed followed by var_dump () to view information about the Closure instance, as illustrated in the following code snippet:
- 5. 然后执行回调，然后使用 var_dump() 查看有关 Closure 实例的信息，如以下代码片段所示：

```php
$doMd5 = $hashGen->makeHash('md5');
$text  = 'The quick brown fox jumped over the fence';
echo $doMd5($text) . "\n";
var_dump($doMd5);
```

- 6. To end this example, we create and bind an anonymous class to the Closure instance, as illustrated in the following code snippet. Theoretically, the output display should start with Anonymous if the anonymous class were truly bound to $this:
- 6. 为了结束此示例，我们创建一个匿名类并将其绑定到 Closure 实例，如以下代码片段所示。理论上，如果匿名类真正绑定到 $this，输出显示应以 Anonymous 开头：

```php
$temp = new class() { public $class = 'Anonymous: '; };
$doMd5->bindTo($temp);
echo $doMd5($text) . "\n";
var_dump($doMd5);
```

Here is the output of this code example running in PHP 8:

以下是此代码示例在 PHP 8 中运行的输出：

```
root@php8_tips_php8 [ /repo/ch08 ]#
php php7_closure_from_callable.php
HashGen: b335d9cb00b899bc6513ecdbb2187087
object(Closure)#2 (2) {
["this"]=>  object(Services\HashGen)#1 (1) {

["class"]=>  string(9) "HashGen: "
}
["parameter"]=>  array(1) {
    ["$text"]=>  string(10) "<required>"
}
}
PHP Warning:  Cannot bind method Services\HashGen::hashToMd5() to object of class class@anonymous in /repo/ch08/php7_closure_from_callable.php on line 16
HashGen: b335d9cb00b899bc6513ecdbb2187087
object(Closure)#2 (2) {
    ["this"]=> object(Services\HashGen)#1 (1) {
        ["class"]=>  string(9) "HashGen: "
    }
    ["parameter"]=>  array(1) {
        ["$text"]=>  string(10) "<required>"
    }
}

从输出中可以看到，Closure 只是忽略了绑定另一个类的尝试并产生了预期的输出。此外，还生成了一条 Warning 消息，通知你非法绑定尝试。

现在让我们看看注释处理的差异。

## 注释处理的差异

PHP 传统上支持多种符号来表示注释。其中一个符号就是井号（#）。然而，由于引入了名为 Attributes 的新语言结构，井号紧跟在左方括号之后（# [）时不再允许表示注释。井号不紧跟左方括号的情况仍然作为注释分隔符。

下面是一个在 PHP 7 及更早版本中有效，但在 PHP 8 中无效的简短示例：

```
// /repo/ch08/php7_hash_bracket_comment.php
$test = new class() {
    # This works as a comment
    public $works = 'OK';
    #[ This does not work in PHP 8 as a comment]
    public $worksPhp7 = 'OK';
```
};
var_dump($test);

当我们在 PHP 7 中运行此示例时，输出如预期所示，如下所示：

```
root@php8_tips_php7 [ /repo/ch08 ]#
php php7_hash_bracket_comment.php
/repo/ch08/php7_hash_bracket_comment.php:10:
class class@anonymous#1 (2) {
    public $works =>  string(2) "OK"
    public $worksPhp7 =>  string(2) "OK"
}
```

然而，同一个示例在 PHP 8 中会抛出致命的 Error 消息，如下所示：

```
root@php8_tips_php8 [ /repo/ch08 ]#
php php7_hash_bracket_comment.php
PHP Parse error:  syntax error, unexpected identifier "does",
expecting "]" in /repo/ch08/php7_hash_bracket_comment.php on
line 7
```

请注意，如果我们正确地构建了 Attribute 实例，该示例在 PHP 8 中可能偶然能工作。然而，由于所使用的语法与注释的语法一致，因此代码失败了。

现在你已经对从 PHP 8 中移除的函数和用法有了一定的了解，我们现在来检查核心弃用项。

## 检查核心弃用项

在本节中，我们检查在 PHP 8 中已弃用的函数和用法。随着 PHP 语言的不断成熟，PHP 社区能够建议 PHP 核心开发团队移除某些函数、类或甚至语言用法。如果三分之二的 PHP 开发团队投票赞成一项提案，它将被采纳以包含在语言的未来版本中。

对于要移除的功能，它不会立即从语言中删除。相反，函数、类、方法或用法会生成 Deprecation 通知。此通知用作通知开发人员该函数、类、方法或用法将在尚不确定的 PHP 版本中被禁止。因此，你必须密切关注 Deprecation 通知。未能这样做不可避免地会在未来导致代码中断。

> **Tip**
> 
> 从 PHP 5.3 开始，正式的 **Request for Comments (RFC)** 流程启动。任何提案的状态都可以在 https://wiki.php.net/rfc 查看。

让我们先从参数顺序中的弃用用法开始检查。

### 参数顺序中的弃用用法

术语 *usage* 指的是你在应用程序代码中调用函数和类方法的方式。你会发现，在 PHP 8 中，过去允许的旧用法现在被认为是不良实践。了解 PHP 8 如何在代码用法中强制执行最佳实践有助于你编写更好的代码。

如果你定义一个函数或方法，其中包含强制参数和可选参数的混合，大多数 PHP 开发者都同意可选参数应跟随强制参数。在 PHP 8 中，如果未遵循此用法最佳实践，将导致 Deprecation 通知。弃用此用法的理由是为了避免潜在的逻辑错误。

这个简单示例演示了这种用法差异。在下面的示例中，我们定义了一个接受三个参数的简单函数。请注意，$op 可选参数被夹在两个强制参数 $a 和 $b 之间：

```
// /repo/ch08/php7_usage_param_order.php
function math(float $a, string $op = '+', float $b) {
    switch ($op) {
        // not all cases are shown
        case '+' :
        default :
            $out = "$a + $b = " . ($a + $b);
    }
    return $out . "\n";
}
```

如果我们在 PHP 7 中回显加法操作的结果，就不会有问题，如下所示：

```
root@php8_tips_php7 [ /repo/ch08 ]#
php php7_usage_param_order.php
22 + 7 = 29
```

然而在 PHP 8 中，会出现 Deprecation 通知，之后操作仍被允许继续。以下是 PHP 8 中运行的输出：

```
root@php8_tips_php8 [ /repo/ch08 ]#
php php7_usage_param_order.php
PHP Deprecated: Required parameter $b follows optional parameter $op in /repo/ch08/php7_usage_param_order.php on line 4
22 + 7 = 29
```

Deprecation 通知是向开发者发出的信号，表明这种用法被认为是不良实践。在这种情况下，最佳实践是修改函数签名并先列出所有强制参数。

以下是可接受于所有 PHP 版本的重写示例：

```
// /repo/ch08/php8_usage_param_order.php
function math(float $a, float $b, string $op = '+') {
    // remaining code is the same
}
```

需要注意的是，以下用法在 PHP 8 中仍然允许：

```
function test(object $a = null, $b) {}
```

然而，更好的写法是重写相同的函数签名，同时仍遵循先列出强制参数的最佳实践，写法如下：

```
function test(?object $a, $b) {}
```

你现在已经了解了从 PHP 8 核心中移除的功能。让我们现在看看 PHP 8 扩展中移除的功能。

## 处理 PHP 8 扩展中移除的功能

在本节中，我们将看看 PHP 8 扩展中移除的功能。这些信息对于避免编写在 PHP 8 中无法工作的代码至关重要。此外，对移除功能的了解有助于你为 PHP 8 迁移准备现有代码。

下表总结了扩展中移除的功能：

| 扩展 | 移除的函数 | 建议的替代方案 |
|---|---|---|
| Exif | read_exif_data() | exif_read_data() |
| GD | image2wbmp()
 png2wbmp()
 jpeg2wbmp() | 创建图像时按正常方式进行，然后使用 imagewbmp() |
| GMP | gmp_random() | gmp_random_range() 或 gmp_random_bits() |
| Imap | imap_header() | imap_headerinfo() |
| LDAP | ldap_sort() | $res = ldap_search(); and then: usort($res); |
| LDAP | ldap_control_paged_result()
 ldap_control_paged_result_response() | $res = ldap_search(); and then implement pagination using FilterIterator |
| OCI8 | oci_internal_debug()
 ociinternaldebug() | None |
| Zlib | gzgetss() | strip_tags(gzgets()) |

表 8.2 – 从 PHP 8 扩展中移除的函数

上表提供了有用的移除函数列表。在进行 PHP 8 迁移之前，请使用此列表检查现有代码。

现在让我们看看 mbstring 扩展的一个可能严重的更改。

## 发现 mbstring 扩展更改

mbstring 扩展有两个重大更改，具有巨大的向后兼容代码中断潜力。第一个更改是移除了大量便利别名。第二个重大更改是移除了对 mbstring PHP 函数重载能力的支持。让我们先看看移除的别名。

### 处理 mbstring 扩展移除的别名

应一些开发者的要求，负责此扩展的 PHP 开发团队慷慨地创建了一系列别名，将 mb_*() 替换为 mb*()。授予此请求的确切理由已在时间中丢失。然而，支持如此大量别名的负担，每次扩展需要更新时都会浪费大量时间。因此，PHP 开发团队投票决定在 PHP 8 中从 mbstring 扩展中移除这些别名。

下表提供了已移除别名的列表，以及应使用哪个函数来替代：

| 别名 | 改用 |
|---|---|
| `mbereg` | `mb_ereg()` |
| `mbereg_match` | `mb_ereg_match()` |
| `mbereg_replace` | `mb_ereg_replace()` |
| `mbereg_search` | `mb_ereg_search()` |
| `mbereg_search_getpos` | `mb_ereg_search_getpos()` |
| `mbereg_search_getregs` | `mb_ereg_search_getregs()` |
| `mbereg_search_init` | `mb_ereg_search_init()` |
| `mbereg_search_pos` | `mb_ereg_search_pos()` |
| `mbereg_search_regs` | `mb_ereg_search_regs()` |
| `mbereg_search_setpos` | `mb_ereg_search_setpos()` |
| `mberegi` | `mb_eregi()` |
| `mberegi_replace` | `mb_eregi_replace()` |
| `mbregex_encoding` | `mb_regex_encoding()` |
| `mbsplit` | `mb_split()` |

表 8.3 – 已移除的 mbstring 别名

现在让我们看一下字符串处理中的另一个重大变更，涉及函数重载。

## 使用 mbstring 扩展函数重载

函数重载功能允许标准 PHP 字符串函数（例如，`substr()`）在 php.ini 指令 `mbstring.func_overload` 被赋予值时，被静默替换为其 mbstring 扩展等价物（例如，`mb_substr()`）。分配给此指令的值采用位标志的形式。根据此标志的设置，`mail()`、`str*()`、`substr()` 和 `split()` 函数可能会受到重载的影响。此功能已在 PHP 7.2 中弃用，并在 PHP 8 中被移除。

此外，与此功能相关的三个 mbstring 扩展常量也已被移除。这三个常量是 `MB_OVERLOAD_MAIL`、`MB_OVERLOAD_STRING` 和 `MB_OVERLOAD_REGEX`。

> **提示**
> 
> 有关此功能的更多信息，请访问以下链接：
> 
> `https://www.php.net/manual/en/mbstring.overload.php`

任何依赖此功能的代码都将中断。避免严重应用程序故障的唯一方法是重写受影响的代码，并用预期的 mbstring 扩展函数替换被静默替换的 PHP 核心字符串函数。

在下面的示例中，当 mbstring.func_overload 启用时，PHP 7 对 strlen() 和 mb_strlen() 报告相同的值：

```
// /repo/ch08/php7_mbstring_func_overload.php
$str = 'วันนี้สบายดีไหม';
$len1 = strlen($str);
$len2 = mb_strlen($str);
echo "Length of '$str' using 'strlen()' is $len1\n";
echo "Length of '$str' using 'mb_strlen()' is $len2\n";
```

这里是 PHP 7 中的输出：

```
root@php8_tips_php7 [ /repo/ch08 ]#
php php7_mbstring_func_overload.php
Length of 'วันนี้สบายดีไหม' using 'strlen()' is 45
Length of 'วันนี้สบายดีไหม' using 'mb_strlen()' is 15

root@php8_tips_php7 [ /repo/ch08 ]#
echo "mbstring.func_overload=7" >> /etc/php.ini

root@php8_tips_php7 [ /repo/ch08 ]#
php php7_mbstring_func_overload.php
Length of 'วันนี้สบายดีไหม' using 'strlen()' is 15
Length of 'วันนี้สบายดีไหม' using 'mb_strlen()' is 15
```

正如你从前面的输出中看到的，一旦在 php.ini 文件中启用了 mbstring.func_overload 设置，strlen() 和 mb_strlen() 报告的结果是相同的。这是因为对 strlen() 的调用被静默重定向到 mb_strlen()。在 PHP 8 中，输出（未显示）显示了两种情况下的结果，因为 mbstring.func_overload 设置被忽略。strlen() 报告的长度为 45，而 mb_strlen() 报告的长度为 15。

为了确定你的代码是否容易受到此向后兼容性破坏的影响，请检查你的 php.ini 文件，看看 mbstring.func_overload 设置的值是否不为零。

你现在已经知道在哪里查找与 `mbstring` 扩展相关的潜在代码破坏之处。现在，我们将注意力转向 Reflection 扩展的变更。

## 重写使用 Reflection*::export() 的代码

在 Reflection 扩展中，PHP 8 与早期版本之间的一个关键区别是，所有 `Reflection*::export()` 方法都已被移除！此更改的主要原因是，简单地回显 Reflection 对象产生的效果与使用 `export()` 完全相同。

如果你有任何代码当前使用任何 `Reflection*::export()` 方法，你需要重写代码以改用 `__toString()` 方法。

## 发现其他已弃用的 PHP 8 扩展功能

在本节中，我们回顾了 PHP 8 扩展中值得注意的许多其他重要已弃用功能。首先，我们来看 XML-RPC。

## 对 XML-RPC 扩展的更改

在 PHP 8 之前的 PHP 版本中，XML-RPC 扩展是核心的一部分并且始终可用。从 PHP 8 开始，此扩展已悄然移至 **PHP 扩展社区库 (PECL)** (http://pecl.php.net/)，并且不再默认包含在标准 PHP 发行版中。你仍然可以安装并使用此扩展。此更改可以通过扫描 PHP 核心中扩展的列表在此处轻松确认：https://github.com/php/php-src/tree/master/ext。

这不会导致向后兼容的代码破坏。然而，如果你执行标准 PHP 8 安装然后迁移包含对 XML-RPC 的引用的代码，你的代码可能会生成致命的 `Error` 消息并显示 XML-RPC 类和/或函数未定义的提示。在这种情况下，只需使用 `pecl` 或通常用于安装非核心扩展的任何其他方法安装 XML-RPC 扩展即可。

现在我们将注意力转向 DOM 扩展。

## 对 DOM 扩展所做的更改

自 PHP 5 以来，Document Object Model (DOM) 扩展在其源代码存储库中包含了一些从未实现的类。在 PHP 8 中，做出了支持 DOM 作为现行标准（类似于 HTML 5）的决定。现行标准不是具有一系列固定的版本，而是结合了持续的版本发布，以努力跟上网络技术。

> **提示**
> 
> 有关拟议的 DOM 现行标准的更多信息，请查看此参考：https://dom.spec.whatwg.org/。有关将 PHP DOM 扩展迁移到现行标准基础的良好讨论，请查看 *第 9 章，掌握 PHP 8 最佳实践* 中的 *使用接口和特征* 部分。

主要由于向现行标准的转变，以下未实现的类已于 PHP 8 起从 DOM 扩展中移除：

- `DOMNameList`
- `DOMImplementationList`
- `DOMConfiguration`
- `DOMError`
- `DOMErrorHandler`
- `DOMImplementationSource`
- `DOMLocator`
- `DOMUserDataHandler`
- `DOMTypeInfo`

这些类从未实现，这意味着你的源代码不会遭受任何向后兼容性破坏。

现在让我们看一下 PHP PostgreSQL 扩展中的弃用项。

## 对 PostgreSQL 扩展所做的更改

除了表 8.5 中指出的已弃用功能 – PHP 8 扩展中的已弃用功能（稍后显示）之外，你需要知道在 PHP 8 PostgreSQL 扩展中已弃用了几十个别名。与从 mbstring 扩展中移除的别名一样，我们在本节中介绍的别名在别名名称的后半部分没有下划线字符。

本表总结了已移除的别名，以及应调用的函数：

| 别名 | 改用 | 别名 | 改用 |
|---|---|---|---|
| pg_errormessage() | pg_last_error() | pg_getlastoid() | pg_last_oid() |
| pg_numrows() | pg_num_rows() | pg_locreate() | pg_lo_create() |
| pg_numfields() | pg_num_fields() | pg_lounlink() | pg_lo_unlink() |
| pg_cmdtuples() | pg_affected_rows() | pg_loopen() | pg_lo_open() |
| pg_fieldname() | pg_field_name() | pg_loclose() | pg_lo_close() |
| pg_fieldsize() | pg_field_size() | pg_loread() | pg_lo_read() |
| pg_fieldtype() | pg_field_type() | pg_lowrite() | pg_lo_write() |
| pg_fieldnum() | pg_field_num() | pg_loreadall() | pg_lo_read_all() |
| pg_result() | pg_fetch_result() | pg_loimport() | pg_lo_import() |
| pg_fieldprlen() | pg_field_prtlen() | pg_loexport() | pg_lo_export() |
| pg_fieldisnull() | pg_field_is_null() | pg_setclientencoding() | pg_set_client_encoding() |
| pg_freeresult() | pg_free_result() | pg_clientencoding() | pg_client_encoding() |

表 8.4 – PostgreSQL 扩展中的已弃用功能

请注意，很难找到有关弃用的文档。在这种情况下，你可以查阅此处的 PHP 7.4 到 PHP 8 迁移指南：https://www.php.net/manual/en/migration80.deprecated.php#migration80.deprecated.pgsql。否则，你始终可以在此处的 C 源代码 docblocks 中查找 @deprecation 注释：https://github.com/php/php-src/blob/master/ext/pgsql/pgsql.stub.php。这里有一个示例：

```
/**
 * @alias pg_last_error
 * @deprecated
 */
function pg_errormessage(
    ?PgSql\Connection $connection = null): string {}
```

在本节的最后一部分，我们总结了 PHP 8 扩展中的已弃用功能。



## PHP 8 扩展中的已弃用功能

最后，为了让您更容易识别 PHP 8 扩展中的已弃用功能，我们提供了一个汇总。下表汇总了 PHP 8 扩展中已弃用的功能：

| 扩展 | 已弃用功能 | 建议的替代方案 |
|---|---|---|
| Enchant | enchant_dict_add_to_personal()
enchant_dict_is_in_session()
enchant_broker_free()
enchant_broker_free_dict()
enchant_broker_set_dict_path()
enchant_broker_get_dict_path() | enchant_dict_add()
enchant_dict_is_added()
unset ($enchantObject)
(相同)
无：不再支持
(相同) |
| OCI8 | OCI-Lob
OCI-Collection | OCILob
OCICollection |
| ODBC | odbc_connect()
odbc_exec() | 不再重用连接
flags 参数已移除 |
| PostgreSQL | PGSQL_LIBPQ_VERSION_STR | PGSQL_LIBPQ_VERSION |
|  | pg_connect() | 现在必须使用连接字符串 |
|  | pg_lo_import(), pg_lo_export() | 第一个参数现在是连接 |
|  | pg_fetch_all() | 现在如果没有结果则返回空数组 |
| Reflection | ReflectionParameter::getClass()
ReflectionParameter::isArray()
ReflectionParameter::isCallable()
ReflectionParameter::isDisabled() | ReflectionParameter::getType()
(相同)
(相同)
无 |
| LibXML | libxml_disable_entity_loader() | 无：不再需要 |

表 8.5 – PHP 8 扩展中的已弃用功能

我们将使用 PostgreSQL 扩展来说明已弃用功能。在运行代码示例之前，您需要在 PHP 8 Docker 容器内进行少量设置。请按以下步骤操作：

- 1. 打开 PHP 8 Docker 容器的命令 shell。从命令 shell 使用以下命令启动 PostgreSQL：
- 2. 接下来，切换到 su postgres 用户。
- 3. 提示符更改为 bash-4.3$。从这里，输入 psql 以进入 PostgreSQL 交互式终端。
- 4. 接下来，从 PostgreSQL 交互式终端发出以下命令集以创建并填充示例数据库表：

/etc/init.d/postgresql start

```
CREATE DATABASE php8_tips;
\c php8_tips;
\i /repo/sample_data/pgsql_users_create.sql
```

5. 以下是整个命令链的重放：

```
root@php8_tips_php8 [ /repo/ch08 ]# su postgres
bash-4.3$ psql
psql (10.2)
Type "help" for help.
postgres=# CREATE DATABASE php8_tips;
CREATE DATABASE
postgres=# \c php8_tips;
You are now connected to database "php8_tips"
    as user "postgres".
php8_tips=# \i /repo/sample_data/pgsql_users_create.sql
CREATE TABLE
INSERT 0 4
CREATE ROLE
GRANT
php8_tips=# \q
bash-4.3$ exit
exit
root@php8_tips_php8 [ /repo/ch08 ]#
```

6. 我们现在定义一个简短的代码示例来说明刚刚讨论的弃用概念。请注意，在以下代码示例中，我们为不存在的用户创建了一个 **结构化查询语言 (SQL)** 语句：

// /repo/ch08/php8_pgsql_changes.php
$usr = 'php8';
$pwd = 'password';
$dsn = 'host=localhost port=5432 dbname=php8_tips '
    . ' user=php8 password=password';
$db  = pg_connect($dsn);
$sql = "SELECT * FROM users WHERE user_name='joe'";
$stmt = pg_query($db, $sql);
echo pg_errormessage();
$result = pg_fetch_all($stmt);
var_dump($result);
```

7. 以下是前述代码示例的输出：

```
root@php8_tips_php8 [ /repo/ch08 ]#
php php8_pgsql_changes.php
Deprecated: Function pg_errormessage() is
deprecated in /repo/ch08/php8_pgsql_changes.php on
line 22
array(0) {}
```

输出中需要注意的两点主要事实是 pg_errormessage() 已被弃用，并且当查询未返回结果时，不是返回 FALSE 布尔值，而是返回空数组。不要忘记使用以下命令停止 PostgreSQL 数据库：

/etc/init.d/postgresql stop

现在您对各种 PHP 8 扩展中的已弃用功能有了一定的了解，我们将注意力转向与安全相关的弃用。

## 处理已弃用或移除的安全相关功能

对功能造成影响的任何更改都极其重要。忽略这些更改很容易不仅导致代码中断，还可能使您的网站暴露给潜在攻击者。在本节中，我们将介绍 PHP 8 中存在的一系列与安全相关的功能更改。让我们从检查过滤器开始讨论。

## 检查 PHP 8 流过滤器更改

PHP 输入/输出 (I/O) 操作依赖于一个名为流的子系统。这种架构的一个有趣方面是能够向任何给定流附加流过滤器。您可以附加的过滤器可以是使用 stream_filter_register() 注册的自定义定义的流过滤器，也可以是随 PHP 安装一起提供的预定义过滤器。

您需要了解的一个重要更改是，在 PHP 8 中，所有 mcrypt.* 和 mdecrypt.* 过滤器已被移除，以及 string.strip_tags 过滤器。如果您不确定 PHP 安装中包含哪些过滤器，您可以运行 phpinfo()，或者更好的是，使用 stream_get_filters()。

以下是使用本书的 PHP 7 Docker 容器运行的 stream_get_filters() 输出：

```
root@php8_tips_php7 [ /repo/ch08 ]#
php -r "print_r(stream_get_filters());"
Array (
    [0] => zlib.*
    [1] => bzip2.*
    [2] => convert.iconv.*
    [3] => mcrypt.*
    [4] => mdecrypt.*
    [5] => string.rot13
    [6] => string.toupper
    [7] => string.tolower
    [8] => string.strip_tags
    [9] => convert.*
    [10] => consumed
    [11] => dechunk
)
```

以下是同一命令在 PHP 8 Docker 容器中运行的情况：

```
root@php8_tips_php8 [ /repo/ch08 ]#
php -r "print_r(stream_get_filters());"
Array (
    [0] => zlib.*
    [1] => bzip2.*
    [2] => convert.iconv.*
    [3] => string.rot13
    [4] => string.toupper
    [5] => string.tolower
    [6] => convert.*
    [7] => consumed
    [8] => dechunk
)
```

您会注意到从 PHP 8 的输出中，前面提到的过滤器都已被移除。任何使用所列三个过滤器中的任何一个的代码在 PHP 8 迁移后都会中断。我们现在来看自定义错误处理的更改。

## 处理自定义错误处理更改

从 PHP 7.0 开始，大多数错误现在都会 **抛出**。例外情况是 PHP 引擎不知道存在错误条件的情况，例如内存不足、超出时间限制或发生段错误。另一个例外是程序故意使用 `trigger_error()` 函数触发错误时。

使用 `trigger_error()` 函数捕获错误不是最佳实践。*最佳实践* 是开发面向对象的代码并将其放在 try/catch 结构中。然而，如果您被分配管理一个确实使用这种做法的应用程序，则传递给自定义错误处理程序的内容发生了变化。

在 PHP 8 之前的版本中，传递给自定义错误处理程序第五个参数 `$errorcontext` 的数据是有关传递给函数参数的信息。在 PHP 8 中，此参数被忽略。为了说明差异，请查看接下来显示的简单代码示例。以下是导致此的步骤：

- 1. 首先，我们定义一个自定义错误处理程序，如下所示：

// /repo/ch08/php7_error_handler.php
function handler($errno, $errstr, $errfile,
    $errline, $errorcontext = NULL) {
    echo "Number  : $errno\n";
    echo "String  : $errstr\n";
    echo "File    : $errfile\n";
    echo "Line    : $errline\n";
    if (!empty($errorcontext))
        echo "Context: \n"
            . var_export($errorcontext, TRUE);
    exit;
}
```

2. 然后我们定义一个触发错误、设置错误处理程序并调用该函数的函数，如下所示：

```
function level1($a, $b, $c) {
    trigger_error("This is an error", E_USER_ERROR);
}
set_error_handler('handler');
echo level1(TRUE, 222, 'C');
```

以下是 PHP 7 中运行的输出：

```
root@php8_tips_php7 [ /repo/ch08 ]#
php php7_error_handler.php
Number : 256
String : This is an error
File   : /repo/ch08/php7_error_handler.php
Line   : 17
Context:
array (
  'a' => true,
  'b' => 222,
  'c' => 'C',
)
```

正如前面的输出所示，$errorcontext 提供了有关函数接收到的参数的信息。相比之下，请查看 PHP 8 产生的输出，如下所示：

```
root@php8_tips_php8 [ /repo/ch08 ]#
php php7_error_handler.php
Number : 256
String : This is an error
File   : /repo/ch08/php7_error_handler.php
Line   : 17
```

正如您所见，输出是相同的，只是缺少传入 $errorcontext 的信息。现在让我们来看一下生成回溯。



## Dealing with changes to backtraces

令人惊讶的是，在 PHP 8 之前，可以通过回溯更改函数参数。这之所以可能，是因为由 debug_backtrace() 或 Exception::getTrace() 生成的跟踪通过引用提供了对函数参数的访问。

这是一种极其糟糕的做法，因为它允许程序在可能处于错误状态的情况下继续运行。此外，在审查此类代码时，参数数据是如何提供的并不清楚。因此，在 PHP 8 中，不再允许这种做法。debug_backtrace() 或 Exception::getTrace() 仍然像以前一样运行。唯一的区别是它们不再通过引用传递参数变量。

现在让我们来看看 PDO 错误处理的更改。

## PDO error-handling mode default changed

多年来，当使用 PDO 扩展的数据库应用程序未能产生结果时，初级 PHP 开发者感到困惑。在许多情况下，此问题的原因是未报告的简单 SQL 语法错误。这是由于在 PHP 8 之前的版本中，默认的 PDO 错误模式为 PDO::ERRMODE_SILENT。

SQL 错误不是 PHP 错误。因此，此类错误不会被正常的 PHP 错误处理捕获。相反，PHP 开发者必须专门将 PDO 错误模式设置为 PDO::ERRMODE_WARNING 或 PDO::ERRMODE_EXCEPTION。PHP 开发者现在可以松一口气了，因为从 PHP 8 开始，PDO 默认错误处理模式现在是 PDO::ERRMODE_EXCEPTION。

在下面的示例中，PHP 7 允许不正确的 SQL 语句静默失败：

```php
// /repo/ch08/php7_pdo_err_mode.php
$dsn = 'mysql:host=localhost;dbname=php8_tips';
$pdo = new PDO($dsn, 'php8', 'password');
$sql = 'SELECT propertyKey, hotelName FROM hotels '
    ."WHERE country = 'CA'";
$stmt = $pdo->query($sql);
if ($stmt)
    while($hotel = $stmt->fetch(PDO::FETCH_OBJ))
        echo $hotel->hotelName . ' ' . $hotel->propertyKey . "\n";
else
    echo "No Results\n";
```

在 PHP 7 中，唯一输出是 No Results，这既具有误导性又没有帮助。它可能会导致开发者认为没有结果，而实际上问题是 SQL 语法错误。

此处显示的在 PHP 8 中运行的输出更有帮助：

```
root@php8_tips_php8 [ /repo/ch08 ]# php php7_pdo_err_mode.php
PHP Fatal error: Uncaught PDOException: SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near 'SELECT propertyKey, hotelName FROM hotels WHERE country = 'CA'' at line 1 in /repo/ch08/php7_pdo_err_mode.php:10
```

从前面的 PHP 8 输出中可以看到，实际问题已得到明确识别。

> TIP
> 有关此更改的更多信息，请参阅此 RFC：
> https://wiki.php.net/rfc/pdo_default_errmode

接下来我们检查 track_errors php.ini 指令。

## Examining the track_errors php.ini setting

从 PHP 8 开始，track_errors php.ini 指令已被移除。这意味着自动创建的变量 $php_errormsg 不再可用。在大多数情况下，PHP 8 之前导致错误的东西现在已被转换为抛出 Error 消息。但是，对于 PHP 8 之前的版本，您仍然可以使用 error_get_last() 函数代替。

在下面的简单代码示例中，我们首先打开 track_errors 指令。然后我们在没有任何参数的情况下调用 strpos()，故意导致错误。然后我们依赖 $php_errormsg 来揭示真正的错误：

```
// /repo/ch08/php7_track_errors.php
ini_set('track_errors', 1);
@strpos();
echo $php_errormsg . "\n";
echo "OK\n";
```

以下是 PHP 7 中的输出：

```
root@php8_tips_php7 [ /repo/ch08 ]# php php7_track_errors.php
strpos() expects at least 2 parameters, 0 given
OK
```

从前面的输出可以看到，$php_errormsg 揭示了错误，并且代码块被允许继续。在 PHP 8 中，当然我们不允许在没有任何参数的情况下调用 strpos()。以下是输出：

```
root@php8_tips_php8 [ /repo/ch08 ]# php php7_track_errors.php
PHP Fatal error: Uncaught ArgumentCountError: strpos() expects at least 2 arguments, 0 given in /repo/ch08/php7_track_errors.php:5
```

正如您所见，PHP 8 抛出 Error 消息。最佳做法是使用 try/catch 块并捕获可能抛出的任何 Error 消息。您也可以使用 error_get_last() 函数。以下是重写后的示例，可在 PHP 7 和 PHP 8 中运行（未显示输出）：

```
// /repo/ch08/php8_track_errors.php
try {
    strpos();
    echo error_get_last()['message'];
    echo "\nOK\n";
} catch (Error $e) {
    echo $e->getMessage() . "\n";
}
```

您现在对 PHP 8 中已弃用或移除的功能有了一些了解。本章到此结束。

## Summary

在本章中，您了解了已弃用和移除的 PHP 功能。本章的第一部分涉及已移除的核心功能。更改的理由已得到解释，您了解到本章中描述的功能被移除的主要原因不仅是为了让您转向遵循最佳实践的代码，而且是为了让您使用更快、更高效的 PHP 8 功能。

在下一部分，您了解了已弃用的功能。本节的关键主题是强调已弃用的函数、类和方法如何导致不良实践和漏洞百出的代码。您还获得了有关在许多关键 PHP 8 扩展中已被移除或弃用的功能的指导。

您学习了如何定位和重写已弃用的代码，以及如何为已移除的功能开发变通方法。本章中学习的另一项技能包括如何重构使用涉及扩展的已移除功能的代码，最后但并非最不重要的是，您学习了通过重写依赖于已移除函数的代码来提高应用程序安全性。

在下一章中，您将通过掌握最佳实践来学习如何在 PHP 8 代码中获得更高的效率和性能。

# Section 3: PHP 8 Best Practices

在本节中，您将了解 PHP 8 最佳实践。您将了解 PHP 8 如何引入了许多额外的控制来强制执行某些最佳实践。此外，还涵盖了在 PHP 8 中编写代码的最佳方式。

本节包含以下章节：

- *第 9 章，掌握 PHP 8 最佳实践*
- *第 10 章，改进性能*
- *第 11 章，将现有 PHP 应用迁移到 PHP 8*
- *第 12 章，使用异步编程创建 PHP 8 应用程序*
