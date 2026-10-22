
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

## Example 5-6. A main() method to invoke our function.

```
#include <stdio.h>

extern int addArray();

int main() {
    int sum = addArray();
```
```
printf("The array sum is: %d\n", sum);
}
```

Notice we have to tell the compiler about the definition of our addArray() function because it is not defined in this file. The extern keyword provides a promise that there will be something with this name that takes no arguments and returns an integer available. As such, it is ok to assign the result of this function to an integer variable called sum. This is then passed into the printf() function where it is formatted as a human-friendly output message indicating the accumulated sum.

To build the executable we compile the simplemain.c and simple.c files and store the result in an executable called simplemain. Because we did not include the -c option, it does engage the linker. This no longer complains because we did provide a definition for the main() method.

```
brian@tweezer ~/g/w/s/ch05> clang simplemain.c simple.c -o simplemain
brian@tweezer ~/g/w/s/ch05> ls -laF simplemain
-rwxr-xr-x  1 brian  staff  49640 Feb 19 16:01 simplemain*
brian@tweezer ~/g/w/s/ch05> ./simplemain
The array sum is: 45
```

If we use the nm program on the final executable, you will notice that we have provided everything we need to this time. The undefined symbols will be expected to be provided by a dynamic library when the program runs. They are excluded from the binary to keep the size down.

```
brian@tweezer ~/g/w/s/ch05> nm -a simplemain
                 U ___stack_chk_fail
                 U ___stack_chk_guard
0000000100008018 d __dyld_private
0000000100000000 T __mh_execute_header
0000000100003ea0 T _addArray
0000000100003e70 T _main
                 U _memcpy
                 U _printf
                 U dyld_stub_binder
```

Now that we have a working program, let us return to our function shown in Example 5-5. This works because we used a literal syntax to initialize the array. We did not specify how big the array needed to be because the compiler could figure it out. In memory it has allocated enough memory to hold that many integers. This allocation is done on the stack so that when we return from the function there is no additional clean up necessary. We end up with a sufficiently large location in memory to store our numbers to sum up as shown in Figure 5-3.

![](img/16b7990eb0dbf589db177bdbd2f4a189_106_0.png)

Figure 5-3. A C array is just a named portion of memory storing our data

What happens if we tell the compiler how big it needs to be but then give it more numbers than that? In Example 5-7 we tell the compiler we are only expecting five integers in the array but then give it ten. This is known as a blivet in some circles⁸. With the following discussion I am hoping to show you how a compiler can help you come to a more correct solution as you make edits to your code by providing feedback on your mistakes. This happens well before we try to run the code which is usually where we find problems in interpreted languages.

## Example 5-7. A broken version of our function.

```
int addArray() {
    int retValue = 0;
    int array[5] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    for(int i = 0; i < 10; i++) {
        retValue += array[i];
    }

    return retValue;
}
```

Fortunately, this is also something easy for a compiler to detect. It will point out that we are being silly and gives us a warning.

```
brian@tweezer ~/g/w/s/ch05> clang -c simple.c -o simple.o
    int array[5] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
```

1 warning generated.

What happens if we want to return an array from our function? In Example 5-8 we attempt this but quickly see that it will not work.

## Example 5-8. An unsuccessful attempt to return an array from a function.

```
int[] generateArray() {
    int array[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    return array;
}
```

Even though what we are doing seems reasonable, the compiler again informs us that we are not doing it right.

```
brian@tweezer ~/g/w/s/ch05> clang -c simple2.c -o simple2.o
simple2.c:1:22: error: brackets are not allowed here; to declare an array, place the brackets after the identifier
int[] generateArray() {
  ~~
     ^
     []
simple2.c:1:20: error: function cannot return array type 'int []'
int[] generateArray() {
     ^
simple2.c:3:10: warning: incompatible pointer to integer conversion returning 'int [10]' from a function with result type 'int' [-Wint-conversion]
    return array;
           ^~~~~
simple2.c:3:10: warning: address of stack memory associated with local variable 'array' returned [-Wreturn-stack-address]
    return array;
           ^~~~~
2 warnings and 2 errors generated.
```

Array names are special variables in C. They are placeholders for the address of a contiguous block in memory that stores these values. We can introduce a pointer to an integer and assign it to the location of the beginning of the array. In order to access the value at that location, we must use the dereference operator, *.

## Example 5-9. Using a pointer to an array

```
#include <stdio.h>

void generateArray() {
    int array[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    int * a = array;
    printf("a is %p\n", a);
    printf("The first value is: %d\n", *(a));
    printf("The second value is: %d\n", *(a + 1));
    printf("The third value is: %d\n", *(a + 2));
}

int main() {
    generateArray();
}
```

The first value in the array is located at the beginning of the array so we can access it with *a. The second integer is one memory address over so we add one to the base of the array before dereferencing it. The third value is two over.

Compiling our program and running it shows us the output we expect. Your value for a as an address is unlikely to be the same, but it should look similar.

```
brian@tweezer ~/g/w/s/ch05> clang simple3.c -o simple3
brian@tweezer ~/g/w/s/ch05> ./simple3
a is 0x7ffee f3a9720
The first value is: 0
The second value is: 1
The third value is: 2
```

The reason the compiler yelled at us with the code in Example 5-8 is because you cannot return an array like we tried. Instead, you have to return a pointer.

We try once again in Example 5-10 to return our array.
