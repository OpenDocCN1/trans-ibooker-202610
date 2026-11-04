```
... not all output is shown ...
/repo/ch06/php7_curly_brace_usage.php
/repo/ch06/php7_compare_num_str_valid.php
/repo/ch06/php8_compare_num_str.php
/repo/ch06/php8_disabled_funcs_redefine.php
```

You now have an idea of how to work with disabled functions. Next, let's have a look at changes to the vital crypt() function.

# Learning about changes to the crypt() function

The crypt() function has been a staple of PHP hash generation since PHP version 4. One of the reasons for its resilience is because it has so many options. If your code uses crypt() directly, you'll be pleased to note that if an unusable salt value is provided, Defense Encryption Standard (DES), long considered broken, is no longer the fallback in PHP 8! The salt is also sometimes referred to as the initialization vector (IV).

Another important change involves the rounds value. A round is like shuffling a deck of cards: the more times you shuffle, the higher the degree of randomization (unless you're dealing with a Las Vegas card shark!). In cryptography, blocks are analogous to cards. During each round, a cryptographic function is applied to each block. If the cryptographic function is simple, the hash can be generated more quickly; however, a larger number of rounds are required to fully randomize the blocks.

The SHA-1 (Secure Hash Algorithm) family uses a fast but simple algorithm, and thus requires more rounds. The SHA-2 family, on the other hand, uses a more complex hashing function, which takes more resources, but fewer rounds.

When using the PHP crypt() function in conjunction with CRYPT_SHA256, (SHA-2 family), PHP 8 will no longer silently resolve the rounds parameter to the closest limit. Instead, crypt() will fail with a *0 return, matching glibc behavior. In addition, in PHP 8, the second argument (the salt), is now mandatory.

The following example illustrates the differences between PHP 7 and PHP 8 when using the crypt() function:

+   1. First, we define variables representing an unusable salt value, and an illegal number of rounds:

```
// /repo/ch06/php8_crypt_sha256.php
$password = 'password';
$salt = str_repeat('+x=', CRYPT_SALT_LENGTH + 1);
$rounds = 1;
```