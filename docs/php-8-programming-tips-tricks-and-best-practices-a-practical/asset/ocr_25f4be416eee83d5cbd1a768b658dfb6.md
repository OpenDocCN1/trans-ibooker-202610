Here is a code example that searches for the presence of LF within a string. However, note that instead of providing a string as an argument, an integer with a value of 10 is provided:

```
```php
// /repo/ch06/php8_num_str_needle.php
function search($needle, $haystack) {
    $found = (strpos($haystack, $needle))
        ? 'contains' : 'DOES NOT contain';
    return "This string $found LF characters\n";
}
$haystack = "We're looking\nFor linefeeds\nIn this
    string\n";
$needle = 10;        // ASCII code for LF
echo search($needle, $haystack);
```
```

Here are the results of the code sample running in PHP 7:

```
```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_num_str_needle.php
This string contains LF characters
```
```

And here are the results of the same code block running in PHP 8:

```
```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_num_str_needle.php
This string DOES NOT contain LF characters
```
```

As you can see, comparing the output in PHP 7 with the output in PHP 8, the same code block yields radically different results. This is an extremely difficult potential code break to spot as no Warnings or Errors are generated.

The best practice is to apply a string type hint to the needle argument of any function or method that incorporates one of the PHP str*() functions. If we rewrite the previous example, the output is consistent in both PHP 7 and PHP 8. Here is the same example rewritten using a type hint:

```
```php
// /repo/ch06/php8_num_str_needle_type_hint.php
declare(strict_types=1);
function search(string $needle, string $haystack) {
    $found = (strpos($haystack, $needle))
        ? 'contains' : 'DOES NOT contain';
```
```