does not necessarily know (or need to know) how to modify this source code; on the other hand, this source code is not always available, especially when it comes from proprietary and paid software. The execution system (in collaboration with the operating system) must therefore include a mechanism, which allocates memory during program execution in order to satisfy the actual needs of each execution. This mechanism is called *dynamic allocation*. The result of a dynamic allocation is the start address of the area just allocated, which must be stored in a pointer.

A memory allocation may be rejected, for example if there is insufficient space. Usually in this case, the primitive used for memory allocation returns an "illegal" address, often referred to as the NULL pointer (from the name of the constant used to define this address in many languages). This raises two important points. You have to check whether memory allocation has been successful before attempting to access the returned address. In addition, any pointer in a program is likely to contain (for whatever reason) this illegal NULL value. This value must be detected, because accessing this illegal address interrupts the execution of the program.

# 7.6.1.2. Allocation for large structures

Memory allocation for very large data, e.g. of multiple megabytes, can raise certain issues which differ according to the scope of the binding of the data.

Consider the case of a local variable bound to such data. Memory allocation is often done on the execution stack, for example, this is the case in C. As the size of the stack is limited, it can become totally full and overflow, resulting in an execution error. This is illustrated in example 7.14, despite the 16 gigabytes of RAM available in the machine.

# EXAMPLE 7.14.-

```
```c
#include <stdio.h>
#include <stdlib.h>
#define SIZE (10000000L)

int main () {
  int t[SIZE] ;
  t[0] = 5 ;
  printf ( "%d\n", t[0] ) ;
  return 0 ;
}
```
```

It is not possible to allocate 10 million integers to the stack, and so executing this program generates a segmentation error.