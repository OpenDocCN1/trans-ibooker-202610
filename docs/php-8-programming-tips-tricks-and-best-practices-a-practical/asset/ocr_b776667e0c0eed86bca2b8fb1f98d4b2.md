The following example demonstrates create_function() usage. The objective of this example is to scan a web-server access log and sort the results by Internet Protocol (IP) address:

1. We start by recording the start time in microseconds. Later, we use this value to determine performance. Here's the code you'll need:

```
// /repo/ch08/php7_create_function.php
$start = microtime(TRUE);
```

2. Next, use create_function() to define a callback that reorganizes the IP address at the start of each line into uniform segments of exactly three digits each. We need to do this in order to perform a proper sort (defined later). The first argument to create_function() is a string the represents the parameters. The second argument is the actual code to be executed. The code is illustrated in the following snippet:

```
$normalize = create_function(
    '&$line, $key',
    '$split = strpos($line, " ");'
    . '$ip = trim(substr($line, 0, $split));'
    . '$remainder = substr($line, $split);'
    . '$tmp = explode(".", $ip);'
    . 'if (count($tmp) === 4)'
    . '    $ip = vsprintf("%03d.%03d.%03d.%03d", $tmp);'
    . '$line = $ip . $remainder;'
);
```

Note the extensive use of strings. This awkward syntax can easily lead to syntax or logic errors, as most code editors make no effort to interpret commands embedded in a string.

3. Next, we define a sorting callback to be used with usort(), as follows:

```
$sort_by_ip = create_function(
    '$line1, $line2',
    'return $line1 <=> $line2;' );
```