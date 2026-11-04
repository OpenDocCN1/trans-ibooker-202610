# Dealing with changes to password_hash()

Over the years, so many developers had misused crypt() that the PHP core team decided to add a wrapper function, password_hash(). This proved to be a smashing success and is now one of the most widely used security functions. Here is the function signature for password_hash():

```
password_hash(string $password, mixed $algo, array $options=?)
```

Algorithms currently supported include bcrypt, Argon2i, and Argon2id. It's recommended that you use the predefined constants for algorithms: PASSWORD_BCRYPT, PASSWORD_ARGON2I, and PASSWORD_ARGON2ID. The PASSWORD_DEFAULT algorithm is currently set to bcrypt. Options vary according to the algorithm. If you use either PASSWORD_BCRYPT or the PASSWORD_DEFAULT algorithms, the options include cost and salt.

Conventional wisdom suggests that it's better to use the randomly generated salt created by the password_hash() function. In PHP 7, the salt option was deprecated and is now ignored in PHP 8. This won't cause a backward-compatible break unless you're relying on salt for some other reason.

In this code example, a non-random salt value is used:

```
// /repo/ch06/php8_password_hash.php
$salt = 'xxxxxxxxxxxxxxxxxxxxxxxx';
$password = 'password';
$hash = password_hash(
    $password, PASSWORD_DEFAULT, ['salt' => $salt]);
echo $hash . "\n";
var_dump(password_get_info($hash));
```

In the PHP 7 output, a deprecation Notice is issued:

```
root@php8_tips_php7 [ /repo/ch06 ]# php php8_password_hash.php
PHP Deprecated: password_hash(): Use of the 'salt' option to password_hash is deprecated in /repo/ch06/php8_password_hash.php on line 6
$2y$10$xxxxxxxxxxxxxxxxxxxxxxu0d9YtxiLKHM/198x//sqUV1V2XTZEZ.
/repo/ch06/php8_password_hash.php:8:
array(3) {
  'algo' => int(1)
```