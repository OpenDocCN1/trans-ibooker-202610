```
list[d] = list[d+1];
list[d+1] = t;
p++;
}
}
if (p == 0) break;
}
}
```

2. We then compile the C code into object code using the GNU C compiler (included in the Docker image used for this course), as follows:

```
gcc -c -Wall -Werror -fpic bubble.c
```

3. Next, we incorporate the object code into a shared library. This step is necessary as the FFI extension is only able to access shared libraries. We run the following code to do this:

```
gcc -shared -o libbubble.so bubble.o
```

4. We are now ready to define the PHP script that uses our new shared library. We begin by defining a function that shows output from a FFI\CData array, as follows:

```
// /repo/ch04/php8_ffi_using_func_from_lib.php
function show($label, $arr, $max)
{
    $output = $label . "\n";
    for ($x = 0; $x < $max; $x++)
        $output .= $arr[$x] . ',';
    return substr($output, 0, -1) . "\n";
}
```

5. The critical part is next: defining the FFI instance. We use FFI::cdef() to accomplish this and supply two arguments. The first argument is the function signature, and the second argument is a path to our newly created shared library. Both arguments can be seen in the following code snippet:

```
$bubble = FFI::cdef(
    "void bubble_sort(int [], int);",
    "./libbubble.so");
```