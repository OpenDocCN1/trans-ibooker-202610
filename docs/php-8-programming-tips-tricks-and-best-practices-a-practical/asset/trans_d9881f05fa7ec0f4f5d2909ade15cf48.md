
```php
$this->src_dir = $src_dir
?? realpath(self::DEFAULT_SRC);
spl_autoload_register([$this, 'autoload']);
}
```

2. 实际的自动加载代码与我们在上面的 __autoload() 函数示例中展示的类似。以下是执行实际自动加载的方法：

```php
public function autoload($class) {
    $fn = str_replace('\\', '/', $class);
    $fn = $this->src_dir . '/' . $fn . '.php';
    $fn = str_replace('//', '/', $fn);
    require_once($fn);
}
}
```

现在你已经对如何使用 spl_autoload_register() 函数有了一个概念，我们必须检查在运行 PHP 8 时可能出现的潜在代码中断。

## PHP 8 中 spl_autoload_register() 的潜在代码中断

spl_autoload_register() 函数的第二个参数是一个可选的布尔值，默认为 FALSE。如果第二个参数设置为 TRUE，则在 PHP 7 及以下版本中，如果自动加载器注册失败，spl_autoload_register() 函数会抛出一个 Exception。在 PHP 8 中，然而，如果第二个参数的数据类型不是 callable，则会抛出致命的 TypeError，而不管第二个参数的值如何！

下面所示的简单程序示例说明了这种危险。在此示例中，我们使用 spl_autoload_register() 函数注册一个不存在的 PHP 函数。我们将第二个参数设置为 TRUE：

```php
// /repo/ch05/php7_spl_spl_autoload_register.php
try {
    spl_autoload_register('does_not_exist', TRUE);
    $data = ['A' => [1,2,3],'B' => [4,5,6],'C' => [7,8,9]];
    $response = new \Application\Strategy\JsonResponse($data);
    echo $response->render();
} catch (Exception $e) {
    echo "A program error has occurred\n";
}
```

如果我们然后在 PHP 7 中运行这段代码，结果如下：

```
root@php8_tips_php7 [ /repo/ch05 ]#
php php7_spl_spl_autoload_register.php
A program error has occurred
```

如输出所示，会抛出一个 Exception。catch 块被调用，消息 A program error has occurred 出现。然而，当我们在 PHP 8 中运行相同的程序时，会抛出一个致命的 Error：

```
root@php8_tips_php8 [ /repo/ch05 ]#
php php7_spl_spl_autoload_register.php
PHP Fatal error: Uncaught TypeError: spl_autoload_register(): Argument #1 ($callback) must be a valid callback, no array or string given in /repo/ch05/php7_spl_spl_autoload_register.php:12
```

显然，catch 块被绕过了，因为它设计用来捕获 Exception，而不是 Error。简单的解决方案是让 catch 块捕获 Throwable 而不是 Exception。这使得相同的代码可以在 PHP 7 或 PHP 8 中运行

以下是重写后的代码可能的样子。输出未显示，因为它与在 PHP 7 中运行的相同示例相同：

```
// /repo/ch05/php8_spl_spl_autoload_register.php
try {
    spl_autoload_register('does_not_exist', TRUE);
    $data = ['A' => [1,2,3],'B' => [4,5,6],'C' => [7,8,9]];
    $response = new \Application\Strategy\JsonResponse($data);
    echo $response->render();
} catch (Throwable $e) {
    echo "A program error has occurred\n";
}
```

你现在对 PHP 8 的自动加载有了更好的理解，以及如何发现和纠正潜在的自动加载向后兼容性中断。现在让我们来看看 PHP 8 中与魔术方法相关的更改。

## 导航魔术方法的更改

PHP **魔法方法**是预定义的钩子，会中断 OOP 应用的正常流程。每个魔术方法，如果定义，会从对象实例创建的那一刻起改变应用程序的行为，直到实例 **超出作用域** 为止。

> **重要提示**
> 对象实例在被 unset 或覆盖时 *超出作用域*。对象实例也在定义在函数或类方法中，并且该函数或类方法的执行结束时超出作用域。最终，如果没有其他原因，对象实例会在 PHP 程序结束时超出作用域。

本节将使你对 PHP 8 中引入的魔术方法使用和行为的重要更改有扎实的理解。一旦你理解本节中描述的情况，你将能够做出适当的代码修改，以防止在迁移到 PHP 8 时应用程序代码失败。

让我们首先看看对象构造方法的更改。

## 处理构造函数更改

理想情况下，**类构造函数**是在对象实例创建时自动调用的方法，用于执行某种对象初始化。这种初始化最通常涉及使用作为参数提供给此方法的值填充对象属性。初始化还可以执行任何必要的任务，例如打开文件句柄、建立数据库连接等。

在 PHP 8 中，对类构造函数调用方式做了一些更改。这意味着在将应用程序迁移到 PHP 8 时存在向后兼容性中断的潜在风险。我们将首先检查的第一个更改与使用与类同名的方法作为类构造函数已弃用的用法有关。

## 处理方法与类同名更改

在 PHP 4 版本中引入的第一个 PHP OOP 实现中，确定了与类同名的方法将承担类构造函数的角色，并在创建新对象实例时自动调用。

一个鲜为人知的事实是，即使在 PHP 8 中，函数、方法和甚至类名都是 *大小写不敏感* 的。因此 `$a = new ArrayObject();` 等同于 `$b = new arrayobject();`。另一方面，变量名是大小写敏感的。

从 PHP 5 开始，随着一个新且更强大的 OOP 实现，魔术方法被引入。其中一个方法是 __construct()，专门保留用于类构造，设计用于取代旧的用法。使用与类同名的方法作为构造函数在 PHP 5 的剩余版本中得到支持，并且一直延续到 PHP 7 的所有版本。

在 PHP 8 中，对与类本身同名的类构造函数方法的支持已被移除。如果同时定义了 __construct() 方法，你将不会有问题：__construct() 作为类构造函数具有优先权。如果没有 __construct() 方法，并且你检测到与类()同名的方法，你可能会失败。请记住，方法和类名都是大小写不敏感的！

看看下面的示例。它在 PHP 7 中有效，但在 PHP 8 中无效：

- 1. 首先，我们定义一个 Text 类，其中有一个与类同名的类构造函数方法。构造函数方法基于提供的文件名创建一个 SplFileObject 实例：
- 2. 然后我们添加三行过程式代码来使用该类，提供包含葛底斯堡演说的文件名：

```
// /repo/ch05/php8_oop_bc_break_construct.php
class Text {
    public $fh = '';
    public const ERROR_FN = 'ERROR: file not found';
    public function text(string $fn) {
        if (!file_exists($fn))
            throw new Exception(self::ERROR_FN);
        $this->fh = new SplFileObject($fn, 'r');
    }
    public function getText() {
        return $this->fh->fpassthru();
    }
}
```

```
$fn   = __DIR__ . '../sample_data/gettysburg.txt';
$text = new Text($fn);
echo $text->getText();
```

- 3. 在 PHP 7 中运行程序首先会产生一个弃用通知，然后是预期的文本。这里仅显示输出的前几行：

```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_bc_break_construct.php
PHP Deprecated: Methods with the same name as their class will not be constructors in a future version of PHP; Text has a deprecated constructor in /repo/ch05/php8_bc_break_construct.php on line 4

Fourscore and seven years ago our fathers brought forth on this continent a new nation, conceived in liberty and dedicated to the proposition that all men are created equal. ... <remaining text not shown>
```

- 4. 然而，在 PHP 8 中运行相同的程序时，会抛出致命的 Error，如此输出所示：

```
root@php8_tips_php8 [ /repo/ch05 ]# php php8_bc_break_construct.php
PHP Fatal error: Uncaught Error: Call to a member function fpassthru() on string in /repo/ch05/php8_bc_break_construct.php:16
```

需要注意的是，PHP 8 中显示的错误并没有告诉你程序失败的真正原因。因此，极其重要的是你扫描你的 PHP 应用程序，特别是旧的应用程序，以查看是否存在与类同名的方法。因此，最佳实践是简单地将与类同名的方法重命名为 __construct()。

现在让我们看看 PHP 8 中如何解决类构造函数中处理 Exception 和 exit 的不一致问题。

## 解决类构造函数中的不一致问题

PHP 8 中解决的另一个问题与类构造方法要么抛出 Exception，要么执行 exit() 的情况有关。在 PHP 8 之前的版本中，如果在类构造函数中抛出 Exception，则如果定义了 __destruct() 方法，则不会调用它。另一方面，如果在构造函数中使用 exit() 或 die()（这两个 PHP 函数彼此等效），则会调用 __destruct() 方法。在 PHP 8 中，此不一致性得到解决。现在，在任何一种情况下，__destruct() 方法都不会被调用。
