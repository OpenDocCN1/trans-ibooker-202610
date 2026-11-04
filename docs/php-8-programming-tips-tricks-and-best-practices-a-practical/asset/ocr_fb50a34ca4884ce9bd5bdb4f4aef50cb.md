The following operators are affected: <==>, ==, !=, >, >=, <, and <=. The following functions are affected if the option flags are set to default:

+   - `in_array()`
+   - `array_search()`
+   - `array_keys()`
+   - `sort()`
+   - `rsort()`
+   - `asort()`
+   - `arsort()`
+   - `array_multisort()`

> **Tip**
> 
> For more information on improved numeric string handling in PHP 8, refer to the following link: https://wiki.php.net/rfc/saner-numeric-strings. A related PHP 8 change is documented here: https://wiki.php.net/rfc/string_to_number_comparison.

The best practice is to minimize PHP type juggling by providing type hints for functions or methods. You can also force the data type before the comparison. Finally, consider making use of strict comparisons, although this might not be suitable in all situations.

Now that you have an understanding of how to properly handle comparisons involving numeric strings in PHP 8, let's now have a look at PHP 8 changes involving arithmetic, bitwise, and concatenation operations.

# Handling differences in arithmetic, bitwise, and concatenation operations

Arithmetic, bitwise, and concatenation operations are at the heart of any PHP application. In this section, you learn about hidden dangers that might arise in these simple operations following a PHP 8 migration. You must learn about the changes made in PHP 8 so that you can avoid a potential code break in your application. Because these operations are so ordinary, without this knowledge, you will be hard pressed to discover post-migration errors.