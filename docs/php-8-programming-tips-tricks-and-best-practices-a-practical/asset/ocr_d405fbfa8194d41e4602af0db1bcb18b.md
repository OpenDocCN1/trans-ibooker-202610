The unset typecast has also been removed. The purpose of this typecast is to unset a variable. In the following code snippet, the value of $obj becomes NULL:

```
```php
$obj = new ArrayObject();
/* some code (not shown) */
$obj = (unset) $obj;
```
```

A best practice in PHP 8 is to use either of the following:

```
```php
$obj = NULL;
// or this:
unset($obj);
```
```

Let's now turn our attention to anonymous functions.

# Changes in generating anonymous functions from class methods

In PHP 7.1, a new Closure::fromCallable() method was added that allows you to return a class method as a Closure instance (for example, an anonymous function). ReflectionMethod::getClosure() was also introduced and is also able to convert a class method into an anonymous function.

To illustrate this, we define a class that returns Closure instances able to perform hashing using different algorithms. We'll proceed as follows:

+   1. First, we define a class and a public $class property, as follows:
+   2. We then define a method that produces one of three callbacks, each designed to produce a different type of hash, as follows:

```
```php
// /repo/src/Services/HashGen.php
namespace Services;
use Closure;
class HashGen {
    public $class = 'HashGen: ';
}
```
```

```
```php
public function makeHash(string $type) {
    $method = 'hashTo' . ucfirst($type);
    if (method_exists($this, $method))
        return Closure::fromCallable(
            [$this, $method]);
    else
```
```