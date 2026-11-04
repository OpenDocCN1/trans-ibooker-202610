
然而在 PHP 7 中，此做法被禁止并会发出 Warning，如下所示：

```
PHP Warning: Cannot assign an empty string to a string offset in /repo/ch03/php8_error_str_empty.php on line 5
ABCDEFGHIJKLMNOPQRSTUVWXYZ
```

从前面的输出可以看出，脚本仍被允许执行；然而，尝试删除字母 F 并未成功。正如我们所讨论的，在 PHP 8 中，Warning 被提升为 Error，整个脚本会中止，如下所示：

```
PHP Fatal error: Uncaught Error: Cannot assign an empty string to a string offset in /repo/ch03/php8_error_str_empty.php:5
```

接下来我们将考察在 PHP 8 中哪些原先的 Notices 被提升为 Warnings 的情况。

## 理解被提升为警告的通知

在 PHP 7 之前的版本中，有一些情况被认为对 PHP 引擎运行时的稳定性影响较小而被低估。不幸的是，新手（或也许是懒惰的！）PHP 开发者在急于将代码投入生产时，往往习惯于直接忽略 Notices。

多年来 PHP 标准已大幅收紧，导致 PHP 核心团队将某些错误条件从 Notice 升级为 Warning。这两种错误报告级别都不会导致代码停止工作。然而，PHP 核心团队认为将 Notice 提升为 Warning 会使不良编程实践更加明显。Warnings 更不可能被忽略，最终有助于代码质量提升。

以下是一些在早期 PHP 版本中会导致发出 Notice 的错误条件，而在 PHP 8 中同样的条件现在会生成 Warning 的简要列表：

- 尝试访问不存在的对象属性
- 尝试访问不存在的静态属性
- 使用不存在的键尝试访问数组元素
- 误用资源作为数组偏移量
- 模糊的字符串偏移量转换
- 不存在或未初始化的字符串偏移量

让我们先来看一下涉及对象的情况中的 Notice 提升。

### 不存在对象属性访问的处理

在早期版本的 PHP 中，尝试访问不存在的属性时会发出 Notice。唯一的例外是自定义类中定义了魔术方法 __get() 和/或 __set() 的情况。

在下面的代码示例中，我们定义了一个 Test 类，其中包含两个属性，其中一个被标记为静态：

// /repo/ch03/php8_warn_undef_prop.php
class Test {
    public static $stat = 'STATIC';
    public $exists = 'NORMAL';
}
$obj = new Test();
```

然后我们尝试输出已存在和不存在的属性，如下所示：

```
echo $obj->exists;
echo $obj->does_not_exist;
```

不出所料，在 PHP 7 中尝试输出不存在的属性时，输出会返回一个 Notice，如下所示：

```
NORMAL
PHP Notice:  Undefined property: Test::$does_not_exist in
/repo/ch03/php8_warn_undef_prop.php on line 14
```

同一份代码文件在 PHP 8 中现在会返回一个 Warning，如下所示：

```
NORMAL
PHP Warning:  Undefined property: Test::$does_not_exist in /repo/ch03/php8_warn_undef_prop.php on line 14
```

**重要说明**

Test::$does_not_exist 错误信息并不意味着我们尝试了静态访问。它仅仅表示 $does_not_exist 属性与 Test 类相关联。

现在我们添加尝试访问不存在的静态属性的代码行，如下所示：

```
try {
    echo Test::$stat;
    echo Test::$does_not_exist;
} catch (Error $e) {
    echo __LINE__ . ':' . $e;
}
```

有趣的是，PHP 7 和 PHP 8 现在都会发出致命错误，如下所示：

```
STATIC
22:Error: Access to undeclared static property Test::$does_not_exist in /repo/ch03/php8_warn_undef_prop.php:20
```

任何原本会发出 Warning 的代码块现在发出 Error 都值得关注。如果可能，请扫描代码中对静态类属性的静态引用并确保它们已定义。否则，在升级到 PHP 8 后，你的代码将失败。

现在让我们来看一下不存在偏移量的处理。

### 不存在偏移量的处理

如前一节所述，通常情况下，读取数据时 Notices 被提升为 Warnings，而写入数据时 Warnings 被提升为 Errors（这可能导致数据丢失）。不存在偏移量的处理遵循这一逻辑。

在下面的示例中，数组键取自一个字符串。在这两种情况下，偏移量都不存在：

// /repo/ch03/php8_warn_undef_array_key.php
$key = 'ABCDEF';
$vals = ['A' => 111, 'B' => 222, 'C' => 333];
echo $vals[$key[6]];
```

在 PHP 7 中，结果是一个 Notice，如下所示：

```
PHP Notice: Uninitialized string offset: 6 in /repo/ch03/php8_warn_undef_array_key.php on line 6
PHP Notice: Undefined index:  in /repo/ch03/php8_warn_undef_array_key.php on line 6
```

在 PHP 8 中，结果是一个 Warning，如下所示：

```
PHP Warning: Uninitialized string offset 6 in /repo/ch03/php8_warn_undef_array_key.php on line 6
PHP Warning: Undefined array key "" in /repo/ch03/php8_warn_undef_array_key.php on line 6
```

此示例进一步说明了 PHP 8 错误处理增强的一般原理：如果你的代码向不存在的偏移量写入数据，以前是 Warning 的情况在 PHP 8 中变为 Error。前面的输出显示了在 PHP 8 中尝试从不存在的偏移量读取数据时，现在会发出 Warning。接下来要检查的 Notice 提升涉及资源 ID 的误用。

### 误用资源 ID 作为数组偏移量

在创建与应用程序代码外部服务的连接时，会生成一个资源。这种数据类型的经典示例是文件句柄。在下面的代码示例中，我们打开一个文件句柄（从而创建资源）指向 gettysburg.txt 文件：

```php
// /repo/ch03/php8_warn_resource_offset.php
$fn = __DIR__ . '../sample_data/gettysburg.txt';
$fh = fopen($fn, 'r');
echo $fh . "\n";
```

请注意，我们在最后一行直接输出资源。这会显示资源 ID 编号。然而，如果我们现在尝试将资源 ID 用作数组偏移量，PHP 7 会生成一个 Notice，如下所示：

```
Resource id #5
PHP Notice: Resource ID#5 used as offset, casting to integer (5) in /repo/ch03/php8_warn_resource_offset.php on line 9
```

PHP 8 如预期会生成一个 Warning，如下所示：

```
Resource id #5
PHP Warning: Resource ID#5 used as offset, casting to integer (5) in /repo/ch03/php8_warn_resource_offset.php on line 9
```

请注意，在 PHP 8 中，许多以前产生资源 的函数现在改为产生对象。此主题在 *Chapter 7, Avoiding Traps When Using PHP 8 Extensions* 中有所涵盖。

> Tip
> **Best practice**: Do not use resource IDs as array offsets!

现在我们将注意力转向在模糊字符串偏移量情况下被提升为 Warnings 的与字符串相关的 Notices。

### 模糊的字符串偏移量转换

将注意力转向字符串处理，我们再次回顾使用数组语法识别字符串中单个字符的想法。如果 PHP 需要执行内部类型转换来评估字符串偏移量，但该类型转换的位置不明确，则可能会发生 **ambiguous string offset cast**。

在这个非常简单的示例中，我们定义一个包含字母表所有字母的字符串。然后我们定义一个键数组，其值分别为：NULL；一个布尔值 TRUE；以及一个浮点数 22/7（圆周率的近似值）。然后我们遍历这些键并尝试将键用作字符串偏移量，如下所示：

```php
// /repo/ch03/php8_warn_amb_offset.php
$str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
$ptr = [ NULL, TRUE, 22/7 ];
foreach ($ptr as $key) {
    var_dump($key);
    echo $str[$key];
}
```

正如你可能预料到的，在 PHP 7 中运行的输出会产生 A、B 和 D，以及一系列 Notices，如下所示：

```
NULL
PHP Notice: String offset cast occurred in /repo/ch03/php8_warn_amb_offset.php on line 8
A
/repo/ch03/php8_warn_amb_offset.php:7:
bool(true)
PHP Notice: String offset cast occurred in /repo/ch03/php8_
```
