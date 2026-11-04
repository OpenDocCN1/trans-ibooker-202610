3. We then define the arguments as an array, $arr, and use vprintf() to produce the same result:

```
$arr = [$ord, $day, $pos, $date->format('l, d M Y')];vprintf($patt, $arr);
```

Here is the output of the program running in PHP 8. The output is the same running in PHP 7 (not shown):

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_printf_vs_vprintf.php
The third Thursday of next month is: Thursday, 15 Apr 2021
The third Thursday of next month is: Thursday, 15 Apr 2021
```

As you can see, the output of both functions is identical. The only usage difference is that vprintf() accepts the parameters in the form of an array.

Prior versions of PHP allowed a developer to play fast and loose with arguments presented to the v*printf() family of functions. In PHP 8, the data type of the arguments is now strictly enforced. This only presents a problem where code controls do not exist to ensure that an array is presented. Another even more important difference is that PHP 7 will allow ArrayObject with v*printf(), whereas PHP 8 will not.

In the example shown here, PHP 7 issues a Warning, whereas PHP 8 throws an Error:

1. First, we define the pattern and the source array:

```
// /repo/ch06/php7_vprintf_bc_break.php
$patt = "\t%s. %s. %s. %s. %s.";
$arr = ['Person', 'Woman', 'Man', 'Camera', 'TV'];
```

2. We then define a test data array in order to test which arguments are accepted by vsprintf():

```
$args = [
    'Array' => $arr,
    'Int'   => 999,
    'Bool'  => TRUE,
    'Obj'   => new ArrayObject($arr)
];
```