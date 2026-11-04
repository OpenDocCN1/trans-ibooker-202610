# Understanding the problems associated with locale dependence

The unfortunate side effect of locale dependence in earlier PHP versions was inconsistent results when typecasting from float to string and then back again. Inconsistencies were also seen when a float value was concatenated to a string. Certain optimizing operations performed by OpCache resulted in the concatenation operation occurring before the locale had been set, yet another way in which inconsistent results might be produced.

In PHP 8, vulnerable operations and functions are now locale-independent. What this means is that all float values are now stored using a period as the decimal separator. The default locale is no longer inherited from the environment by default. If you need the default locale to be set, you must now explicitly call setlocale().

# Reviewing functions and operations affected by locale independence

Most PHP functions are not affected by the switch to locale independence for the simple reason that locale is irrelevant to that function or extension. Furthermore, most PHP functions and extensions are already locale-independent. Examples include the PDO extension, along with functions such as var_export() and json_encode(), and the printf() family.

Functions and operations affected by locale independence include the following:

+   - `(string) $float`
+   - `strval($float)`
+   - `print_r($float)`
+   - `var_dump($float)`
+   - `debug_zval_dump($float)`
+   - `settype($float, "string")`
+   - `implode([$float])`
+   - `xmlrpc_encode($float)`