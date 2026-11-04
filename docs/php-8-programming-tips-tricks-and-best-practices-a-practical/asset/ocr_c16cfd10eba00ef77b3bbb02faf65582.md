# Dealing with changes to backtraces

Amazingly, before PHP 8, it was possible to change function arguments through a backtrace. This was possible because traces produced by either debug_backtrace() or Exception::getTrace() provided access to function arguments by reference.

This is an extremely bad practice because it allows your program to continue to operate despite potentially being in an error state. Further, when reviewing such code, it's not clear how the argument data is being provided. Accordingly, in PHP 8, this practice is no longer allowed. Both debug_backtrace() or Exception::getTrace() still operate as before. The only difference is that they no longer pass argument variables by reference.

Let's now have a look at changes to PDO error handling.

# PDO error-handling mode default changed

For many years, novice PHP developers were mystified when their database applications using the PDO extension failed to produce results. The reason for this problem, in many cases, was a simple SQL syntax error that was not reported. This was due to the fact that in PHP versions prior to PHP 8, the default PDO error mode was PDO::ERRMODE_SILENT.

SQL errors are not PHP errors. Accordingly, such errors are not captured by normal PHP error handling. Instead, PHP developers had to specifically set the PDO error mode to either PDO::ERRMODE_WARNING or PDO::ERRMODE_EXCEPTION. PHP developers can now breathe a sigh of relief because, as of PHP 8, the PDO default error-handling mode is now PDO::ERRMODE_EXCEPTION.

In the following example, PHP 7 allows the incorrect SQL statement to silently fail:

```
```php
// /repo/ch08/php7_pdo_err_mode.php
$dsn = 'mysql:host=localhost;dbname=php8_tips';
$pdo = new PDO($dsn, 'php8', 'password');
$sql = 'SELEK propertyKey, hotelName FUM hotels '
    ."WARE country = 'CA'";
$stmt = $pdo->query($sql);
if ($stm)
    while($hotel = $stmt->fetch(PDO::FETCH_OBJ))
        echo $hotel->name . ' ' . $hotel->key . "\n";
else
    echo "No Results\n";
```
```