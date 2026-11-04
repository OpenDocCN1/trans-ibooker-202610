
Here is a simple example that demonstrates this concept:

- 1. First, we define a class with three properties:
- 2. We then create an instance, serialize the instance, and display the resulting string:
- 3. Here is how the serialized object appears:
- 4. We then unserialize the object into a new variable and use var_dump() to examine the two:

```
// /repo/ch05/php8_serialization.php
class Test {
    public $name = 'Doug';
    private $key = 12345;
    protected $status = ['A','B','C'];
}
```

```
$test = new Test();
$str = serialize($test);
echo $str . "\n";
```

```
O:4:"Test":3:{s:4:"name";s:4:"Doug";s:9:"Testkey";
i:12345;
s:9:"*status";a:3:{i:0;s:1:"A";i:1;s:1:"B";i:2;s:1:"C";}}
```

As you can see from the serialized string, the letter O designates Object, a is for array, s is for string and i is for integer.

```
$obj = unserialize($str);
var_dump($test, $obj);
```

5. Placing the var_dump() outputs side by side, you can clearly see that the restored object is identical to the original:

```
object(Test)#1 (3) {
  ["name"]=> string(4)
  "Doug"
  ["key":"Test":private]=> int(12345)
  ["status":protected]=>
  array(3) {
    [0]=> string(1)
    "A"
    [1]=> string(1)
    "B"
    [2]=> string(1)
    "C"
  }
}
```

```
object(Test)#2 (3) {
  ["name"]=> string(4)
  "Doug"
  ["key":"Test":private]=> int(12345)
  ["status":protected]=>
  array(3) {
    [0]=> string(1)
    "A"
    [1]=> string(1)
    "B"
    [2]=> string(1)
    "C"
  }
}
```

Let's now have a look at the magic methods that supply legacy PHP serialization support: __sleep() and __wakeup().

## Understanding the __sleep() magic method

The purpose of the __sleep() magic method is to provide a filter used to prevent certain properties from appearing in the serialized string. To use a user object as an example, you may wish to exclude sensitive properties such as a national identification number, credit card number, or password from the serialization.

Here is an example using the __sleep() magic method to exclude a password:

- 1. First, we define a Test class with three properties:

```
// /repo/ch05/php8_serialization_sleep.php
class Test {
    public $name = 'Doug';
    protected $key = 12345;
    protected $password = '$2y$10$ux07vQNSA0ctbzZcZNA' .
        '1xOa8hi6kchJrJZzqWcxpw/XQUjSNqacx.';
}
```

2. We then define a __sleep() method that excludes the $password property:

```
public function __sleep() {
    return ['name','key'];
}
}
```

3. We then create an instance of this class and serialize it. The last line echoes the state of the serialized string:

```
$test = new Test();
$str = serialize($test);
echo $str . "\n";
```

4. In the output, you can clearly see that the $password property is not present. Here is the output:

```
O:4:"Test":2:{s:4:"name";s:4:"Doug",s:6:"*key";i:12345;}
```

This is important in that, in most cases, the reason you need to serialize an object is you wish to store it somewhere, whether that be in a session file or in a database. If the filesystem or database is subsequently compromised, you have one less security vulnerability to worry about!

## Understanding a potential code break in the __sleep() method

There is a potential code break involving the __sleep() magic method. In versions prior to PHP 8, if __sleep() returns an array with non-existent properties, they are still serialized and assigned a value of NULL. The problem with this approach is that when the object is subsequently unserialized, an extra property now appears, one that is not there by design!

In PHP 8, non-existent properties in the __sleep() magic method return are silently ignored. If your legacy code anticipates the old behavior and takes steps to delete the unwanted property, or even worse, if your code assumes the unwanted property exists, you will ultimately have an error. Such assumptions are extremely dangerous as they can lead to unexpected code behavior.

To illustrate the issue, have a look at the following code example:

1. First, we define a Test class that defines __sleep() to return a variable that doesn't exist:

```
class Test {
    public $name = 'Doug';
    public function __sleep() {
        return ['name', 'missing'];
    }
}
```

2. Next, we create an instance of Test and serialize it:

```
echo "Test instance before serialization:\n";
$test = new Test();
var_dump($test);
```

3. We then unserialize the string into a new instance, $restored:

```
echo "Test instance after serialization:\n";
$stored = serialize($test);
$restored = unserialize($stored);
var_dump($restored);
```

4. In theory, the two object instances $test and $restored should be the same. However, have a look at the output running in PHP 7:

```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_bc_break_sleep.php
Test instance before serialization:
/repo/ch05/php8_bc_break_sleep.php:13:
class Test#1 (1) {
    public $name => string(4) "Doug"
}
Test instance after serialization:
PHP Notice: serialize(): "missing" returned as member variable from __sleep() but does not exist in /repo/ch05/php8_bc_break_sleep.php on line 16
class Test#2 (2) {
    public $name => string(4) "Doug"
public $missing => NULL
}
```

5. As you can see from the output, the two objects are clearly *not* the same! However, in PHP 8, the non-existent property is ignored. Have a look at the same script running in PHP 8:

```
root@php8_tips_php8 [ /repo/ch05 ]# php php8_bc_break_sleep.php
Test instance before serialization:
object(Test)#1 (1) {
  ["name"]=> string(4) "Doug"
}
Test instance after serialization:
PHP Warning: serialize(): "missing" returned as member variable from __sleep() but does not exist in /repo/ch05/php8_bc_break_sleep.php on line 16
object(Test)#2 (1) {
  ["name"]=> string(4) "Doug"
}
```

You might also observe that in PHP 7, a Notice is issued, whereas in PHP 8, the same situation produces a Warning. A pre-migration check for a potential code break in this case is difficult because you would need to determine, if the magic method __sleep() is defined, whether or not a non-existent property is being included in the list.

Let's now have a look at the counterpart method, __wakeup().

## Learning about __wakeup()

The purpose of the __wakeup() magic method is mainly to perform additional initialization on the unserialized object. Examples would be to restore a database connection or reinstate a file handle. Here's a very simple example that uses __wakeup() magic to re-open a file handle:

- 1. First, we define a class that opens a file handle upon instantiation. We also define a method that returns the contents of the file:

```
// /repo/ch05/php8_serialization_wakeup.php
class Gettysburg {
    public $fn = __DIR__ . '/gettysburg.txt';
    public $obj = NULL;
    public function __construct() {
        $this->obj = new SplFileObject($this->fn, 'r');
    }
    public function getText() {
        $this->obj->rewind();
        return $this->obj->fpassthru();
    }
}
```

2. To use the class, create an instance, and run getText(). (This assumes that the file referenced by $this->fn exists!)

```
$old = new Gettysburg();
echo $old->getText();
```

3. The output (not shown) is the Gettysburg Address.

4. If we now attempt to serialize this object, a problem arises. Here's an example of code that would serialize the object:

```
$str = serialize($old);
```

5. At this point, running the code in place so far, here is the output:

```
PHP Fatal error: Uncaught Exception: Serialization of 'SplFileObject' is not allowed in /repo/ch05/php8_serialization_wakeup.php:19
```

6. In order to fix this problem, we return to the class and add a __sleep() method that prevents the SplFileObject instance from being serialized:

```
public function __sleep() {
    return ['fn'];
}
```

7. If we then rerun the code to serialize the object, all is well. Here is the code to unserialize and call getText():

```
$str = serialize($old);
$new = unserialize($str);
echo $new->getText();
```
