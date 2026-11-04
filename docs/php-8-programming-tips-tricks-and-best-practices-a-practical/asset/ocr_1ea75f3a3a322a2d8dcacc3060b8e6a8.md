Here is the PHP 7 output:

```
PHP Warning:  Creating default object from empty value in /repo/ch03/php8_warn_prop_nobj.php on line 4
class stdClass#1 (1) {
    public $test =>
        int(1)
}
```

As you can see, in PHP 7 a stdClass() instance is silently created and a Warning is issued, but the operation is allowed to continue. If we run the same code under PHP 8, notice here the difference in output:

```
PHP Fatal error:  Uncaught Error: Attempt to assign property "test" on null in /repo/ch03/php8_warn_prop_nobj.php:4
```

The good news is in PHP 8 the Error is thrown, which means we could easily catch it by implementing a try() / catch() block. As an example, here's how the code shown previously might be rewritten:

```
try {
    $a->test = 0;
    $a->test++;
    var_dump($a);
} catch (Error $e) {
    error_log(__FILE__ . ':' . $e->getMessage());
}
```

As you can see, any problems with the three lines are now wrapped safely inside a try() / catch() block, meaning that recovery is possible. We now turn our attention to array error-handling enhancements.

# Promoted warnings in array handling

A number of bad practices regarding arrays, allowed in PHP 7 and earlier versions, now throw an Error. As discussed in the previous subsection, PHP 8 array error-handling changes serve to give you a more forceful response to the error situations we describe here. The ultimate goal of these enhancements is to nudge developers toward good coding practices.