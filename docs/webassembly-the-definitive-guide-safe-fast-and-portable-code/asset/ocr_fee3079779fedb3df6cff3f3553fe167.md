No problem. This is an easy fix. We can simply tell clang to compile the code but not link it with the -c option:

```
brian@tweezer ~/g/w/s/ch05> clang -c simple.c -o simple.o
brian@tweezer ~/g/w/s/ch05> ls -laF simple.*
-rw-r--r--  1 brian  staff     170 Feb 19 15:27 simple.c
-rw-r--r--  1 brian  staff    1060 Feb 19 15:43 simple.o
```

This has generated an object file that has the function definition in it. The nm command⁷ shows us what is in the compiled file:

```
brian@tweezer ~/g/w/s/ch05> nm -a simple.o
                U ___stack_chk_fail
                U ___stack_chk_guard
0000000000000000 T _addArray
                U _memcpy
00000000000000a0 s l___const.addArray.array
```

This might be confusing at first, but the idea should be clear enough with some explanation. Our function, addArray() is defined as a text segment symbol in the object file. The three items with a U symbol type indicate that they are undefined. These particular symbols refer to some buffer overflow protection methods linked in automatically for safety reasons and a function to copy memory from one location to another. The definitions of these functions will be required to make the code executable, but this is what the linking stage and reusable libraries like libc provide.

What we end up with is an incomplete executable, but a properly formed binary representation of our function. If we provide a main() method and link the executable, we can demonstrate how it works. In Example 5-6, our function is called by our driver program.

# Example 5-6. A main() method to invoke our function.

```
#include <stdio.h>

extern int addArray();

int main() {
    int sum = addArray();
```