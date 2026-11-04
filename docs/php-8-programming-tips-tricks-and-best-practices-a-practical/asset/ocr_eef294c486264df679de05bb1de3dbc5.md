You'll notice from the PHP 8 output that the filters mentioned earlier have all been removed. Any code that uses any of the three filters listed will break after a PHP 8 migration. We now look at changes made to custom error handling.

# Dealing with custom error-handling changes

Starting with PHP 7.0, most errors are now **thrown**. The exception to this are situations where the PHP engine is unaware that there is an error condition, such as running out of memory, exceeding the time limit, or if a segmentation fault occurs. Another exception is when the program deliberately triggers an error using the `trigger_error()` function.

Using the `trigger_error()` function to trap errors is not a best practice. A *best practice* would be to develop object-oriented code and place it inside a try/catch construct. However, if you are assigned to manage an application that does make use of this practice, there is a change in what gets passed to the custom error handler.

In versions prior to PHP 8, the data passed to the custom error handler's fifth argument, `$errorcontext`, was information about arguments passed to the function. In PHP 8, this argument is ignored. To illustrate the difference, have a look at the simple code example shown next. Here are the steps leading to this:

+   1. First, we define a custom error handler, as follows:

```
// /repo/ch08/php7_error_handler.php
function handler($errno, $errstr, $errfile,
    $errline, $errorcontext = NULL) {
    echo "Number  : $errno\n";
    echo "String  : $errstr\n";
    echo "File    : $errfile\n";
    echo "Line    : $errline\n";
    if (!empty($errorcontext))
        echo "Context: \n"
            . var_export($errorcontext, TRUE);
    exit;
}
```