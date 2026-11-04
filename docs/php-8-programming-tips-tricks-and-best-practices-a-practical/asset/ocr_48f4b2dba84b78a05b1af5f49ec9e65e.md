Note that in PHP 8, many functions that formerly produced a resource now produce an object instead. This topic is covered in *Chapter 7, Avoiding Traps When Using PHP 8 Extensions*.

> Tip
> **Best practice**: Do not use resource IDs as array offsets!

We now turn our attention to string-related Notices promoted to Warnings in the case of ambiguous string offsets.

# Ambiguous string offset cast

Turning our attention to string handling, we once again revisit the idea of identifying a single character in a string using array syntax. An **ambiguous string offset cast** might occur if PHP has to perform an internal type cast in order to evaluate a string offset, but where in that type-cast is not clear.

In this very simple example, we define a string that contains all the letters of the alphabet. We then define an array of keys with these values: NULL; a Boolean, TRUE; and a float, 22/7 (the approximate value of Pi). We then loop through the keys and attempt to use the key as a string offset, as illustrated here:

```
```php
// /repo/ch03/php8_warn_amb_offset.php
$str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
$ptr = [ NULL, TRUE, 22/7 ];
foreach ($ptr as $key) {
    var_dump($key);
    echo $str[$key];
}
```
```

As you might have anticipated, the output running in PHP 7 produces the output A, B, and D, along with a series of Notices, shown here:

```
```
NULL
PHP Notice: String offset cast occurred in /repo/ch03/php8_warn_amb_offset.php on line 8
A
/repo/ch03/php8_warn_amb_offset.php:7:
bool(true)
PHP Notice: String offset cast occurred in /repo/ch03/php8_
```
```