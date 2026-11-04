
- With bitwise operators
- When incrementing or decrementing a variable whose value is a non-well-formed numeric string

Here are some suggestions to fix your code:

- Consider using trim() on numeric strings that might include leading or trailing white space (for example, numeric strings embedded within posted form data).
- If your code relies upon strings that start with a number, use an explicit typecast to ensure that the number is correctly interpolated.
- Do not rely upon an empty string (for example, $str = '' ) to cleanly convert to 0.

In this following code example, a non-well-formed string with a trailing space is assigned to $age:

```php
// /repo/ch06/php8_num_str_is_numeric.php
$age = '77  ';
echo (is_numeric($age))
    ? "Age must be a number\n"
    : "Age is $age\n";
```

When we run this code in PHP 7, is_numeric() returns TRUE. Here is the PHP 7 output:

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_num_str_is_numeric.php
Age is 77
```

On the other hand, when we run this code in PHP 8, is_numeric() returns FALSE as the string is not considered numeric. Here is the PHP 8 output:

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_num_str_is_numeric.php
Age must be a number
```

As you can see, string handling differences between PHP 7 and PHP 8 can cause applications to behave differently, with potentially disastrous results. Let's now have a look at inconsistent results involving well-formed strings.

## Dealing with inconsistent string-to-numeric comparison results

To complete a non-strict comparison involving string and numeric data, the PHP engine first performs a type-juggling operation that internally converts the string to a number before performing the comparison. Even a well-formed numeric string, however, can yield results that would be viewed as nonsensical from a human perspective.

As an example, have a look at this code sample:

- 1. First we perform a non-strict comparison between a variable, $zero, with a value of zero and a variable, $string, with a value of ABC:
- 2. The following non-strict comparison uses in_array() to locate a value of zero in the $array array:
- 3. Finally, we perform a non-strict comparison between a leading-numeric string, 42abc88, and a hardcoded number, 42:

```
$zero = 0;
$string = 'ABC';
$result = ($zero == $string) ? 'is' : 'is not';
echo "The value $zero $result the same as $string\n"
```

```
$array = [1 => 'A', 2 => 'B', 3 => 'C'];
$result = (in_array($zero, $array))
    ? 'is in' : 'is not in';
echo "The value $zero $result\n"
    . var_export($array, TRUE)
```

```
$mixed = '42abc88';
$result = ($mixed == 42) ? 'is' : 'is not';
echo "\nThe value $mixed $result the same as 42\n";
```

The results running in PHP 7 defy human comprehension! Here are the PHP 7 results:

```
root@php8_tips_php7 [/repo/ch06 ]#
php php7_compare_num_str.php
The value 0 is the same as ABC
The value 0 is in
array (1 => 'A', 2 => 'B', 3 => 'C')
The value 42abc88 is the same as 42
```

From a human perspective, none of these results make any sense! From the computer's perspective, on the other hand, it makes perfect sense. The string ABC, when converted to a number, ends up with a value of zero. Likewise, when the array search is made, each array element, having only a string value, ends up being interpolated as zero.

The case of the leading-numeric string is a bit trickier. In PHP 7, the interpolation algorithm converts numeric characters until the first non-numeric character is encountered. Once that happens, the interpolation stops. Accordingly, the string 42abc88 becomes an integer, 42, for comparison purposes. Now let's have a look at how PHP 8 handles string-to-numeric comparisons.

## Understanding comparison changes made in PHP 8

In PHP 8, if a string is compared with a number, only numeric strings are considered valid for comparison. Strings in exponential notation are also considered valid for comparison, as well as numeric strings with leading or trailing whitespace. It's extremely important to note that PHP 8 makes this determination before converting the string.

Have a look at the output of the same code example described in the previous subsection (Dealing with inconsistent string-to-numeric comparison results), running in PHP 8:

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php7_compare_num_str.php
The value 0 is not the same as ABC
The value 0 is not in
array (1 => 'A', 2 => 'B', 3 => 'C')
The value 42abc88 is not the same as 42
```

So, as you can see from the output, there is a massive potential for your application to change its behavior following a PHP 8 upgrade. As a final note in PHP 8 string handling, let's look at how you can avoid upgrade issues.

## Avoiding problems during a PHP 8 upgrade

The main issue you face is the difference in how PHP 8 handles non-strict comparisons that involve operands with different data types. If one operand is either int or float, and the other operand is string, you have a potential problem post-upgrade. If the string is a valid numeric string, the non-strict comparison will proceed without any issues.

The following operators are affected: <==>, ==, !=, >, >=, <, and <=. The following functions are affected if the option flags are set to default:

- `in_array()`
- `array_search()`
- `array_keys()`
- `sort()`
- `rsort()`
- `asort()`
- `arsort()`
- `array_multisort()`

> **Tip**
> 
> For more information on improved numeric string handling in PHP 8, refer to the following link: https://wiki.php.net/rfc/saner-numeric-strings. A related PHP 8 change is documented here: https://wiki.php.net/rfc/string_to_number_comparison.

The best practice is to minimize PHP type juggling by providing type hints for functions or methods. You can also force the data type before the comparison. Finally, consider making use of strict comparisons, although this might not be suitable in all situations.

Now that you have an understanding of how to properly handle comparisons involving numeric strings in PHP 8, let's now have a look at PHP 8 changes involving arithmetic, bitwise, and concatenation operations.

## Handling differences in arithmetic, bitwise, and concatenation operations

Arithmetic, bitwise, and concatenation operations are at the heart of any PHP application. In this section, you learn about hidden dangers that might arise in these simple operations following a PHP 8 migration. You must learn about the changes made in PHP 8 so that you can avoid a potential code break in your application. Because these operations are so ordinary, without this knowledge, you will be hard pressed to discover post-migration errors.

Let's first have a look at how PHP handles non-scalar data types in arithmetic and bitwise operations.

## Handling non-scalar data types in arithmetic and bitwise operations

Historically, the PHP engine has been very *forgiving* about using mixed data types in an arithmetic or bitwise operation. We've already had a look at comparison operations that involve *numeric*, *leading-numeric*, and *non-numeric* strings and numbers. As you learned, when a non-strict comparison is used, PHP invokes type juggling to convert the string to a number before performing the comparison. A similar action takes place when PHP performs an arithmetic operation that involves numbers and strings.

Prior to PHP 8, **non-scalar data types** (data types other than `string`, `int`, `float`, or `boolean`) were allowed in an arithmetic operation. PHP 8 has clamped down on this bad practice, and no longer allows operands of the `array`, `resource`, or `object` type. PHP 8 consistently throws a `TypeError` when the non-scalar operands are used in an arithmetic operation. The only exception to this general change is that you can still perform arithmetic operations where all operands are of the `array` type.

> **Tip**
> For further information on the vital change in arithmetic and bitwise operations, have a look here: https://wiki.php.net/rfc/arithmetic_operator_type_checks.

Here is a code example to illustrate arithmetic operator handling differences in PHP 8:

- 1. First, we define sample non-scalar data to test in an arithmetic operation:
- 2. We then attempt to add the integer 99 to a resource, object, and to perform a modulus operation on an array:

```php
// /repo/ch06/php8_arith_non_scalar_ops.php
$fn = __DIR__ . '../sample_data/gettysburg.txt';
$fh = fopen($fn, 'r');
$obj = new class() { public $val = 99; };
$arr = [1,2,3];
```

```php
echo "Adding 99 to a resource\n";
try { var_dump($fh + 99); }
catch (Error $e) { echo $e . "\n"; }
```
