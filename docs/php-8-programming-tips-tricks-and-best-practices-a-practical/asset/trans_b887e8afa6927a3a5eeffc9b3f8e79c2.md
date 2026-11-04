
## 理解 PHP 8 扩展的方差支持

方差的概念是 OOP 的核心。**Variance** 是一个总称，涵盖了各种 **subtypes** 之间的相互关系。约 20 年前，一对早期的计算机科学家 Wing 和 Liskov 提出了一个重要的定理，它是 OOP 子类型核心，现在被称为 **the Liskov Substitution Principle.**

不深入精确的数学，这条原则可以概括如下：

> *如果可以将 X 类的实例替换为 Y 类的实例，并且应用程序的行为在任何方面都不发生变化，则可以认为 X 类是 Y 类的子类型。*

> **提示**
> 
> 最初描述并提供了里氏替换原则精确数学公式化定义的论文可以在这里找到：*子类型的行为概念*，ACM 程序设计语言和系统汇刊，由 B. Liskov 和 J. Wing 撰写，1994 年 11 月 (https://dl.acm.org/doi/10.1145/197320.197383)。

在本节中，我们将研究 PHP 8 如何以 **covariant returns** 和 **contravariant parameters** 的形式提供增强的方差支持。理解协变和逆变将提高你编写良好稳健代码的能力。没有这种理解，你的代码可能会产生不一致的结果并成为许多错误的来源。

让我们先介绍协变返回。

### 理解协变返回

PHP 中的协变支持旨在保留从最具体到最一般的类型的顺序。这种经典示例体现在 `try` / `catch` 块的构建方式中：

- 1. 在此示例中，`try` 块内创建了一个 PDO 实例。接下来的两个 `catch` 块首先查找 `PDOException`。之后是第二个 `catch` 块，它捕获实现 `Throwable` 的任何类。因为 PHP 的 `Exception` 和 `Error` 类都实现了 `Throwable`，第二个 `catch` 块最终成为除 `PDOException` 之外的任何错误的后备：

```php
try {
    $pdo = new PDO($dsn, $usr, $pwd, $opts);
} catch (PDOException $p) {
    error_log('Database Error: ' . $p->getMessage());
} catch (Throwable $t) {
    error_log('Unknown Error: ' . $t->getMessage());
}
```

- 2. 在此示例中，如果由于无效参数导致 PDO 实例失败，错误日志将具有条目 **Database Error**，后跟从 PDOException 获取的消息。

- 3. 另一方面，如果发生其他一般错误，错误日志将具有条目 **Unknown Error**，后跟来自其他 Exception 或 Error 类的消息。

- 4. 在此示例中，catch 块的顺序被反转：

```php
try {
    $pdo = new PDO($dsn, $usr, $pwd, $opts);
} catch (Throwable $t) {
    error_log('Unknown Error: ' . $t->getMessage());
} catch (PDOException $p) {
    error_log('Database Error: ' . $p->getMessage());
}
```

- 5. 由于 PHP 协变支持的工作方式，第二个 catch 块永远不会被调用。相反，源自此代码块的所有错误日志条目都将以 **Unknown Error** 开头。

现在让我们看看 PHP 协变支持如何应用于对象方法返回数据类型：

- 1. 首先，我们定义一个接口 FactoryInterface，它标识一个方法 make()。此方法接受数组作为参数，并预期返回 ArrayObject 类型的对象：

```php
interface FactoryInterface {
    public function make(array $arr): ArrayObject;
}
```

- 2. 接下来，我们定义一个 ArrTest 类，它扩展了 ArrayObject：

```php
class ArrTest extends ArrayObject {
    const DEFAULT_TEST = 'This is a test';
}
```

- 3. ArrFactory 类实现了 FactoryInterface 并完全定义了 make() 方法。注意，此方法返回的是 ArrTest 数据类型而不是 ArrayObject：

```php
class ArrFactory implements FactoryInterface {
    protected array $data;
    public function make(array $data): ArrTest {
        $this->data = $data;
        return new ArrTest($this->data);
    }
}
```

- 4. 在过程调用代码块中，我们创建 ArrFactory 的一个实例，并两次运行其 make() 方法，理论上产生两个 ArrTest 实例。然后我们使用 var_dump() 来揭示两个生成对象的当前状态：

```php
$factory = new ArrFactory();
$obj1 = $factory->make([1,2,3]);
$obj2 = $factory->make(['A','B','C']);
var_dump($obj1, $obj2);
```

- 5. 在 PHP 7.1 中，由于它不支持协变返回数据类型，会抛出致命 Error。输出显示，方法返回类型声明与 FactoryInterface 中定义的不匹配：

```php
root@php8_tips_php7 [/repo/ch05 ]#
php php8_variance_covariant.php
PHP Fatal error:  Declaration of ArrFactory::make(array $data): ArrTest must be compatible with
FactoryInterface::make(array $arr): ArrayObject in /repo/
ch05/php8_variance_covariant.php on line 9
```

- 6. 当我们在 PHP 8 中运行相同的代码时，你可以看到为返回类型提供了协变支持。执行不受阻碍，如下所示：

```php
root@php8_tips_php8 [ /repo/ch05 ]#
php php8_variance_covariant.php
object(ArrTest)#2 (1) {
  ["storage":"ArrayObject":private]=>
  array(3) {
    [0]=>  int(1)
    [1]=>  int(2)
    [2]=>  int(3)
  }
}
object(ArrTest)#3 (1) {
  ["storage":"ArrayObject":private]=>
  array(3) {
    [0]=>  string(1) "A"
    [1]=>  string(1) "B"
    [2]=>  string(1) "C"
  }
}
```

ArrTest 扩展了 ArrayObject，并且是一个合适的子类型，清楚地符合里氏替换原则所定义的标准。正如你从最后的输出中看到的那样，PHP 8 比早期版本的 PHP 更充分地拥抱了真正的 OOP 原则。最终结果是，使用 PHP 8 时，你的代码和应用程序架构可以更加直观和逻辑合理。

现在让我们看看逆变参数。

### 使用逆变参数

而协变关注的是从一般到具体的子类型的排序，**contravariance** 关注的是相反的方向：从具体到一般。在 PHP 7 及更早版本中，逆变的完整支持不可用。因此，在 PHP 7 中实现接口或扩展抽象类时，参数类型提示是 **invariant**。

另一方面，在 PHP 8 中，由于支持逆变参数，你可以在顶层的超类和接口中自由地具体化。只要子类型兼容，你就可以在扩展或实现的类中将类型提示修改为更一般。

这为你在定义接口或抽象类时定义整体架构提供了更大的自由。PHP 8 中使用你的接口或超类的开发人员在实现派生类逻辑方面获得了更大的灵活性。

让我们看看 PHP 8 对逆变参数的支持如何工作：

- 1. 在此示例中，我们首先定义一个 IterObj 类，它扩展了内置的 ArrayIterator PHP 类：
- 2. 我们然后定义一个抽象的 Base 类，它强制要求一个方法 stringify()。注意其唯一参数的数据类型是 IterObj：
- 3. 接下来，我们定义一个 IterTest 类，它扩展了 Base 并为 stringify() 方法提供了实现。特别值得注意的是，我们覆盖了数据类型，将其更改为 iterable：

```php
// /repo/ch05/php8_variance_contravariant.php
class IterObj extends ArrayIterator {}
```

```php
abstract class Base {
    public abstract function stringify(IterObj $it);
}
```

```php
class IterTest extends Base {
    public function stringify(iterable $it) {
        return implode(',',
            iterator_to_array($it)) . "\n";
    }
}
class IterObj extends ArrayIterator {}
```

- 4. 接下来的几行代码创建了 IterTest、IterObj 和 ArrayIterator 的实例。然后我们两次调用 stringify() 方法，提供后两个对象作为参数：

```php
$test = new IterTest();
$objIt = new IterObj([1,2,3]);
$arrIt = new ArrayIterator(['A','B','C']);
echo $test->stringify($objIt);
echo $test->stringify($arrIt);
```

- 5. 在 PHP 7.1 中运行此代码示例会产生预期的致命 Error，如下所示：

```php
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_variance_contravariant.php
PHP Fatal error: Declaration of
IterTest::stringify(iterable $it) must be compatible with
Base::stringify(IterObj $it) in /repo/ch05/php8_variance_contravariant.php on line 11
```

因为 PHP 7.1 不提供逆变参数的支持，它将参数的数据类型视为不变的，并简单地显示一条消息，指出子类的的数据类型与父类中指定的数据类型不兼容。

- 6. PHP 8 另一方面提供了逆变参数的支持。因此，它认识到 Base 类中指定的数据类型 IterObj 是与 iterable 兼容的子类型。此外，提供的两个参数都与 iterable 兼容，允许程序执行继续进行。以下是 PHP 8 的输出：

```php
root@php8_tips_php8 [ /repo/ch05 ]# php php8_variance_contravariant.php
1,2,3
A,B,C
```

你从 PHP 8 对协变返回和逆变参数的支持中获得的主要优势是能够不仅覆盖方法逻辑，还能覆盖方法签名。你会发现，虽然 PHP 8 在执行良好的编码实践方面更加严格，但增强的方差支持为你设计继承结构提供了更大的自由。在某种程度上，至少就参数和返回值数据类型而言，PHP 8 如果不是更不限制的话，也是更不限制的！
