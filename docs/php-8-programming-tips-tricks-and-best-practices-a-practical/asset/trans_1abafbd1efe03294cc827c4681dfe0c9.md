
```
if (isset($added[$key])) {
    $value = $container->get($added[$key])
        ->filter($value);
}
}
var_dump($data);
```

6. 最后，我们抓取内存统计信息以形成 SplObjectStorage 与 WeakMap 使用之间的比较基准。我们还取消设置 $filters，这理论上应该释放大量内存。我们运行 gc_collect_cycles() 来强制 PHP 垃圾回收过程，将释放的内存归还到内存池中：

```
$mem = memory_get_usage();
unset($filters);
gc_collect_cycles();
$end = memory_get_usage();
echo "\nMemory Before Unset: $mem\n";
echo "Memory After  Unset: $end\n";
echo 'Difference      : ' . ($end - $mem) . "\n";
echo 'Peak Memory Usage : ' . memory_get_peak_usage();
```

以下是调用程序的运行结果，在 PHP 8 中运行：

```
root@php8_tips_php8 [ /repo/ch10 ]#
php php7_weak_map_problem.php
array(5) {
["name"]=>  string(18) "bad JavaScriptname"
["status"]=>  int(0)
["gender"]=>  NULL
["space"]=>  string(30) "leading/trailing whitespace or"
["url"]=>  &string(32) "https://unlikelysource.com/about"
}
Memory Before Unset: 518936
Memory After  Unset: 518672
Difference      :  264
Peak Memory Usage : 780168
```

正如输出所示，我们的容器类工作完美，使我们能够访问任何存储的过滤器类。值得注意的是，执行 unset ($filters) 命令后释放的内存为 264 字节：并不多！

你现在已经了解了 SplObjectStorage 类的典型用法。让我们现在来看一下 SplObjectStorage 类的一个潜在问题，以及 WeakMap 如何解决它。

## 理解 WeakMap 相对于 SplObjectStorage 的优势

SplObjectStorage 的主要问题是，当分配的对象被取消设置或超出作用域时，它仍然保留在内存中。其原因是当对象附加到 SplObjectStorage 实例时，是通过引用完成的。

如果你只处理少量对象，可能不会遇到任何严重问题。如果你使用 SplObjectStorage 并为存储分配大量昂贵的对象，这最终可能导致长期运行的程序中出现内存泄漏。另一方面，如果你使用 WeakMap 实例进行存储，垃圾回收就可以移除该对象，从而释放内存。当你开始将 WeakMap 实例集成到常规编程实践中时，你最终会得到更高效的代码，占用更少的内存。

> **提示**
> 有关 WeakMap 的更多信息，请在此处查看原始 RFC：https://wiki.php.net/rfc/weak_maps。另请查看文档：https://www.php.net/weakMap。

让我们现在重写上一节中的示例 (/repo/ch010/php7_weak_map_problem.php)，但这次使用 WeakMap：

- 1. 如前面的代码示例所述，我们定义了一个名为 UsesWeakMap 的容器类来保存我们的昂贵过滤器类。此类与上一节中显示的类的主要区别在于，UsesWeakMap 使用 WeakMap 而不是 SplObjectStorage 进行存储。以下是类设置和 __construct() 方法：

// /repo/src/Php7/Container/UsesWeakMap.php
namespace Php8\Container;
use WeakMap;
class UsesWeakMap {
```

```php
public $container;
public $default;
public function __construct(array $config = []) {
    $this->container = new WeakMap();
    if ($config)
        foreach ($config as $obj)
            $this->container->offsetSet(
                $obj, get_class($obj));
    $this->default = new class () {
        public function filter($value) {
            return $value; }};
    }
```
```

- 2. 这两个类之间的另一个区别是 WeakMap 实现了 IteratorAggregate。然而，这仍然允许我们在 get() 方法中使用简单的 foreach() 循环：

```php
public function get(string $key) {
    foreach ($this->container as $idx => $obj)
        if ($idx instanceof $key) return $idx;
    return $this->default;
    }
}
```

请注意，使用 foreach() 循环迭代 WeakMap 实例时，我们返回键 ($idx) 而不是值！

- 3. 然后我们定义一个调用程序来调用自动加载器并使用适当的过滤器类。此调用程序与上一节中的调用程序之间的最大区别在于，我们使用了基于 WeakMap 的新容器类：

```php
// /repo/ch010/php8_weak_map_problem.php
require __DIR__ .
'../../src/Server/Autoload/Loader.php';
$loader = new \Server\Autoload\Loader();
use Laminas\Filter\ {StringTrim, StripNewlines,
    StripTags, ToInt, Whitelist, UriNormalize};
use Php8\Container\UsesWeakMap;
```

- 4. 与前一个示例一样，我们定义一个示例数据数组并分配过滤器。此代码未显示，因为它与前一个示例的步骤 2 和 3 相同。

- 5. 然后我们在一个数组中创建过滤器实例，该数组作为我们新容器类的参数。我们使用过滤器数组作为参数来创建容器类实例：

```php
$filters = [
    new StringTrim(),
    new StripNewlines(),
    new StripTags(),
    new ToInt(),
    new Whitelist(['list' => ['M','F','X']]),
    new UriNormalize(['enforcedScheme' => 'https']),
];
$container = new UsesWeakMap($filters);
```

- 6. 最后，正如前一个示例的步骤 6 所示，我们遍历数据并应用容器类中的过滤器。我们还收集并显示内存统计信息。

以下是使用 WeakMap 的修订程序的输出，在 PHP 8 中运行：

```
root@php8_tips_php8 [/repo/ch10 ]#
php php8_weak_map_problem.php
array(5) {
["name"]=> string(18) "bad JavaScriptname"
["status"]=> int(0)
["gender"]=> NULL
["space"]=> string(30) "leading/trailing whitespace or"
["url"]=> &string(32) "https://unlikelysource.com/about"
}
Memory Before Unset: 518712
Memory After  Unset: 517912
Difference       :   800
Peak Memory Usage  : 779944
```

正如你所预期的那样，总体内存使用量略低。然而，最大的区别是取消设置 $filters 后的内存差异。在前一个示例中，差异为 264 字节。在此示例中，使用 WeakMap 产生的差异为 800 字节。这意味着使用 WeakMap 有可能释放比使用 SplObjectStorage 多三倍以上的内存！

这结束了我们对弱引用和弱映射的讨论。你现在有能力编写更高效、占用更少内存的代码。存储的对象越大，节省内存的潜力就越大。

## Summary

在本章中，你不仅了解了新的 JIT 编译器的工作原理，还了解了传统的 PHP 解释-编译-执行循环。使用 PHP 8 并启用 JIT 编译器有可能将你的 PHP 应用程序加速三倍以上。

在下一节中，你了解了什么是稳定排序，以及 PHP 8 如何实现这一重要技术。通过掌握稳定排序，你的代码将以合理的方式生成数据，从而提高客户满意度。

接下来的部分向你介绍了一种技术，通过利用 SplFixedArray 类可以大幅提高性能并减少内存消耗。之后，你了解了 PHP 8 对弱引用的支持以及新的 WeakMap 类。使用本章介绍的技术将使你的应用程序执行速度更快、运行更高效、占用更少内存。

在下一章中，你将学习如何成功迁移到 PHP 8。

## 11

## 迁移现有
PHP 应用到 PHP 8

整本书中都已警告过潜在的代码破坏。遗憾的是，实际上并没有很好的工具可以扫描你的现有代码并检查潜在的代码破坏。在本章中，我们将带你开发一组类的基础，这些类构成了 PHP 8 **向后兼容 (BC)** 破坏扫描器的基础。此外，你将学习将现有客户 PHP 应用程序迁移到 PHP 8 的推荐流程。

阅读完本章并仔细研究示例后，你将更好地处理 PHP 8 迁移。了解整体迁移流程后，你将更有信心并能够以最少的问题执行 PHP 8 迁移。

本章涵盖的主题包括以下内容：

- 了解开发、暂存和生产环境
- 学习如何在迁移前发现 BC 破坏
- 执行迁移
- 测试和排除迁移故障

