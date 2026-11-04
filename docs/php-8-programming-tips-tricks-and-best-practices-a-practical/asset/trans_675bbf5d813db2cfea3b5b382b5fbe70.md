
8. 然而，如果我们随后尝试反序列化该对象，会出现另一个错误：

```
PHP Fatal error: Uncaught Error: Call to a member function rewind() on null in /repo/ch05/php8_serialization_wakeup.php:13
```

当然，问题是文件句柄在序列化过程中丢失了。当对象被反序列化时，__construct() 方法未被调用。

9. 这正是 __wakeup() 魔术方法存在的目的。为了解决此错误，我们定义一个 __wakeup() 方法来调用 __construct() 方法：

```
public function __wakeup() {
    self::__construct();
}
```

10. 如果我们重新运行代码，现在会看到葛底斯堡演说两次（未显示）。

现在你对 PHP 原生序列化的工作原理有了大致了解，并且也对 __sleep() 和 __wakeup() 魔术方法以及潜在的代码破坏有所了解。让我们现在来看一个旨在促进对象自定义序列化的接口。

## 介绍 Serializable 接口

为了便于对象的序列化，Serializable 接口从 PHP 5.1 开始被添加到语言中。该接口背后的想法是提供一种识别能够自行序列化的对象的方法。此外，该接口指定的方法旨在提供一定程度的对象序列化控制。

只要一个类实现了该接口，开发人员就可以确保定义了两个方法：serialize() 和 unserialize()。以下是接口定义：

```
interface Serializable {
    public function serialize(): string|null
    public function unserialize(string $serialized): void
}
```

任何实现该接口的类，其自定义的 serialize() 和 unserialize() 方法都会在原生序列化和反序列化期间自动调用。为了说明此技术，请考虑以下示例：

- 1. 首先，我们定义一个实现 Serializable 接口的类。该类定义了三个属性——两个为字符串类型，另一个表示日期和时间：
- 2. 然后我们定义一个自定义的 serialize() 方法，在序列化对象属性之前初始化日期和时间。unserialize() 方法将值恢复到所有属性：
- 3. 然后我们创建一个实例并使用 var_dump() 检查其内容：
- 4. var_dump() 的输出显示 u 属性尚未初始化：

// /repo/ch05/php8_bc_break_serializable.php
class A implements Serializable {
    private $a = 'A';
    private $b = 'B';
    private $u = NULL;
}
```

```
public function serialize() {
    $this->u = new DateTime();
    return serialize(get_object_vars($this));
}
public function unserialize($payload) {
    $vars = unserialize($payload);
    foreach ($vars as $key => $val)
        $this->$key = $val;
}
```

```
$a1 = new A();
var_dump($a1);
```

```
object(A)#1 (3) {
  ["a":"A":private]=> string(1) "A"
  ["b":"A":private]=> string(1) "B"
  ["u":"A":private]=> NULL
}
```

- 5. 然后我们对其进行序列化，并将其恢复到变量 $a2：

```
$str = serialize($a1);
$a2 = unserialize($str);
var_dump($a2);
```

- 6. 从下面的 var_dump() 输出中，你可以看到对象已完全恢复。此外，我们知道自定义 serialize() 方法已被调用，因为 u 属性已使用日期和时间值初始化。以下是输出：

```
object(A)#3 (3) {
  ["a":"A":private]=> string(1) "A"
  ["b":"A":private]=> string(1) "B"
  ["u":"A":private]=> object(DateTime)#4 (3) {
    ["date"]=> string(26) "2021-02-12 05:35:10.835999"
    ["timezone_type"]=> int(3)
    ["timezone"]=> string(3) "UTC"
  }
}
```

让我们现在来看一下实现 Serializable 接口的对象的序列化过程中的问题。

## 检查 PHP 可序列化接口问题

早期序列化方法存在一个整体问题。如果要序列化的类定义了 __wakeup() 魔术方法，它并不会在反序列化时立即调用。相反，任何已定义的 __wakeup() 魔术方法首先会被排队，整个对象链被反序列化，然后队列中的方法才会被执行。这可能导致对象的 unserialize() 方法所看到的内容与队列中的 __wakeup() 方法所看到的内容不匹配。

这种架构缺陷可能导致在使用实现 Serializable 接口的对象时出现不一致的行为和模糊的结果。许多开发者认为 Serializable 接口严重损坏，因为在嵌套对象序列化发生时需要创建反向引用。这种需求出现在嵌套序列化调用发生的情况中。

例如，当一个类定义的方法进而调用 PHP serialize() 函数时，就可能发生此类嵌套调用。在 PHP 8 之前，反向引用的创建顺序在 PHP 序列化中是预设的，可能导致连锁故障的雪崩。

解决方案是使用两个新的魔术方法来完全控制序列化和反序列化顺序，接下来将介绍。

## 新的魔术方法来控制 PHP 序列化

一种新的序列化控制方式首次在 PHP 7.4 中引入并延续到 PHP 8。为了利用这项新技术，你需要做的就是实现两个魔术方法：__serialize() 和 __unserialize()。如果实现了，PHP 将序列化的控制完全交给 __serialize() 方法。同样，反序列化的控制完全由 __unserialize() 魔术方法控制。如果定义了 __sleep() 和 __wakeup() 方法，则会被忽略。

作为进一步的好处，PHP 8 在以下 SPL 类中提供了对这两个新魔术方法的完全支持：

- ArrayObject
- ArrayIterator
- SplDoublyLinkedList
- SplObjectStorage

> **最佳实践**
> 
> 为了完全控制序列化，请实现新的 __serialize() 和 __unserialize() 魔术方法。你不再需要实现 Serializable 接口，也不再需要定义 __sleep() 和 __wakeup()。有关 Serializable 接口最终弃用的更多信息，请参阅此 RFC：
> https://wiki.php.net/rfc/phase_out_serializable.

作为新的 PHP 序列化用法的示例，请考虑以下代码示例：

- 1. 在示例中，Test 类在实例化时使用随机密钥进行初始化：

// /repo/ch05/php8_bc_break_serialization.php
class Test extends ArrayObject {
    protected $id = 12345;
    public $name = 'Doug';
private $key = '';
public function __construct() {
    $this->key = bin2hex(random_bytes(8));
}
}
```

- 2. 我们添加一个 getKey() 方法来显示当前密钥值：

```
public function getKey() {
    return $this->key;
}
```

- 3. 序列化时，密钥会被从结果字符串中过滤掉：

```
public function __serialize() {
    return ['id' => $this->id,
        'name' => $this->name];
}
```

- 4. 反序列化时，会生成一个新的密钥：

```
public function __unserialize($data) {
    $this->id = $data['id'];
    $this->name = $data['name'];
    $this->__construct();
}
```

- 5. 我们现在创建一个实例，并显示密钥：

```
$test = new Test();
echo "\nOld Key: " . $test->getKey() . "\n";
```

密钥可能如下所示：

```
Old Key: mXq78Dhp1ByDWuPtzK820g==
```

- 6. 我们添加代码来序列化对象并显示字符串：

```
$str = serialize($test);
echo $str . "\n";
```

序列化字符串可能如下所示：

```
O:4:"Test":2:{s:2:"id";i:12345;s:4:"name";s:4:"Doug";}
```

请注意输出中秘密未出现在序列化字符串中。这很重要，因为如果序列化字符串的存储位置被攻破，可能会暴露安全漏洞，给攻击者提供入侵系统的途径。

- 7. 然后我们添加代码来反序列化字符串并显示密钥：

```
$obj = unserialize($str);
echo "New Key: " . $obj->getKey() . "\n";
```

这是最后的输出。请注意，已生成新的密钥：

```
New Key: kDgU7FGfJn5q1OKcHEbyqQ==
```

如你所见，使用新的 PHP 序列化功能并不复杂。任何时序问题现在完全在你的控制之下，因为新的魔术方法是在对象被序列化和反序列化的顺序中执行的。

> **重要说明**
> PHP 7.4 及以上版本*理解*来自旧版本 PHP 的序列化字符串，然而，由 PHP 7.4 或 8.x 序列化的字符串可能无法被旧版本 PHP 正确反序列化。

> **提示**
> 有关完整讨论，请参阅有关自定义序列化的 RFC：
> https://wiki.php.net/rfc/custom_object_serialization

你现在对 PHP 序列化以及两个新魔术方法提供的改进支持有了全面的了解。现在是时候转换思路，检查 PHP 8 如何扩展方差支持。
