
> Tip
> For a full explanation of how variance support is applied in PHP 7.4 and PHP 8, have a look here: https://wiki.php.net/rfc/covariant-returns-and-contravariant-parameters.

We'll now have a look at changes to the SPL and how those changes can have an impact on application performance after migrating to PHP 8.

## Handling Standard PHP Library (SPL) changes

The SPL is an extension that contains key classes that implement basic data structures and enhance OOP functionality. It was first introduced in PHP 5 and is now included by default in all PHP installations. Covering the entire SPL is beyond the scope of this book. Instead, in this section, we discuss where significant changes have occurred in the SPL when running PHP 8. In addition, we give you tips and guidance on SPL changes that have the potential to cause your existing applications to stop working.

We start by examining changes to the SplFileObject class.

## Understanding changes to SplFileObject

SplFileObject is an excellent class that incorporates most of the standalone f*() functions, such as fgets(), fread(), fwrite(), and so forth, into a single class. SplFileObject::__construct() method arguments mirror the arguments provided to the fopen() function.

The main difference in PHP 8 is that a relatively obscure method, fgetss(), has been removed from the SplFileObject class. The SplFileObject::fgetss() method, available in PHP 7 and below, mirrors the standalone fgetss() function in that it combines fgets() with strip_tags().

For the sake of illustration, let's assume you have created a website that allows users to upload text files. Before displaying content from the text file, you wish to remove any markup tags. Here is an example that uses the fgetss() method to accomplish this:

- 1. We first define a block of code that acquires the filename:

```
// /repo/ch05/php7_spl_splfileobject.php
$fn = $_GET['fn'] ?? '';
if (!$fn || !file_exists($fn))
    exit('Unable to locate file');
```

- 2. We then create the SplFileObject instance, and read the file line by line using the fgetss() method. At the end, we echo the safe contents:

```
$obj = new SplFileObject($fn, 'r');
$safe = '';
while ($line = $obj->fgetss()) $safe .= $line;
echo '<h1>Contents</h1><hr>' . $safe;
```

- 3. Let's say that the file to be read is this:

```
<h1>This File is Infected</h1>
<script>alert('You Been Hacked');</script>
<img src="http://very.bad.site/hacked.php" />
```

- 4. Here is the output running in PHP 7.1 using this URL:

http://localhost:7777/ch05/php7_spl_splfileobject.php?
fn=includes/you_been_hacked.html

As you can see from the output shown next, all HTML markup tags have been removed:

![](img/dbf2674d5b31107c344104b5edcb21c5_206_0.png)

Figure 5.1 – Result after reading a file using SplFileObject::fgetss()

To accomplish the same thing in PHP 8, the code shown previously would need to be modified by replacing fgetss() with fgets(). We would also need to use strip_tags() on the line concatenated to $safe. Here is how the modified code might appear:

```
// /repo/ch05/php8_spl_splfileobject.php
$fn = $_GET['fn'] ?? '';
if (!$fn || !file_exists($fn))
    exit('Unable to locate file');
```

```
$obj = new SplFileObject($fn, 'r');
$safe = '';
while ($line = $obj->fgets())
    $safe .= strip_tags($line);
echo '<h1>Contents</h1><hr>' . $safe;
```

The output from the modified code is identical to that shown in Figure 5.1. We'll now turn our attention to changes in another SPL class: SplHeap.

## Examining changes to SplHeap

SplHeap is a foundational class used to represent data structured as a binary tree. Two additional classes are also available that build upon SplHeap. SplMinHeap organizes the tree with the minimum value at the top. SplMaxHeap does the reverse, placing the maximum value at the top.

A heap structure is especially useful in situations where data arrives out of order. Once inserted into the heap, the item is automatically placed in its proper order. Thus, at any given moment, you can display the heap safe in the knowledge that all items will be in order without having to run one of the PHP sort functions.

The key to maintaining the automatic sort order is to define an abstract method, compare(). As this method is abstract, SplHeap cannot be instantiated directly. Instead, you need to extend the class and implement compare().

There is the potential for a backward-compatible code break when using SplHeap in PHP 8 as the method signature for compare() must be exactly as follows: SplHeap::compare($value1, $value2).

Let's now have a look at a code example that uses SplHeap to build a list of billionaires organized by last name:

- 1. First, we define a file with data on billionaires. In this example, we simply copied and pasted data from this source: https://www.bloomberg.com/billionaires/.
- 2. We then define a BillionaireTracker class that extracts information from the pasted text into an array of ordered pairs. The full source code (not shown here) for the class can be found in the source code repository here:/repo/src/Services/BillionaireTracker.php.

Here is how the data produced by the class appears:

```
array(20) {
    [0] =>  array(1) {
        [177000000000] =>  string(10) "Bezos,Jeff"
    }
    [1] =>  array(1) {
        [157000000000] =>  string(9) "Musk,Elon"
    }
    [2] =>  array(1) {
        [136000000000] =>  string(10) "Gates,Bill"
    }
    ... remaining data not shown
```

As you can see, the data is presented in descending order where the key represents net worth. In contrast, in our sample program, we plan to produce data in ascending order by last name.

- 3. We then define a constant that identifies the billionaire data source file, and set up an autoloader:

```
// /repo/ch05/php7_spl_splheap.php
define('SRC_FILE', __DIR__
    . '/../sample_data/billionaires.txt');
require_once __DIR__
    . '/../src/Server/Autoload/Loader.php';
$loader = new \Server\Autoload\Loader();
```

- 4. Next, we create an instance of the BillionaireTracker class and assign the results to $list:

```
use Services\BillionaireTracker;
$tracker = new BillionaireTracker();
$list = $tracker->extract(SRC_FILE);
```

- 5. Now comes the part of most interest: creating the heap. To accomplish this, we define an anonymous class that extends SplHeap. We then define a compare() method that performs the necessary logic to place inserted elements in their proper place. PHP 7 allows you to change the method signature. In this example, we provide arguments in the form of an array:

```
$heap = new class () extends SplHeap {
    public function compare(
        array $arr1, array $arr2) : int {
        $cmp1 = array_values($arr2)[0];
        $cmp2 = array_values($arr1)[0];
        return $cmp1 <=> $cmp2;
    }
};
```

You might also note that the value for $cmp1 is assigned from the second array, and the value for $cmp2 is from the first array. The reason for this switch is because we wish to produce results in ascending order.

- 6. We then use SplHeap::insert() to add elements to the heap:

```
foreach ($list as $item)
    $heap->insert($item);
```

- 7. Finally, we define a BillionaireTracker::view() method (not shown) to run through the heap and display results:

```
$patt = "%20s\t%32s\n";
$line = str_repeat('-', 56) . "\n";
echo $tracker->view($heap, $patt, $line);
```

- 8. Here is the output produced by our little program running in PHP 7.1:

```
root@php8_tips_php7 [ /repo/ch05 ]#
php php7_spl_splheap.php
--------------------------------------------------------
                Net Worth                              Name
--------------------------------------------------------
        84,000,000,000                        Ambani,Mukesh
       115,000,000,000                        Arnault,Bernard
        83,600,000,000                        Ballmer,Steve
```

```
... some lines were omitted to save space ...
58,200,000,000                     Walton,Rob
100,000,000,000                    Zuckerberg,Mark
--------------------------------------------------
                                   1,795,100,000,000
--------------------------------------------------
```

You will note, however, that when we attempt to run the same program in PHP 8, an error is thrown. Here is the output of the same program running in PHP 8:

```
root@php8_tips_php8 [ /repo/ch05 ]# php php7_spl_splheap.php
PHP Fatal error: Declaration of SplHeap@anonymous::compare(array $arr1, array $arr2): int must be compatible with SplHeap::compare(mixed $value1, mixed $value2) in /repo/ch05/php7_spl_splheap.php on line 16
```

Accordingly, to get this working properly, we must redefine the anonymous class that extends SplHeap. Here is a modified version of that portion of the code:

```
$heap = new class () extends SplHeap {
    public function compare($arr1, $arr2) : int {
        $cmp1 = array_values($arr2)[0];
        $cmp2 = array_values($arr1)[0];
        return $cmp1 <=> $cmp2;
    }
};
```

The only change is in the compare() method signature. When executed, the results (not shown) are identical. The full code for PHP 8 can be viewed at /repo/ch05/php8_spl_splheap.php.

This concludes our discussion of changes to the SplHeap class. Please note that the same change also applies to SplMinHeap and SplMaxHeap. Let's now have a look at a potentially significant change in the SplDoublyLinkedList class.

## Handling changes in SplDoublyLinkedList

The SplDoublyLinkedList class is an iterator that's able to display information in either FIFO (First-In, First-Out) or LIFO (Last-In, First-Out) order. It's more common, however, to say that you can iterate through the list in either forward or reverse order.
