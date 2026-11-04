```
} catch (PDOException $p) {
    error_log('Database Error: ' . $p->getMessage());
} catch (Throwable $t) {
    error_log('Unknown Error: ' . $t->getMessage());
}
```

2. In this example, if a PDO instance fails due to invalid parameters, the error log would have the entry **Database Error** followed by a message gleamed from the PDOException.

3. On the other hand, if some other general error occurred, the error log would have the entry **Unknown Error** followed by the message coming from some other Exception or Error class.

4. In this example, however, the order of the catch blocks is reversed:

```
try {
    $pdo = new PDO($dsn, $usr, $pwd, $opts);
} catch (Throwable $t) {
    error_log('Unknown Error: ' . $t->getMessage());
} catch (PDOException $p) {
    error_log('Database Error: ' . $p->getMessage());
}
```

5. Due to the way PHP covariance support works, the second catch block would never be invoked. Instead, all error log entries originating from this block of code would have an entry starting with **Unknown Error**.

Let's now have a look at how PHP covariance support applies to object method return data types:

1. First, we define an interface, FactoryIterface, that identifies a method, make(). This method accepts an array as an argument and is expected to return an object of type ArrayObject:

```
interface FactoryInterface {
    public function make(array $arr): ArrayObject;
}
```