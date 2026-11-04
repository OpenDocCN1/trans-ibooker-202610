We now add lines of code attempting to access a non-existent static property, as follows:

```
try {
    echo Test::$stat;
    echo Test::$does_not_exist;
} catch (Error $e) {
    echo __LINE__ . ':' . $e;
}
```

Interestingly, both PHP 7 and PHP 8 now issue a fatal error, as seen here:

```
STATIC
22:Error: Access to undeclared static property Test::$does_not_exist in /repo/ch03/php8_warn_undef_prop.php:20
```

Anytime a block of code that previously issued a Warning now issues an Error is cause for concern. If possible, scan your code for static references to static class properties and make sure they are defined. Otherwise, after a PHP 8 upgrade, your code will fail.

Let's now have a look at non-existent offset handling.

# Non-existent offset handling

As mentioned in the previous section, in general, Notices have been promoted to Warnings where data is read, whereas Warnings have been promoted to Errors where data is written (and could conceivably result in lost data). The handling of non-existent offsets follows this logic.

In the following example, an array key is drawn from a string. In both cases, the offset doesn't exist:

```
// /repo/ch03/php8_warn_undef_array_key.php
$key = 'ABCDEF';
$vals = ['A' => 111, 'B' => 222, 'C' => 333];
echo $vals[$key[6]];
```

In PHP 7, the result is a Notice, as seen here:

```
PHP Notice: Uninitialized string offset: 6 in /repo/ch03/php8_warn_undef_array_key.php on line 6
PHP Notice: Undefined index:  in /repo/ch03/php8_warn_undef_array_key.php on line 6
```