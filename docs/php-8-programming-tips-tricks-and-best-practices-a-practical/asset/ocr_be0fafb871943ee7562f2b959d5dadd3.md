Any code that relies upon this functionality will break. The only way to avoid serious application failure is to rewrite the affected code and replace the silently substituted PHP core string functions with the intended mbstring extension functions.

In the following example, when mbstring.func_overload is enabled, PHP 7 reports the same values for both strlen() and mb_strlen():

```
// /repo/ch08/php7_mbstring_func_overload.php
$str = 'วันนี้สบายดีไหม';
$len1 = strlen($str);
$len2 = mb_strlen($str);
echo "Length of '$str' using 'strlen()' is $len1\n";
echo "Length of '$str' using 'mb_strlen()' is $len2\n";
```

Here is the output in PHP 7:

```
root@php8_tips_php7 [ /repo/ch08 ]#
php php7_mbstring_func_overload.php
Length of 'วันนี้สบายดีไหม' using 'strlen()' is 45
Length of 'วันนี้สบายดีไหม' using 'mb_strlen()' is 15

root@php8_tips_php7 [ /repo/ch08 ]#
echo "mbstring.func_overload=7" >> /etc/php.ini

root@php8_tips_php7 [ /repo/ch08 ]#
php php7_mbstring_func_overload.php
Length of 'วันนี้สบายดีไหม' using 'strlen()' is 15
Length of 'วันนี้สบายดีไหม' using 'mb_strlen()' is 15
```

As you can see from the preceding output, once the mbstring.func_overload setting is enabled in the php.ini file, the results reported by strlen() and mb_strlen() are identical. This is because calls to strlen() are silently diverted to mb_strlen() instead. In PHP 8, the output (not shown) shows the results in both cases because the mbstring.func_overload setting is ignored. strlen() reports a length of 45, and mb_strlen() reports a length of 15.

To determine if your code is vulnerable to this backward-compatible break, check your php.ini file and see if the mbstring.func_overload setting has a value other than zero.