```
$obj = new SplFileObject($fn, 'r');
$safe = '';
while ($line = $obj->fgets())
    $safe .= strip_tags($line);
echo '<h1>Contents</h1><hr>' . $safe;
```

The output from the modified code is identical to that shown in Figure 5.1. We'll now turn our attention to changes in another SPL class: SplHeap.

# Examining changes to SplHeap

SplHeap is a foundational class used to represent data structured as a binary tree. Two additional classes are also available that build upon SplHeap. SplMinHeap organizes the tree with the minimum value at the top. SplMaxHeap does the reverse, placing the maximum value at the top.

A heap structure is especially useful in situations where data arrives out of order. Once inserted into the heap, the item is automatically placed in its proper order. Thus, at any given moment, you can display the heap safe in the knowledge that all items will be in order without having to run one of the PHP sort functions.

The key to maintaining the automatic sort order is to define an abstract method, compare(). As this method is abstract, SplHeap cannot be instantiated directly. Instead, you need to extend the class and implement compare().

There is the potential for a backward-compatible code break when using SplHeap in PHP 8 as the method signature for compare() must be exactly as follows: SplHeap::compare($value1, $value2).

Let's now have a look at a code example that uses SplHeap to build a list of billionaires organized by last name:

+   1. First, we define a file with data on billionaires. In this example, we simply copied and pasted data from this source: https://www.bloomberg.com/billionaires/.
+   2. We then define a BillionaireTracker class that extracts information from the pasted text into an array of ordered pairs. The full source code (not shown here) for the class can be found in the source code repository here:/repo/src/Services/BillionaireTracker.php.