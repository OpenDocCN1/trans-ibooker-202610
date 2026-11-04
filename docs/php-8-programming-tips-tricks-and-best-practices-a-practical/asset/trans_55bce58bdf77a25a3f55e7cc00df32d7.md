
2. 我们随后运行三个测试，每个测试都检查 $var 是否存在，使用 isset()、property_exists() 和 array_key_exists()：

// not all code is shown
$default = 'DEFAULT';
echo (isset($obj->var))
    ? $obj->var : $default;
echo (property_exists($obj,'var'))
    ? $obj->var : $default;
echo (array_key_exists('var',$obj))
    ? $obj->var : $default;
```

当我们在 PHP 7 中运行此代码时，所有测试都成功，如下所示：

```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_oop_diff_array_key_exists.php
OK.OK.OK.
```

然而在 PHP 8 中，会发生致命的 TypeError，因为 array_key_exists() 现在只接受数组作为参数。PHP 8 的输出如下所示：

```
root@php8_tips_php8 [ /repo/ch05 ]#
php php8_oop_diff_array_key_exists.php
OK.OK.PHP Fatal error: Uncaught TypeError: array_key_exists(): Argument #2 ($array) must be of type array, class@anonymous given in /repo/ch05/php8_oop_diff_array_key_exists.php:10
```

最佳实践是使用 property_exists() 或 isset()。我们现在将注意力转向 PHP 自动加载的更改。

## 使用 PHP 8 自动加载

最初在 PHP 5.1 中引入的基本自动加载类机制在 PHP 8 中工作方式相同。主要区别在于，对在 PHP 7.2 中已弃用的全局函数 __autoload() 的支持已在 PHP 8 中完全移除。从 PHP 7.2 开始，开发人员被鼓励使用自 PHP 5.1 起就为此目的提供的 spl_autoload_register() 来注册他们的自动加载逻辑。另一个主要区别是当无法注册自动加载器时 spl_autoload_register() 的反应方式。

理解使用 `spl_autoload_register()` 时自动加载过程的工作原理对您作为开发人员的工作至关重要。未能掌握 PHP 如何自动定位和加载类将限制您作为开发人员的成长能力，并可能对您的职业发展产生不利影响。

在深入了解 `spl_autoload_register()` 之前，让我们先看看 `__autoload()` 函数。

## 了解 __autoload() 函数

`__autoload()` 函数被许多开发人员用作自动加载逻辑的主要来源。此函数的行为与 *魔法方法* 非常相似，这就是为什么它会根据上下文自动调用。会触发对 `__autoload()` 函数的自动调用的情况包括在创建新类实例的时刻，但此时类定义尚未加载。此外，如果类扩展了另一个类，自动加载逻辑也会被调用，以便在创建扩展它的子类之前加载超类。

使用 `__autoload()` 函数的优点是它定义起来相当容易，并且通常定义在网站的初始 `index.php` 文件中。缺点包括以下内容：

- `__autoload()` 是 PHP 的过程式函数；未使用 OOP 原则定义或控制。例如，在为应用程序定义单元测试时，这可能会成为一个问题。
- 如果您的应用程序使用命名空间，则 `__autoload()` 函数必须定义在全局命名空间中；否则，`__autoload()` 函数所在命名空间之外的类将无法加载。
- `__autoload()` 函数与 `spl_autoload_register()` 配合不佳。如果您同时使用 `__autoload()` 函数和 `spl_autoload_register()` 定义自动加载逻辑，则 `__autoload()` 函数的逻辑将被完全忽略。

为了说明潜在问题，我们将定义一个 OopBreakScan 类，在第 11 章《将现有 PHP 应用迁移到 PHP 8》中会更详细地讨论：

- 1. 首先，我们定义 OopBreakScan 类，然后向其添加一个方法，该方法扫描文件内容以查找 __autoload() 函数。请注意，错误消息是 Base 类中定义的一个类常量，它只是警告 __autoload() 函数的存在：

```php
namespace Migration;
class OopBreakScan extends Base {
    public static function scanMagicAutoloadFunction(
        string $contents, array &$message) : bool {
        $found = 0;
        $found += (stripos($contents,
            'function __autoload(') !== FALSE);
        $message[] = ($found)
            ? Base::ERR_MAGIC_AUTOLOAD
            : sprintf(Base::OK_PASSED,
                __FUNCTION__);
        return (bool) $found;
    }
    // remaining methods not shown
}
```

此类扩展了 Migration\Base 类（未显示）。这很重要，因为任何自动加载逻辑不仅需要找到子类，还需要找到其超类。

- 2. 接下来，我们定义一个调用程序，其中定义了魔术 __autoload() 函数：

```php
// /repo/ch05/php7_autoload_function.php
function __autoload($class) {
    $fn = __DIR__ . '../src/'
        . str_replace('\\', '/', $class)
        . '.php';
    require_once $fn;
}
```

- 3. 然后我们通过让调用程序扫描自身来使用该类：

```
use Migration\OopBreakScan;
$contents = file_get_contents(__FILE__);
$message = [];
OopBreakScan::
    scanMagicAutoloadFunction($contents, $message);
var_dump($message);
```

这里是 PHP 7 中运行的输出：

```
root@php8_tips php7 [/repo/ch05 ]#
php php7_autoload_function.php
/repo/ch05/php7_autoload_function.php:23:
array(1) {
    [0] => string(96) "WARNING: the "__autoload()" function is removed in PHP 8: replace with "spl_autoload_register()""
}
```

正如您从输出中看到的，Migration\OopBreakScan 类已被自动加载。我们知道这一点是因为调用了 scanMagicAutoloadFunction 方法，并且我们有其结果。此外，我们知道 Migration\Base 类也被自动加载了。我们之所以知道这一点，是因为输出中出现的错误消息是超类的常量。

然而，同一代码在 PHP 8 中运行会产生以下结果：

```
root@php8_tips php8 [/repo/ch05 ]#
php php7_autoload_function.php
PHP Fatal error: __autoload() is no longer supported, use spl_autoload_register() instead in /repo/ch05/php7_autoload_function.php on line 4
```

这个结果并不令人惊讶，因为对魔术 __autoload() 函数的支持已在 PHP 8 中移除。在 PHP 8 中，您必须改用 spl_autoload_register()。我们现在将注意力转向 spl_autoload_register()。

## 学习使用 spl_autoload_register()

spl_autoload_register() 函数的主要优点是它允许您注册多个自动加载器。虽然这可能看起来有点过分，但想象一下这样的噩梦场景：您正在使用许多不同的开源 PHP 库……并且它们都有自己的自动加载器定义！只要所有此类库都使用 spl_autoload_register()，拥有多个自动加载器回调就不会有问题。

使用 spl_autoload_register() 注册的每个自动加载器必须是可调用的。以下任何一项都被视为可调用：

- PHP 过程式函数
- 匿名函数
- 可以以静态方式调用的类方法
- 定义了 __invoke() 魔法方法的任何类实例
- 这种形式的数组：[$instance, 'method']

> 提示
> 
> Composer 维护自己的自动加载器，而它又依赖于 spl_autoload_register()。如果您使用 Composer 管理开源 PHP 包，您只需在应用程序代码开始时包含 /path/to/project/vendor/autoload.php 即可使用 Composer 自动加载器。要让 Composer 自动加载您的应用程序源代码文件，请在 composer.json 文件的 autoload : psr-4 键下添加一个或多个条目。有关更多信息，请参阅 https://getcomposer.org/doc/04-schema.md#psr-4。

一个相当典型的自动加载器类可能如下所示。请注意，这是我们在本书中展示的许多 OOP 示例中使用的类：

- 1. 在 __construct() 方法中，我们分配源目录。之后，我们使用上面提到的数组可调用语法调用 spl_autoload_register()：

// /repo/src/Server/Autoload/Loader.php
namespace Server\Autoload;
class Loader {
    const DEFAULT_SRC = __DIR__ . '/../../';
    public $src_dir = '';
    public function __construct($src_dir = NULL) {
