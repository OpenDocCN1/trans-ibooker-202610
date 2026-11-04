2. We then run three tests that each check for the existence of $var, using isset(), property_exists(), and array_key_exists():

```
// not all code is shown
$default = 'DEFAULT';
echo (isset($obj->var))
    ? $obj->var : $default;
echo (property_exists($obj,'var'))
    ? $obj->var : $default;
echo (array_key_exists('var',$obj))
    ? $obj->var : $default;
```

When we run this code in PHP 7, all tests succeed, as shown here:

```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_oop_diff_array_key_exists.php
OK.OK.OK.
```

In PHP 8, however, a fatal TypeError occurs, as array_key_exists() now only accepts an array as an argument. The PHP 8 output is shown here:

```
root@php8_tips_php8 [ /repo/ch05 ]#
php php8_oop_diff_array_key_exists.php
OK.OK.PHP Fatal error: Uncaught TypeError: array_key_exists(): Argument #2 ($array) must be of type array, class@anonymous given in /repo/ch05/php8_oop_diff_array_key_exists.php:10
```

The best practice is to use either property_exists() or isset(). We now turn our attention to changes in PHP autoloading.

# Working with PHP 8 autoloading

The basic autoloading class mechanism first introduced in PHP 5.1 works the same in PHP 8. The main difference is that the support for the global function __autoload(), deprecated in PHP 7.2, has been completely removed in PHP 8. Starting with PHP 7.2, developers were encouraged to register their autoloading logic using spl_autoload_register(), available for that purpose since PHP 5.1. Another major difference is how spl_autoload_register() reacts if unable to register an autoloader.