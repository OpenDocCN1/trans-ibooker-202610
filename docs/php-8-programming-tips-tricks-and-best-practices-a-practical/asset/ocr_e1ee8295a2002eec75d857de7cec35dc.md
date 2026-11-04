In this code example, a cURL connection is initialized for an external website. The next few lines test for success using the is_resource() function:

```
```php
// ///repo/ch07/php7_ext_is_resource.php
$url = 'https://unlikelysource.com/';
$ch  = curl_init($url);
if (is_resource($ch))
    echo "Connection Established\n"
else
    throw new Exception('Unable to establish connection');
```
```

The following output from PHP 7 shows success:

```
```
root@php8_tips_php7 [ /repo/ch07 ]#
php php7_ext_is_resource.php
Connection Established
```
```

The output of the same code running in PHP 8 is not successful, as we can see here:

```
```
root@php8_tips_php8 [ /repo/ch07 ]#
php php7_ext_is_resource.php
PHP Fatal error:  Uncaught Exception: Unable to establish connection in /repo/ch07/php7_ext_is_resource.php:9
```
```

The output from PHP 8 is deceptive in that a connection has been established! Because the program code is checking to see if the cURL handle is a resource, however, the code throws an Exception error. The reason for the failure is because a CurlHandle instance is returned rather than a resource.

In this situation, you can avoid a code break and have the code run successfully in both PHP 8 and any earlier PHP version by substituting !empty() (not empty) in place of is_resource(), as shown here:

```
```php
// ///repo/ch07/php8_ext_is_resource.php
$url = 'https://unlikelysource.com/';
$ch  = curl_init($url);
if (!empty($ch))
    echo "Connection Established\n";
else
    throw new Exception('Unable to establish connection');
var_dump($ch);
```
```