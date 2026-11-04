```
```php
$usr = 'fake';
$pwd = 'xyz';
$dsn = 'mysql:host=localhost;dbname=' . $db;
$sql = 'SELECT event_name, event_date FROM events';
```
```

6. Next, in the calling program, we call both classes and attempt to execute a query. The connection deliberately fails as we supply the wrong username and password:

```
```php
$ptn = "%2d : %s : %s\n";
try {
    $conn = new ConnectPdo($dsn, $usr, $pwd);
    var_dump($conn->query($sql));
} catch (Throwable $t) {
    printf($ptn, __LINE__, get_class($t),
        $t->getMessage());
}
$conn = new ConnectMysqli($db, $usr, $pwd);
var_dump($conn->query($sql));
```
```

7. As you now know from our discussion above, the output running in PHP 7 shows the PDOException being thrown from the class constructor when the ConnectPdo instance is created. On the other hand, when the ConnectMysqli instance fails, die() is called, with the message Unable to Connect. You also see, on the very last line of the output, the error log information originating from the __destruct() method. Here is that output:

```
```
root@php8_tips_php7 [ /repo/ch05 ]#
php php8_bc_break_destruct.php
15 : PDOException : SQLSTATE[28000] [1045] Access denied for user 'fake'@'localhost' (using password: YES)
PHP Warning: mysqli_connect(): (HY000/1045): Access denied for user 'fake'@'localhost' (using password: YES) in /repo/src/Php7/Connector/ConnectMysqli.php on line 8
Unable to Connect
Php7\Connector\ConnectMysqli:Connection Terminated
```
```