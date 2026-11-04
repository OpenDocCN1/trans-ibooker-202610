6. We then create a FFI\CData element as an integer array with 16 values populated with random integers, using the rand() function. The code is shown in the following snippet:

```
$max = 16;
$arr_b = FFI::new('int[' . $max . ']');
for ($i = 0; $i < $max; $i++)
    $arr_b[$i]->cdata = rand(0,9999);
```

7. Finally, we display the contents of the array before the sort, perform the sort, and display the contents after. Note in the following code snippet that we execute the sort using a call to bubble_sort() from the FFI instance:

```
echo show('Before Sort', $arr_b, $max);
$bubble->bubble_sort($arr_b, $max);
echo show('After Sort', $arr_b, $max);
```

8. The output, as you might expect, shows an array of random integers before the sort. After the sort, the values are in order. Here is the output from the code shown in Step 7:

```
Before Sort
245,8405,8580,7586,9416,3524,8577,4713,
9591,1248,798,6656,9064,9846,2803,304
After Sort
245,304,798,1248,2803,3524,4713,6656,7586,
8405,8577,8580,9064,9416,9591,9846
```

Now that you have an idea how to integrate an external C library into a PHP application using the FFI extension, we turn to our last topic: PHP callbacks.

# Working with PHP callbacks

As we mentioned at the beginning of this section, it's possible to use the FFI extension to incorporate shared C libraries that are part of the actual PHP language (or its extensions). This integration is important as it allows you to read and write native PHP data in your C library by accessing the C data structures defined in the PHP shared C libraries.