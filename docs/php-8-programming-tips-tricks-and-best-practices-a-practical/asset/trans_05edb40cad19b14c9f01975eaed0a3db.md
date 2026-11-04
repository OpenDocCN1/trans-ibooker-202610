
## 错误级别默认值

有用的是注意，分配给 php.ini 文件 error_reporting 指令的错误级别默认值在 PHP 8 中已更新。在 PHP 7 中，默认的 error_reporting 级别如下：

```
error_reporting=E_ALL & ~E_NOTICE & ~E_STRICT & ~E_DEPRECATED
```

在 PHP 8 中，新的级别要简单得多，如下所示：

```
error_reporting=E_ALL
```

还值得注意的是，php.ini 文件设置 display_startup_errors 现在默认已启用。这对于生产服务器可能是一个问题，因为您的网站在 PHP 启动时可能会无意中泄露错误信息。

本节的关键要点是，过去 PHP 仅通过发出 Notices 或 Warnings 就允许你逃脱某些不良实践。然而，正如本节所述，未解决 Warning 或 Notice 生成背后的问题的危险在于 PHP 默默地代表你采取的行动。不依赖 PHP 代替你做决定可以减少隐藏的逻辑错误。遵循良好的编码实践，例如在使用变量之前为所有变量分配默认值，有助于避免此类错误。现在让我们更仔细地看看 PHP 8 中 Warnings 被提升为 Errors 的错误情况。

## 现在为错误的警告的处理

本节我们将介绍与对象、数组和字符串相关的升级后的 PHP 8 错误处理。我们还将检查过去 PHP 发出 Warning 但 PHP 8 现在抛出 Error 的情况。了解本节中讨论的任何潜在错误情况至关重要。原因很简单：如果未能解决本节所述的情况，在服务器升级到 PHP 8 时你的代码将会中断。

开发者经常面临时间压力。可能是有大量新功能或其他变更必须完成。在其他情况下，资源被抽调到其他项目，导致可用于维护的开发者更少。Warnings 通常被忽略，因为应用程序仍在运行，因此许多开发者只是关闭错误显示并希望一切顺利。

多年来，堆积如山的糟糕代码不断累积。不幸的是，PHP 社区现在正在为此付出代价，表现为需要花费数小时才能追踪的神秘运行时错误。通过将某些过去仅引发 `Warning` 的危险实践提升为 `Error`，不良的编码实践在 PHP 8 中会迅速显现，因为 `Errors` 是致命的，会导致应用程序停止运行。

让我们先从检查对象错误处理中的错误提升开始。

> **重要说明**
> 作为一般规则，在 PHP 8 中，当尝试 *写入* 数据时，`Warnings` 会被提升为 `Errors`。另一方面，对于相同的一般情况（例如，尝试读取/写入不存在对象的属性），当尝试 *读取* 数据时，`Notice` 会在 PHP 8 中被提升为 `Warning`。总体思路是写入尝试可能导致数据丢失或损坏，而读取尝试则不会。

## 对象错误处理中被提升的警告

以下是与对象处理相关的现在为 `Errors` 的 `Warnings` 的简要总结。如果尝试执行以下操作，PHP 8 将抛出 `Error`：

- 对非对象的属性进行递增/递减
- 修改非对象的属性
- 为非对象的属性赋值
- 从空值创建默认对象

让我们看一个简单的例子。在以下代码片段中，一个值被赋给不存在的对象 $a。然后该值被递增：

```php
// /repo/ch03/php8_warn_prop_nobj.php
$a->test = 0;
$a->test++;
var_dump($a);
```

这是 PHP 7 的输出：

```
PHP Warning:  Creating default object from empty value in /repo/ch03/php8_warn_prop_nobj.php on line 4
class stdClass#1 (1) {
    public $test =>
        int(1)
}
```

如你所见，在 PHP 7 中，stdClass() 实例会被静默创建并发出 Warning，但操作仍被允许继续。如果我们在 PHP 8 下运行相同的代码，请注意输出的差异：

```
PHP Fatal error:  Uncaught Error: Attempt to assign property "test" on null in /repo/ch03/php8_warn_prop_nobj.php:4
```

好消息是，在 PHP 8 中会抛出 Error，这意味着我们可以通过实现 try() / catch() 块轻松捕获它。例如，以下是之前显示的代码可能被重写的写法：

```
try {
    $a->test = 0;
    $a->test++;
    var_dump($a);
} catch (Error $e) {
    error_log(__FILE__ . ':' . $e->getMessage());
}
```

如你所见，这三行中的任何问题现在都被安全地包裹在 try() / catch() 块中，这意味着可以进行恢复。我们现在将注意力转向数组错误处理增强功能。

## 数组处理中被提升的警告

PHP 7 及更早版本中允许的许多关于数组的不良实践现在会抛出 Error。正如前一小节所述，PHP 8 数组错误处理变更旨在对我们在此描述的错误情况给出更强力的响应。这些增强的最终目标是促使开发者走向良好的编码实践。

以下是被提升为错误的数组处理警告简要列表：

- 无法向数组添加元素，因为下一个元素已被占用
- 无法取消设置非数组变量中的偏移量
- 只能解包数组和 Traversable 类型
- 非法的偏移类型

现在让我们逐一检查此列表中的每个错误条件。

## 下一个元素已被占用

为了说明下一个数组元素无法分配因为它已被占用的一个可能场景，请看这个简单的代码示例：

```
// ch03/php8_warn_array_occupied.php
$a[PHP_INT_MAX] = 'This is the end!';
$a[] = 'Off the deep end';
```

假设由于某种原因，对数组元素的赋值使用了最大可能的整数作为数字键（由 PHP_INT_MAX 预定义常量表示）。如果随后我们尝试为下一个元素赋值，就会出现问题！

运行这段代码块在 PHP 7 中的结果如下：

```
PHP Warning: Cannot add element to the array as the next element is already occupied in
/repo/ch03/php8_warn_array_occupied.php on line 7
array(1) {
    [9223372036854775807] =>
    string(16) "This is the end!"
}
```

然而在 PHP 8 中，Warning 已被提升为 Error，结果输出如下：

```
PHP Fatal error: Uncaught Error: Cannot add element to the array as the next element is already occupied in
/repo/ch03/php8_warn_array_occupied.php:7
```

接下来，我们将关注在非数组变量中使用偏移量。

## 非数组变量中的偏移量

将非数组变量当作数组处理可能会产生意外的结果，除非某些实现了 Traversable 的对象类（如 ArrayObject 或 ArrayIterator）。一个典型例子是在字符串上使用数组样式的偏移量。

在某些情况下，使用数组语法访问字符串字符是有用的。一个例子是检查统一资源定位符（URL）是否以尾随逗号或斜杠结尾。在以下代码示例中，我们检查 URL 是否以尾随斜杠结尾。如果是，我们使用 substr() 将其去掉：

```
// ch03/php8_string_access_using_array_syntax.php
$url = 'https://unlikelysource.com/';
if ($url[-1] == '/')
    $url = substr($url, 0, -1);
echo $url;
// returns: "https://unlikelysource.com"
```

在前面的示例中，$url[-1] 数组语法使你能够访问字符串中的最后一个字符。

> **提示**
> 
> 你也可以使用新的 PHP 8 `str_ends_with()` 函数来实现同样的功能！

然而，字符串绝对不是数组，也不应该被当作数组处理。为了避免不良代码可能导致意外结果，PHP 8 已限制了使用数组语法引用字符串字符的轻微滥用。

在以下代码示例中，我们尝试对字符串使用 unset()：

```
// ch03/php8_warn_array_unset.php
$alpha = 'ABCDEF';
unset($alpha[2]);
var_dump($alpha);
```
