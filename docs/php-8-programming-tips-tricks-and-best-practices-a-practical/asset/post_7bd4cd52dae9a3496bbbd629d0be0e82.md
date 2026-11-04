
## 2 Learning about PHP 8's Functional Additions

This chapter walks you through important additions and enhancements introduced to **PHP 8** at the procedural level. The code examples used show new PHP 8 features and techniques to facilitate procedural programming.

Mastering the use of the new functions and techniques in this chapter will help you to write faster and cleaner applications. Even though this chapter focuses on commands and functions, all of the techniques are also useful when developing class methods as well.

The following topics are covered in this chapter:

- Working with new PHP 8 operators
- Using arrow functions
- Understanding uniform variable syntax
- Learning new array- and string-handling techniques
- Securing SQLite databases with the authorizer

### Technical requirements

To examine and run the code examples provided in this chapter, the minimum recommended hardware is listed here:

- x86_64-based desktop PC or laptop
- 1 gigabyte (GB) free disk space
- 4 GB of random-access memory (RAM)
- 500 kilobits per second (Kbps) or faster internet connection
- In addition, you need to install the following software:
- Docker
- Docker Compose

Please refer to the *Technical requirements* section of *Chapter 1, Introducing New PHP 8 OOP Features*, for more information on Docker and Docker Compose installation, as well as how to build the Docker container used to demonstrate code explained in this book. Throughout, we refer to the directory in which you restored the sample code for this book as `/repo`.

The source code for this chapter is located here: `https://github.com/PacktPublishing/PHP-8-Programming-Tips-Tricks-and-Best-Practices.`

We can now begin our discussion by examining new PHP 8 operators.

### Working with new PHP 8 operators

A number of new **operators** have been introduced with PHP 8. In addition, PHP 8 generally introduces a uniform and consistent manner in which these operators can be used. In this section, we examine the following operators:

- Variadics operator
- Nullsafe operator
- Concatenate operator
- Ternary operator

Let's start with a discussion of the variadics operator.

### Using the variadics operator

The **variadics** operator consists of three leading dots ( . . . ) preceding a normal PHP variable (or object property). This operator has actually been with the language since PHP 5.6. It's also referred to as the following:

- Splat operator
- Scatter operator
- Spread operator

Before we dive into the improvements PHP 8 has made using this operator, let's have a quick look at how the operator is normally used.

### Unknown number of arguments

One of the most common uses for the variadics operator is in a situation where you define a function with an unknown number of arguments.

In the following code example, the multiVardump() function is able to accept any number of variables. It then concatenates the var_export() output and returns a string:

```php
// /repo/ch02/php7_variadic_params.php
function multiVardump(...$args) {
    $output = '';
    foreach ($args as $var)
        $output .= var_export($var, TRUE);
    return $output;
}
$a = new ArrayIterator(range('A','F'));
$b = function (string $val) { return str_rot13($val); };
$c = [1,2,3];
$d = 'TEST';
echo multiVardump($a, $b, $c);
echo multiVardump($d);
```

The first time the function is called, we provide three arguments. The second time it's called, we only provide a single argument. Because we used a variadics operator, there's no need to rewrite the function to accommodate more or fewer arguments.

> Tip
>
> There is a `func_get_args()` PHP function that gathers all function arguments into an array. The variadics operator is preferred, however, as it must be stated in the function signature, and thus makes the intentions of the program developer much clearer. For more information, see https://php.net/func_get_args.

### Vacuuming up remaining arguments

Another use for the variadics operator is to **vacuum up** any remaining arguments. This technique allows you to mix mandatory parameters with an unknown number of optional parameters.

In this example, a where() function produces a WHERE clause to be added to a **Structured Query Language (SQL)** SELECT statement. The first two arguments are mandatory: it's not reasonable to produce a WHERE clause with no arguments! Have a look at the code here:

```php
// ch02/includes/php7_sql_lib.php
// other functions not shown
function where(stdClass $obj, $a, $b = '', $c = '',
    $d = '') {
    $obj->where[] = $a;
    $obj->where[] = $b;
    $obj->where[] = $c;
    $obj->where[] = $d;
}
```

The calling code using this function might look like this:

```php
// /repo/ch02/php7_variadics_sql.php
require_once __DIR__ . '/includes/php7_sql_lib.php';
$start = '2021-01-01';
$end   = '2021-04-01';
$select = new stdClass();
from($select, 'events');
cols($select, ['id', 'event_key',
    'event_name', 'event_date']);
limit($select, 10);
where($select, 'event_date', '>=', "'$start'");
```

```php
where($select, 'AND');
where($select, 'event_date', '<', "'$end'");
$sql = render($select);
// remaining code not shown
```

You might have noted that where() has to be called multiple times as the number of arguments is limited. This is a perfect candidate for the variadics operator! Here is how the rewritten where() function might look:

```php
// ch02/includes/php8_sql_lib.php
// other functions not shown
function where(stdClass $obj, ...$args) {
    $obj->where = (empty($obj->where))
        ? $args
        : array_merge($obj->where, $args);
}
```

Because ...$args is always returned as an array, to ensure any additional calls to the function do not lose clauses we need to perform an array_merge() operation. Here is the rewritten calling program:

```php
// /repo/ch02/php8_variadics_sql.php
require_once __DIR__ . '/includes/sql_lib2.php';
$start = '2021-01-01';
$end   = '2021-04-01';
$select = new stdClass();
from($select, 'events');
cols($select, ['id', 'event_key',
    'event_name', 'event_date']);
limit($select, 10);
where($select, 'event_date', '>=', "'$start'",
    'AND', 'event_date', '<', "'$end'");
$sql = render($select);
// remaining code not shown
```

The resultant SQL statement is shown here:

```
SELECT id,event_key,event_name,event_date
FROM events
WHERE event_date >= '2021-01-01'
    AND event_date <= '2021-04-01'
LIMIT 10
```

The preceding output shows that our SQL generation logic produces a valid statement.

### Using variadics operator as a replacement

So far, none of this is foreign to an experienced PHP developer. What's different in PHP 8 is that the variadics operator can now be used in situations where widening might come into play.

To properly describe the difference in how the variadics operator can be used, we'll need to briefly return to Object-Oriented Programming (OOP). If we rewrite the where () function described just now into a class method, it might look like this:

```
// src/Php7/Sql/Where.php
namespace Php7\Sql;
class Where {
    public $where = [];
    public function where($a, $b = '', $c = '', $d = '') {
        $this->where[] = $a;
        $this->where[] = $b;
        $this->where[] = $c;
        $this->where[] = $d;
        return $this;
    }
    // other code not shown
}
```

Now, let's say we have a Select class that extends Where but redefines the method signature using a variadics operator. This is how it might look:

```php
// src/Php7/Sql/Select.php
namespace Php7\Sql;
class Select extends Where {
    public function where(...$args) {
        $this->where = (empty($obj->where))
            ? $args
            : array_merge($obj->where, $args);
    }
    // other code not shown
}
```

It's reasonable to use a variadics operator, as the number of arguments provided to formulate a WHERE clause is unknown. Here is the rewritten calling program using OOP:

```php
// /repo/ch02/php7_variadics_problem.php
require_once __DIR__ . '/../src/Server/Autoload/Loader.php';
$loader = new \Server\Autoload\Loader();
use Php7\Sql\Select;
$start = "'2021-01-01'";
$end   = "'2021-04-01'";
$select = new Select();
$select->from($select, 'events')
    ->cols($select, ['id', 'event_key',
        'event_name', 'event_date'])
    ->limit($select, 10)
    ->where($select, 'event_date', '>=', "'$start'",
        'AND', 'event_date', '<=', "'$end'");
$sql = $select->render();
// other code not shown
```

However, when you try to run this example under PHP 7, the following warning appears:

```
Warning: Declaration of Php7\Sql\Select::where(...$args) should be compatible with Php7\Sql\Where::where($a, $b = '', $c = '', $d = '') in /repo/src/Php7/Sql/Select.php on line 5
```
