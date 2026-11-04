
你可能会想知道为什么这很重要。你需要了解这一重要变更的原因在于，你可能在 __destruct() 方法中存在逻辑，而该方法在你同时调用 exit() 或 die() 的情况下会被调用。在 PHP 8 中，你已无法再依赖这段代码，这可能会导致向后兼容性破坏。

在此示例中，我们有两个连接类。ConnectPdo 使用 PDO 扩展来提供查询结果，而 ConnectMysqli 使用 MySQLi 扩展：

- 1. 我们首先定义一个指定查询方法的接口。该方法需要一个 SQL 字符串作为参数，并预期返回一个数组作为结果：
- 2. 接下来，我们定义一个基类，其中定义了 __destruct() 魔术方法。由于该类实现了 ConnectInterface 但未定义 query()，因此被标记为抽象类：
- 3. 接下来，我们定义 ConnectPdo 类。它继承自 Base，其 query() 方法使用 PDO 语法生成结果。__construct() 方法在创建连接出现问题时会抛出 PDOException：

// /repo/src/Php7/Connector/ConnectInterface.php
namespace Php7\Connector;
interface ConnectInterface {
    public function query(string $sql) : array;
}

// /repo/src/Php7/Connector/Base.php
namespace Php7\Connector;
abstract class Base implements ConnectInterface {
    const CONN_TERMINATED = 'Connection Terminated';
    public $conn = NULL;
    public function __destruct() {
        $message = get_class($this)
            . ':' . self::CONN_TERMINATED;
        error_log($message);
    }
}
```php
// /repo/src/Php7/Connector/ConnectPdo.php
namespace Php7\Connector;
use PDO;
class ConnectPdo extends Base {
    public function __construct(
        string $dsn, string $usr, string $pwd) {
        $this->conn = new PDO($dsn, $usr, $pwd);
    }
    public function query(string $sql) : array {
        $stmt = $this->conn->query($sql);
        return $stmt->fetchAll(PDO::FETCH_ASSOC);
    }
}
```
- 4. 以类似的方式，我们定义 ConnectMysqli 类。它继承自 Base，其 query() 方法使用 MySQLi 语法生成结果。__construct() 方法在创建连接出现问题时会执行 die()：
```php
// /repo/src/Php7/Connector/ConnectMysqli.php
namespace Php7\Connector;
class ConnectMysqli extends Base {
    public function __construct(
        string $db, string $usr, string $pwd) {
        $this->conn = mysqli_connect('localhost',
            $usr, $pwd, $db)
            or die("Unable to Connect\n");
    }
    public function query(string $sql) : array {
        $result = mysqli_query($this->conn, $sql);
        return mysqli_fetch_all($result, MYSQLI_ASSOC);
    }
}
```
- 5. 最后，我们定义一个调用程序，使用前面描述的两个连接类，并为连接字符串、用户名和密码定义无效值：
```php
// /repo/ch05/php8_bc_break_destruct.php
include __DIR__ . '/../../vendor/autoload.php';
use Php7\Connector\{ConnectPdo,ConnectMysqli};
$db = 'test';
$usr = 'fake';
$pwd = 'xyz';
$dsn = 'mysql:host=localhost;dbname=' . $db;
$sql = 'SELECT event_name, event_date FROM events';
$ptn = "%2d : %s : %s\n";
try {
    $conn = new ConnectPdo($dsn, $usr, $pwd);
    var_dump($conn->query($sql));
} catch (Throwable $t) {
    printf($ptn, __LINE__, get_class($t),
        $t->getMessage());
}
$conn = new ConnectMysqli($db, $usr, $pwd);
var_dump($conn->query($sql));
```
- 6. 接下来，在调用程序中，我们调用这两个类并尝试执行查询。连接故意失败，因为我们提供了错误的用户名和密码：
- 7. 如上文讨论所述，PHP 7 中运行的输出显示，在创建 ConnectPdo 实例时，PDOException 从类构造函数中抛出。另一方面，当 ConnectMysqli 实例失败时，会调用 die()，并显示消息 Unable to Connect。你还会在输出的最后一行看到来自 __destruct() 方法的错误日志信息。以下是该输出：
```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_bc_break_destruct.php
15 : PDOException : SQLSTATE[28000] [1045] Access denied for user 'fake'@'localhost' (using password: YES)
PHP Warning: mysqli_connect(): (HY000/1045): Access denied for user 'fake'@'localhost' (using password: YES) in /repo/src/Php7/Connector/ConnectMysqli.php on line 8
Unable to Connect
Php7\Connector\ConnectMysqli:Connection Terminated
```
- 8. 在 PHP 8 中，无论哪种情况 __destruct() 方法都不会被调用，从而产生此处所示的输出。如输出所示，PDOException 被捕获，并且发出 die() 命令。__destruct() 方法没有任何输出。以下是 PHP 8 的输出：
```
root@php8_tips_php8 [ /repo/ch05 ]#
php php8_bc_break_destruct.php
15 : PDOException : SQLSTATE[28000] [1045] Access denied for user 'fake'@'localhost' (using password: YES)
PHP Warning:  mysqli_connect(): (HY000/1045): Access denied for user 'fake'@'localhost' (using password: YES) in /repo/src/Php7/Connector/ConnectMysqli.php on line 8
Unable to Connect
```
现在你已经知道如何识别与 __destruct() 方法以及对 die() 或 exit() 的调用相关的潜在代码破坏问题，让我们将注意力转向 __toString() 方法的变更。

## 处理 __toString() 的变更

__toString() 魔术方法在对象作为字符串使用时被调用。一个经典的例子是当你简单地 echo 一个对象时。echo 命令期望一个字符串作为参数。当提供非字符串数据时，PHP 会执行类型转换以将数据转换为字符串。由于对象不能直接转换为字符串，PHP 引擎随后会检查是否定义了 __toString()，如果是，则返回其值。

此魔术方法的主要变更是引入了 Stringable，这是一个全新的接口。新的接口定义如下：
```
interface Stringable {
    public function __toString(): string;
}
```
在 PHP 8 中定义了 __toString() 魔术方法的任何类都会静默实现 Stringable 接口。这种新行为不会带来严重的代码破坏风险。然而，由于类现在实现了 Stringable 接口，你不再允许修改 __toString() 方法的签名。

下面是一个简短的示例，揭示了与 Stringable 接口的新关联：
- 1. 在此示例中，我们定义一个定义了 __toString() 的 Test 类：
- 2. 然后我们创建该类的一个实例，随后创建一个 ReflectionObject 实例：
// /repo/ch05/php8_bc_break_magic_to_string.php
class Test {
    public $fname = 'Fred';
    public $lname = 'Flintstone';
    public function __toString() : string {
        return $this->fname . ' ' . $this->lname;
    }
}
```
$test = new Test;
$reflect = new ReflectionObject($test);
echo $reflect;
```
在 PHP 7 中运行的输出（此处显示）的前几行只是表明它是一个 Test 类的实例：
```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_bc_break_magic_to_string.php
Object of class [ <user> class Test ] {
@@ /repo/ch05/php8_bc_break_magic_to_string.php 3-12
```
然而，在 PHP 8 中运行相同的代码示例，会揭示与 Stringable 接口的隐式关联：
```
root@php8_tips_php8 [ /repo/ch05 ]#
php php8_bc_break_magic_to_string.php
Object of class [ <user> class Test implements Stringable ] {
@@ /repo/ch05/php8_bc_break_magic_to_string.php 3-12
```
输出显示，即使你没有显式实现 Stringable 接口，该关联也在运行时创建，并由 ReflectionObject 实例揭示。

> **提示**
> 有关魔术方法的更多信息，请查看此文档页面：https://www.php.net/manual/en/language.oop5.magic.php。

现在你已经了解了涉及魔术方法的 PHP 8 代码可能导致代码破坏的情况，让我们来看看序列化过程中的变更。

## 控制序列化

在许多情况下，原生的 PHP 数据需要存储在文件中或数据库表中。当前技术的难题在于，直接存储复杂 PHP 数据（如对象或数组）是不可能的，除非有某些例外情况。

克服这一限制的一种方法是把对象或数组转换为字符串。**JSON（JavaScript 对象表示法）**常因此原因而被选用。一旦数据被转换为字符串，就能轻松存储在任何文件或数据库中。然而，使用 JSON 格式化对象存在问题。虽然 JSON 足以很好地表示对象属性，但它无法直接恢复原始对象的类和方法。

为解决这一不足，PHP 语言包含两个原生函数，`serialize()` 和 `unserialize()`，它们可以轻松地将对象或数组转换为字符串并恢复到原始状态。尽管听起来很棒，但原生 PHP 序列化存在一些问题。

在我们正确讨论现有 PHP 序列化架构的问题之前，我们需要更仔细地了解原生 PHP 序列化是如何工作的。

## 理解 PHP 序列化

当 PHP 对象或数组需要保存到非面向对象环境（如平面文件或关系数据库表）时，可以使用 `serialize()` 将对象或数组 *展平* 为适合存储的字符串。相反，`unserialize()` 会恢复原始对象或数组。
