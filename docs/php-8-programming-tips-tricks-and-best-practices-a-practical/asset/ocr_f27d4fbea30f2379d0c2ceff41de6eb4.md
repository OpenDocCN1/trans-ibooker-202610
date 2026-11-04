# Improving code using typed properties

In the first section of this chapter, *Using constructor property promotion*, we discussed how data types can be used to control the type of data supplied as arguments to functions or class methods. What this approach fails to do, however, is guarantee that the data type never changes. In this section, you will learn how assigning a data type at the property level provides stricter control over the use of variables in PHP 8.

# What are typed properties?

This extremely important feature was introduced in PHP 7.4 and continues in PHP 8. Simply put, a **typed property** is a class property with a data type preassigned. Here is a simple example:

```
// /repo/ch01/php8_prop_type_1.php
declare(strict_types=1)
class Test {
    public int $id = 0;
    public int $token = 0;
    public string $name = '';
}
$test = new Test();
$test->id = 'ABC';
```

In this example, if we attempt to assign a value representing a data type other than int to $test->id, a Fatal error is thrown. Here is the output:

```
Fatal error: Uncaught TypeError: Cannot assign string to property Test::$id of type int in /repo/ch01/php8_prop_type_1.php:11 Stack trace: #0 {main} thrown in /repo/ch01/php8_prop_type_1.php on line 11
```

As you can see from the preceding output, a Fatal error is thrown when the wrong data type is assigned to a typed property.

You have already been exposed to one form of property typing: **constructor property promotion**. All properties defined using constructor property promotion are automatically property typed!