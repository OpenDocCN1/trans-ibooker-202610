2. We then create two hashes using the crypt() function. In the first usage, $default is the result after supplying an invalid salt argument. The second usage, $sha256, provides a valid salt value, but an invalid number of rounds:

```
$default = crypt($password, $salt);
$sha256 = crypt($password,
'$5$rounds=' . $rounds . '$' . $salt . '$');
echo "Default : $default\n";
echo "SHA-256 : $sha256\n";
```

Here is the output of the code example running in PHP 7:

```
root@php8_tips_php7 [ /repo/ch06 ]#
php php8_crypt_sha256.php
PHP Deprecated: crypt(): Supplied salt is not valid for DES. Possible bug in provided salt format. in /repo/ch06/php8_crypt_sha256.php on line 7
Default : +xj31ZMTZkVA
SHA-256 : $5$rounds=1000$+x=+x=+x=+x=+x=+
$3Si/vFn6/xmdTdyleJ17Rb9Heg6DWgkRVKS9T0Zzy/B
```

Notice how PHP 7 silently modifies the original request. In the first case, crypt() falls back to DES (!). In the second case, PHP 7 silently alters the rounds value from 1 to the nearest limit of 1000.

The same code running in PHP 8, on the other hand, fails and returns *0, as shown here:

```
root@php8_tips_php8 [ /repo/ch06 ]#
php php8_crypt_sha256.php
Default : *0
SHA-256 : *0
```

As we have stressed repeatedly in this book, when PHP makes assumptions for you, ultimately you end up with bad code that produces inconsistent results. In the code example just shown, the best practice would be to define a class method or function that exerts greater control over its parameters. In this manner, you can validate the parameters and avoid having to rely upon PHP assumptions.

Next, we take a look at changes to the password_hash() function.