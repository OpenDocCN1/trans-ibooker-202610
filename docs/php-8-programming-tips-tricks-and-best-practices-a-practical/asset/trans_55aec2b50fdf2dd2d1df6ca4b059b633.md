
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
