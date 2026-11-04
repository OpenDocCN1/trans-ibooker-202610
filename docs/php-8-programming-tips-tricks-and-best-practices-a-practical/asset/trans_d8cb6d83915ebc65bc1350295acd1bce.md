
6. 然而，要用 DateTime 实例填充数组，我们需要将 DateTimeImmutable 转换为 DateTime。在 PHP 7.3 中，引入了 DateTime::createFromImmutable() 方法。因此，当值赋给 $aging 时，修改后的代码可能如下所示：

```php
$aging[$span] = DateTime::createFromImmutable($item);
```

7. 否则，你只能创建新的 DateTime 实例，如下所示：

```php
$aging[$span] = new DateTime($item->format($fmt));
```

通过这一项更改，正确的输出将如下所示：

| Day | Date |
|---|---|
| 0 | 2021-05-24 |
| 30 | 2021-06-23 |
| 60 | 2021-07-23 |
| 90 | 2021-08-22 |

你现在已经了解 DateTimeImmutable 的使用方式，并且也对如何转换为 DateTime 有了概念。你会很高兴地知道，在 PHP 8 中，通过引入 createFromInterface() 方法，两种对象类型之间的转换变得更加容易。

## 检查 createFromInterface() 方法

在 PHP 8 中，在 DateTime 和 DateTimeImmutable 之间转换，以及反向转换变得更加容易。名为 createFromInterface() 的新方法已添加到两个类中。该方法的签名只需一个 DateTimeInterface 实例，这意味着 DateTime 或 DateTimeImmutable 的实例都是此方法的可接受参数。

以下简短的代码示例演示了在 PHP 8 中从一种类型转换为另一种类型是多么容易：

- 1. 首先，让我们定义一个 DateTimeImmutable 实例并输出其类和日期：

```php
// /repo/ch09/php8_date_time.php
$fmt = 'l, d M Y';
$dti = new DateTimeImmutable('last day of next month');
echo $dti::class . ':' . $dti->format($fmt) . "\n";
```

- 2. 然后，从 $dti 创建一个 DateTime 实例并添加 90 天的间隔，显示其类和当前日期：

```php
$dtt = DateTime::createFromInterface($dti);
$dtt->add(new DateInterval('P90D'));
echo $dtt::class . ':' . $dtt->format($fmt) . "\n";
```

- 3. 最后，从 $dtt 创建一个 DateTimeImmutable 实例并显示其类和日期：

```php
$dtx = DateTimeImmutable::createFromInterface($dtt);
echo $dtx::class . ':' . $dtx->format($fmt) . "\n";
```

以下是此代码示例在 PHP 8 中运行的输出：

```
root@php8_tips_php8 [/repo/ch09 ]# php php8_date_time.php
DateTimeImmutable:Wednesday, 30 Jun 2021
DateTime:Tuesday, 28 Sep 2021
DateTimeImmutable:Tuesday, 28 Sep 2021
```

正如你所见，我们使用了相同的 createFromInterface() 方法来创建实例。当然请记住，我们实际上并没有将类实例转换为另一个实例。相反，我们是创建了克隆的实例，但类类型不同。

你现在知道为什么你可能想要 DateTimeImmutable 而不是 DateTime。你也知道在 PHP 8 中，名为 createFromInterface() 的新方法提供了一种统一的方式从一个创建另一个的实例。接下来，我们将看看 PHP 8 中对 trait 处理的改进。

## 了解 PHP 8 trait 处理的改进

trait 的实现首次在 PHP 5.4 版本中引入。自那时以来，进行了持续不断的改进。PHP 8 通过提供一种明确识别当多个 trait 具有冲突方法时使用哪些方法的方式，继续了这一趋势。此外，除了消除可见性声明中的不一致之外，PHP 8 还解决了 trait 处理（或未处理！）抽象方法方面的问题。

作为开发者，完全掌握 trait 的使用可以让你编写更高效且更易维护的代码。Trait 可以帮助你避免产生冗余代码。它们解决了需要在不同命名空间或不同类继承结构中提供相同逻辑的问题。本节介绍的信息使你能够在 PHP 8 下运行的代码中正确使用 trait。

首先，让我们看看 PHP 8 中如何解决 trait 之间的冲突。

### 解决 trait 之间的方法冲突

可以通过简单地用逗号分隔列出 trait 名称来使用多个 trait。然而，如果两个 trait 定义了相同的方法，就可能出现潜在问题。为了解决此类冲突，PHP 提供了 as 关键字。在 PHP 7 及更早版本中，为避免两个同名方法冲突，你可以简单地重命名其中一个方法。执行重命名的代码可能如下所示：

```
use Trait1, Trait2 { <METHOD> as <NEW_NAME>; }
```

然而，这种方法的问题在于 PHP 做出了一个静默假设：METHOD 被假定来自 Trait1！在持续努力强制执行良好编码实践方面，PHP 8 不再允许这种假设。PHP 8 中的解决方案是通过使用 insteadof 而不是 as 来更具体。

这里有一个简单的示例来说明这个问题：

- 1. 首先，让我们定义两个 trait，它们定义了相同的方法 test()，但返回不同的结果：

```php
// /repo/ch09/php7_trait_conflict_as.php
trait Test1 {
    public function test() {
        return '111111';
    }
}
trait Test2 {
    public function test() {
        return '222222';
    }
}
```

- 2. 然后，定义一个使用两个 trait 的匿名类，并将 test() 指定为 otherTest() 以避免命名冲突：

```php
$main = new class () {
    use Test1, Test2 { test as otherTest; }
    public function test() { return 'TEST'; }
};
```

- 3. 接下来，定义一段代码块来输出两个方法的返回值：

```php
echo $main->test() . "\n";
echo $main->otherTest() . "\n";
```

以下是 PHP 7 中的输出：

```
root@php8_tips_php7 [ /repo/ch09 ]#
php php7_trait_conflict_as.php
TEST
111111
```

正如你所见，PHP 7 静默假设我们指的是将 Trait1::test() 重命名为 otherTest()。然而，从示例代码中，完全不清楚这是否是程序员的意图！

在 PHP 8 中运行相同的代码示例，我们会得到不同的结果：

```
root@php8_tips_php8 [ /repo/ch09 ]#
php php7_trait_conflict_as.php
PHP Fatal error:  An alias was defined for method test(), which exists in both Test1 and Test2. Use Test1::test or Test2::test to resolve the ambiguity in /repo/ch09/php7_trait_conflict_as.php on line 6
```

显然，PHP 8 不会做出此类静默假设，因为它们很容易导致意外行为。在此示例中，最佳实践是使用作用域解析 (::) 运算符。以下是重写的代码：

```php
$main = new class () {
    use Test1, Test2 { Test1::test as otherTest; }
    public function test() { return 'TEST'; }
};
```

如果我们在 PHP 8 中重新运行代码，输出将与为 PHP 7 显示的输出相同。作用域解析运算符确认 Trait1 是 test() 方法的源 trait，从而避免任何歧义。现在，让我们看看 PHP 8 trait 如何处理抽象方法签名。

### 使用 trait 抽象签名检查

正如 API 开发者所熟知，将方法标记为抽象是向 API 用户发出该方法为强制性但尚未定义的方式。此技术允许 API 开发者不仅指定方法名称，还指定其签名。

然而，在 PHP 7 及更早版本中，trait 中定义的抽象方法会忽略签名，这违背了使用抽象方法的初衷！当你在 PHP 8 中使用包含抽象方法的 trait 时，其签名会与使用该 trait 的类中的实现进行检查。

以下示例在 PHP 7 中有效，但在 PHP 8 中失败，因为方法签名不同：

- 1. 首先，让我们声明严格类型检查并定义一个包含抽象方法 trait；即 add()。请注意，方法签名要求全部使用整数数据类型：
- 2. 接下来，定义一个使用该 trait 并定义 add() 的匿名类。请注意，该类的所有数据类型均为 float：
- 3. 然后，输出 111.111 和 222.222 相加的结果：

```php
// /repo/ch09/php7_trait_abstract_signature.php
declare(strict_types=1);
trait Test1 {
    public abstract function add(int $a, int $b) : int;
}
```

```php
$main = new class () {
    use Test1;
    public function add(float $a, float $b) : float {
        return $a + $b;
    }
};
```

```php
echo $main->add(111.111, 222.222) . "\n";
```
