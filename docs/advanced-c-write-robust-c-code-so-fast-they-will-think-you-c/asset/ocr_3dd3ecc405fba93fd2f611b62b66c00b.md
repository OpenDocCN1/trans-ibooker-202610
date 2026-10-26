In the preceding code, when **pvalue** is declared, the compiler allocates memory to store only the address of the memory it will be referring to. Like other variables, you should always ensure that a pointer is initialized before it is used as dereferencing an uninitialized pointer results in an undefined behavior. Exactly how much memory is allocated to store a pointer is dependent upon the system that the compiler is designed for and how many bits the processor supports. But all pointers will be of the same size, regardless of what type they point to.

A pointer can also be passed to a function. This allows the function to access the data being pointed to and possibly modify it. Consider the following implementation of swap:

```
void swap(int* data1, int* data2)
{
    int temp{*data1};      // Initialize temp from value pointed to by
data1
    *data1 = *data2;       // Copy data pointed to by data2 into location
                          // pointed to by data1
    *data2 = temp;         // Store the temporarily cached value from
temp
                          // into the location pointed to by data2
}
```

This shows how to declare pointers as arguments to a function, how to get the value from the pointer by using the dereferencing operator *, as well as how to set a value through the dereference operator.

The following example allocates memory from the host system using the new operator and releases it back to the host system using the delete operator:

```
char* name = new char[20];   // Allocate 20 chars worth of memory and
assign it

                          // to name.

    Do something with name

delete [] name;
```