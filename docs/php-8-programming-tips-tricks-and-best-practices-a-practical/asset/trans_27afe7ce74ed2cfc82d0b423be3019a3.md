
## 处理违反直觉的结果

不幸的是，这种安排会产生意外的结果。以下代码片段在使用 PHP 7 执行时呈现出违反直觉的输出：

```
// /repo/ch02/php7_ops_concat_1.php
$a = 11;
$b = 22;
echo "Sum: " . $a + $b;
```

仅从代码来看，你会期望输出类似 "Sum:33" 的内容。事实并非如此！请查看在 PHP 7.1 上运行时的以下输出：

```
root@php8_tips_php7 [ /repo/ch02 ]# php php7_ops_concat_1.php
PHP Warning:  A non-numeric value encountered in /repo/ch02/php7_ops_concat_1.php on line 5
PHP Stack trace:
PHP  1. {main}() /repo/ch02/php7_ops_concat_1.php:0
Warning: A non-numeric value encountered in /repo/ch02/php7_ops_concat_1.php on line 5
Call Stack:
  0.0001    345896    1. {main}()
22
```

此时，你可能在想，代码从不说谎，11 + 22 的和怎么会变成 22，就像我们在前面的输出（最后一行）中看到的那样？

答案涉及运算符优先级：从 PHP 7 开始，它始终是从左到右。因此，如果我们使用括号使运算顺序更清晰，实际发生的情况如下：

```
echo ("Sum: " . $a) + $b;
```

11 被连接到 "Sum: "，结果得到字符串 "Sum: 11"。该字符串随后被类型转换为空整数，导致 0 + 22 的表达式，从而得到我们的结果。

如果你在 PHP 8 中运行相同的代码，请注意这里的差异：

```
root@php8_tips_php8 [ /repo/ch02 ]# php php8_ops_concat_1.php
Sum: 33
```

如你所见，算术运算符优先于连接运算符。使用括号，这就是 PHP 8 中代码的有效处理方式：

```
echo "Sum: " . ($a + $b);
```

> Tip
> **最佳实践：** 使用括号以避免依赖运算符优先级而产生的复杂问题。有关降低连接运算符优先级的理由的更多信息，请查看此处：
> https://wiki.php.net/rfc/concatenation_precedence.

我们现在将注意力转向三元运算符。

## 使用嵌套三元运算符

**三元运算符** 对 PHP 语言来说并不新鲜。

然而，它们在 PHP 8 中的解释方式存在一个主要差异。

这一变化与该运算符传统的 **左结合行为** 有关。

为了说明，让我们看一个简单的例子，如下所示：

- 1. 在此示例中，假设我们使用 RecursiveDirectoryIterator 类结合 RecursiveIteratorIterator 类来扫描目录结构。起始代码可能如下所示：
- 2. 然后我们定义一个函数，该函数匹配包含 $searchPath 搜索路径并以 $searchExt 扩展名结尾的文件，如下所示：

```
// /repo/ch02/php7_nested_ternary.php
$path = realpath(__DIR__ . '../');
$searchPath = '/ch';
$searchExt = 'php';
$dirIter = new RecursiveDirectoryIterator($path);
$itIter = new RecursiveIteratorIterator($dirIter);
```

```
function find_using_if($iter, $searchPath, $searchExt) {
    $matching = [];
    $non_match = [];
    $discard = [];
    foreach ($iter as $name => $obj) {
        if (!$obj->isFile()) {
            $discard[] = $name;
        } elseif (!strpos($name, $searchPath)) {
            $discard[] = $name;
        } elseif ($obj->getExtension() != $searchExt) {
            $non_match[] = $name;
        } else {
            $matching[] = $name;
        }
    }
    show($matching, $non_match);
}
```

- 3. 然而，与其使用 if / elseif / else，一些开发者可能会倾向于使用嵌套三元运算符重构此函数。以下是前一步中使用的相同代码可能的样子：

```php
function find_using_tern($iter, $searchPath,
    $searchExt) {
    $matching = [];
    $non_match = [];
    $discard = [];
    foreach ($iter as $name => $obj) {
        $match = !$obj->isFile()
            ? $discard[] = $name
            : !strpos($name, $searchPath)
                ? $discard[] = $name
                : $obj->getExtension() != $searchExt
                    ? $non_match[] = $name
                    : $matching[] = $name;
    }
    show($matching, $non_match);
}
```

两个函数的输出在 PHP 7 中产生相同的结果，如以下屏幕截图所示：

![](img/dbf2674d5b31107c344104b5edcb21c5_77_0.png)

图 2.2 – 使用 PHP 7 的嵌套三元输出

在 PHP 8 中，不使用括号的嵌套三元运算不再被允许。以下是运行相同代码块时的输出：

![](img/dbf2674d5b31107c344104b5edcb21c5_77_1.png)

图 2.3 – 使用 PHP 8 的嵌套三元输出

> Tip
> 
> **最佳实践：** 使用括号以避免嵌套三元运算出现问题。
> 有关三元运算符嵌套差异的更多信息，请查看本文： https://wiki.php.net/rfc/ternary_associativity.

你现在已经对新的空安全运算符有了概念。你还了解了三个现有运算符——可变参数、连接和三元运算符——其功能已略有修改。你现在处于一个良好的位置，可以在升级到 PHP 8 时避免潜在的危害。让我们现在来看另一个新特性，箭头函数。

## 使用箭头函数

箭头函数实际上最早是在 PHP 7.4 中引入的。然而，由于许多开发者并不关注每一个发布更新，在本书中包含对这一优秀新特性的介绍是很重要的。

在本节中，你将了解箭头函数的语法，以及与匿名函数相比的优缺点。

### 通用语法

简单来说，箭头函数是传统匿名函数的简写语法，就像三元运算符是 if () {} else {} 的简写语法一样。箭头函数的通用语法如下所示：

```
fn(<ARGS>) => <EXPRESSION>
```

<ARGS> 是可选的，并且包含在任何其他用户定义 PHP 函数中看到的任何内容。<EXPRESSION> 可以包含任何标准 PHP 表达式，例如函数调用、算术运算等。

现在让我们看看箭头函数和匿名函数之间的差异。

### 箭头函数与匿名函数

在本小节中，你将了解箭头函数和匿名函数之间的差异。为了成为一名高效的 PHP 8 开发者，了解箭头函数何时以及在何处可以替代匿名函数并提高代码性能是很重要的。

在深入了解箭头函数之前，让我们先看一个简单的匿名函数。在以下示例中，分配给 $addOld 的匿名函数产生两个参数的和：

```php
// /repo/ch02/php8_arrow_func_1.php
$addOld = function ($a, $b) { return $a + $b; };
```

在 PHP 8 中，你可以产生完全相同的结果，如下所示：

```
$addNew = fn($a, $b) => $a + $b;
```

尽管代码更易读，但这一新特性有优缺点，总结如下表所示：

|  | 匿名函数 | 箭头函数 |
|---|---|---|
| 代码更少 | 否 | 是 |
| 继承变量 | 需要 use() | 是 |
| 可以嵌套 | 是 | 是 |
| $this 自动绑定 | 是 | 是 |
| $$ 间接支持 | 是 | 否 |
| 多行代码 | 是 | 否 |

表 2.1 – 匿名函数与箭头函数

从上表可以看出，箭头函数比匿名函数更高效。然而，由于缺乏间接支持和多行代码支持，有时你仍需要使用匿名函数。

### 变量继承

匿名函数与任何标准 PHP 函数类似，只有在将值作为参数传递、使用 global 关键字或添加 use() 修饰符时，才会识别其作用域外的变量。

以下是 DateTime 实例通过 use() 继承到匿名函数中的示例：

```php
// /repo/ch02/php8_arrow_func_2.php
// not all code shown

$old = function ($today) use ($format) {
    return $today->format($format);
};
```

以下是使用箭头函数实现完全相同功能的方式：

```php
$new = fn($today) => $today->format($format);
```

如你所见，语法极其简洁易读。现在让我们检查一个包含箭头函数的实际示例。
