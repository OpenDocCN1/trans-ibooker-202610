As expected, the PHP 8 output is much more consistent. In PHP 8, the v*printf() functions are strictly typed to accept only an array as an argument. Unfortunately, there's a real possibility you may have been using ArrayObject. This is easily addressed by simply using the getArrayCopy() method on the ArrayObject instance, which returns an array.

Here is the rewritten code that works in both PHP 7 and PHP 8:

```
```php
if ($value instanceof ArrayObject)
    $value = $value->getArrayCopy();
echo $key . ':' . vsprintf($patt, $value);
```
```

Now that you have an idea where to look for a potential code break when using the v*printf() functions, let's turn our attention to differences in how string functions with a null length argument work in PHP 8.

# Working with null length arguments in PHP 8

In PHP 7 and earlier, a NULL length argument resulted in an empty string. In PHP 8, a NULL length argument is now treated the same as if the length argument is omitted. Functions affected include the following:

+   - substr()
+   - substr_count()
+   - substr_compare()
+   - iconv_substr()

In the example shown next, PHP 7 returns an empty string whereas PHP 8 returns the remainder of the string. This has a high potential for a code break if the result of the operation is used to confirm or deny the existence of the substring:

1. First, we define a haystack and needle. We then run strpos() to get the position of the needle in the haystack:

```
```php
// /repo/ch06/php8_null_length_arg.php
$str = 'The quick brown fox jumped over the fence';
$var = 'fox';
$pos = strpos($str, $var);
```
```