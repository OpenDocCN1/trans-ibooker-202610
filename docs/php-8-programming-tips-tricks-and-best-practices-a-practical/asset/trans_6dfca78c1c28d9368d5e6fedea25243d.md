
## 避免更新 mbstring 扩展时出现的问题

mbstring 扩展最早在 PHP 4 中引入，此后一直是该语言的活跃组成部分。该扩展最初的目的是提供对各种日文字符编码系统的支持。此后，增加了对多种其他编码的支持——最显著的是基于 **通用编码字符集 2 (UCS-2)**、**UCS-4**、**Unicode 转换格式 8 (UTF-8)**、**UTF-16**、**UTF-32**、**Shift_JIS 日本工业标准 (SJIS)** 和 **国际标准化组织 8859 (ISO-8859)** 等的编码支持。

如果不确定服务器支持哪些编码，只需按如下方式运行 `mb_list_encodings()` 命令（显示部分输出）：

```
root@php8_tips_php7 [ /repo/ch07 ]#
php -r "var_dump(mb_list_encodings());"
Command line code:1:
array(87) {
    ... only selected output is shown ...
    [14] => string(7) "UCS-4BE"
    [16] => string(5) "UCS-2"
    [19] => string(6) "UTF-32"
    [22] => string(6) "UTF-16"
    [25] => string(5) "UTF-8"
    [26] => string(5) "UTF-7"
    [27] => string(9) "UTF7-IMAP"
    [28] => string(5) "ASCII"
    [29] => string(6) "EUC-JP"
    [30] => string(4) "SJIS"
    [31] => string(9) "eucJP-win"
    [32] => string(11) "EUC-JP-2004"
    [76] => string(6) "KOI8-R"
    [78] => string(9) "ArmSCII-8"
    [79] => string(5) "CP850"
    [80] => string(6) "JIS-ms"
    [81] => string(16) "ISO-2022-JP-2004"
    [86] => string(7) "CP50222"
}
```

如前面的输出所示，我们在书中使用的 PHP 7.1 Docker 容器支持 87 种编码。在 PHP 8.0 Docker 容器中（未显示输出），支持 80 种编码。现在让我们看看 PHP 8 中引入的更改，从 mb_str*() 函数开始。

### 发现 mb_str*() 函数中 needle 参数的差异

在第 6 章《理解 PHP 8 功能差异》中，你了解到 PHP 8 对核心 str*pos()、str*str() 和 str*chr() 函数的 needle 参数处理引入了更改。两个主要的 needle 参数差异是能够接受空的 needle 参数以及严格的类型检查以确保 needle 参数仅为字符串。为了保持一致性，PHP 8 在对应的 mb_str*() 函数中引入了相同的更改。

首先让我们先查看空 needle 参数的处理。

### mb_str*() 函数的空 needle 参数处理

为了使 mbstring 扩展与核心字符串函数的这一更改保持一致，以下 mbstring 扩展函数现在允许空的 needle 参数。需要注意的是，这并不意味着参数可以省略或为可选！此更改的含义是，作为 needle 参数提供的值现在也可以包括被视为空的内容。快速了解 PHP 认为为空的内容的好方法可以在 empty() 函数的文档中找到（https://www.php.net/empty）。以下是现在允许空 needle 参数值的 mbstring 函数列表：

- `mb_strpos()`
- `mb_strrpos()`
- `mb_stripos()`
- `mb_strripos()`
- `mb_strstr()`
- `mb_stristr()`
- `mb_strrchr()`
- `mb_strrichr()`

> 提示
> 
> 这里提到的八个 mbstring 扩展函数各自与其核心 PHP 对应函数完全平行。有关这些函数的更多信息，请查看此参考文档：https://www.php.net/manual/en/ref.mbstring.php。

下面简短的代码示例说明了上述八个函数中的空 needle 处理。以下是导致此的步骤：

1. 首先，我们初始化一个多字节文本字符串。在以下示例中，这是 *敏捷的棕色狐狸跳过了栅栏* 的泰语翻译。needle 参数设为 NULL，并初始化要测试的函数数组：

```php
// /repo/ch07/php8_mb_string_empty_needle.php
$text   = 'สุนัขจิ้งจอกสีน้ำตาลกระโดดข้ามรั้วอย่างรวดเร็ว';
$needle = NULL;
$funcs  = ['mb_strpos',  'mb_strrpos', 'mb_stripos',
           'mb_strripos', 'mb_strstr', 'mb_stristr',
           'mb_strrchr',  'mb_strrichr'];
```

2. 然后我们定义一个 printf() 模式，并循环遍历要测试的函数。对每个函数调用，我们提供文本后跟空的 needle 参数，如下所示：

```php
$patt = "Testing: %12s : %s\n";
foreach ($funcs as $str)
    printf($patt, $str, $str($text, $needle));
```

PHP 7 的输出如下所示：

```
root@php8_tips_php7 [ /repo/ch07 ]#
php php8_mb_string_empty_needle.php
PHP Warning:  mb_strpos(): Empty delimiter in /repo/ch07/php8_mb_string_empty_needle.php on line 12
Testing:      mb_strpos :
Testing:      mb_strrpos :
PHP Warning:  mb_stripos(): Empty delimiter in /repo/ch07/php8_mb_string_empty_needle.php on line 12
Testing:      mb_stripos :
Testing:      mb_strripos :
```

```
PHP Warning:  mb_strstr(): Empty delimiter in /repo/ch07/php8_mb_string_empty_needle.php on line 12
Testing:      mb_strstr :
PHP Warning:  mb_stristr(): Empty delimiter in /repo/ch07/php8_mb_string_empty_needle.php on line 12
Testing:      mb_stristr :
Testing:      mb_strrchr :
Testing:      mb_strrichr :
```

如你所见，输出为空，并且在某些情况下会发出 Warning 消息。PHP 8 中的输出截然不同，如预期所示，如下所示：

```
root@php8_tips_php8 [ /repo/ch07 ]#
php php8_mb_string_empty_needle.php
Testing:      mb_strpos : 0
Testing:      mb_strrpos : 46
Testing:      mb_stripos : 0
Testing:      mb_strripos : 46
Testing:      mb_strstr : สุนัขจึงอกสั่นน้ำตาลกระโดดข้ามรั้วอย่างรวดเร็ว
Testing:      mb_stristr : สุนัขจึงอกสั่นน้ำตาลกระโดดข้ามรั้วอย่างรวดเร็ว
Testing:      mb_strrchr :
Testing:      mb_strrichr :
```

有趣的是，当此代码在 PHP 8 中运行时，空的 needle 参数对于 mb_strpos() 和 mb_stripos() 返回整数 0 的值，对于 mb_strrpos() 和 mb_strripos() 返回整数 46。在 PHP 8 中，空的 needle 参数在此情况下被解释为字符串的开头或结尾。mb_strstr() 和 mb_stristr() 的结果均为整个字符串。

### mb_str*() 函数的数据类型检查

为了与核心 str*() 函数保持一致，对应的 mb_str*() 函数中的 needle 参数必须是字符串类型。如果提供 American Standard Code for Information Interchange (ASCII) 值而不是字符串，受影响的函数现在将抛出 ArgumentTypeError 错误。本小节未显示示例，因为第 6 章《理解 PHP 8 功能差异》已在核心 str*() 函数中提供了此差异的示例。

### mb_strrpos() 的差异

在 PHP 的早期版本中，允许将字符编码作为第三个参数传递给 mb_strrpos() 而不是偏移量。这种不良做法在 PHP 8 中不再受支持。相反，你可以提供 0 作为第三个参数，或者考虑使用 PHP 8 的命名参数（在第 1 章《Introducing New PHP 8 OOP Features》中的“Understanding named arguments”部分讨论）以避免必须为可选参数提供值。

现在让我们看一个代码示例，演示 PHP 7 和 PHP 8 之间处理上的差异。按如下方式操作：

- 1. 我们首先定义一个常量来表示要使用的字符编码。分配一个代表敏捷的棕色狐狸跳过了栅栏的泰语翻译的文本字符串。然后我们使用 mb_convert_encoding() 确保使用正确的编码。代码在以下片段中说明：
- 2. 然后我们将栅栏的泰语翻译分配给 $needle 并输出字符串的长度和 $needle 在文本中的位置。然后我们调用 mb_strrpos() 查找 $needle 的最后一次出现。请注意，在以下代码片段中，我们故意遵循使用编码作为第三个参数而不是偏移量的不良做法：

```php
// /repo/ch07/php7_mb_string_strpos.php
define('ENCODING', 'UTF-8');
$text    = 'สุนัขจิ้งจอกสีน้ำตาลกระโดดข้ามรั้วอย่างรวดเร็ว';
$encoded = mb_convert_encoding($text, ENCODING);
```

```php
$needle  = 'รั้ว';
echo 'String Length: '
    . mb_strlen($encoded, ENCODING) . "\n";
echo 'Substring Pos: '
    . mb_strrpos($encoded, $needle, ENCODING) . "\n";
```

此代码示例的输出在 PHP 7 中完美运行，如下所示：

```
root@php8_tips_php7 [ /repo/ch07 ]#
php php7_mb_string_strpos.php
String Length: 46
Substring Pos: 30
```
