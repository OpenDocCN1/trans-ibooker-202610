4. Finally, we display the results in a table, using the get methods to retrieve property values, as follows:

```
$pattern = '<tr><th>%s</th><td>%s</td></tr>';
echo '<table width="50%" border=1>';
printf ($pattern, 'ID', $test->getId());
printf ($pattern, 'Token', $test->getToken());
printf ($pattern, 'Name', $test->getName());
echo '</table>';
```

Here is how that might appear:

| ID | 111 |
| Token | 999999 |
| Name | Fred |

Table 1.4 – Output using Get methods

The main purpose achieved by marking properties as protected (or private) and by defining getters and setters is to control access. Often, this translates into a desire to prevent the property data type from changing. If this is the case, the entire infrastructure can be replaced by assigning property types.

Simply changing the visibility to public alleviates the need for get and set methods; however, it does not prevent the property data from being changed! Using PHP 8 property types achieves both goals: it eliminates the need for get and set methods and also prevents the data type from being accidentally changed.

Notice here how much less code is needed to achieve the same results in PHP 8 using property typing:

```
// /repo/ch01/php8_prop_reduce.php
declare(strict_types=1);
class Test {
    public int $id = 0;
    public int $token = 0;
    public string $name = '';
}
// assign values
$test = new Test();
$test->id = 111;
$test->token = 999999;
```