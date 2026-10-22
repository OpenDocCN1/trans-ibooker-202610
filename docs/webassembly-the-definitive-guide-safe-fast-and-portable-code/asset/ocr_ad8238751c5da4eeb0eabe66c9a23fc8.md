Now that we have a working program, let us return to our function shown in Example 5-5. This works because we used a literal syntax to initialize the array. We did not specify how big the array needed to be because the compiler could figure it out. In memory it has allocated enough memory to hold that many integers. This allocation is done on the stack so that when we return from the function there is no additional clean up necessary. We end up with a sufficiently large location in memory to store our numbers to sum up as shown in Figure 5-3.

![](bbox=[0.32, 0.31, 0.7, 0.35])

Figure 5-3. A C array is just a named portion of memory storing our data

What happens if we tell the compiler how big it needs to be but then give it more numbers than that? In Example 5-7 we tell the compiler we are only expecting five integers in the array but then give it ten. This is known as a blivet in some circles⁸. With the following discussion I am hoping to show you how a compiler can help you come to a more correct solution as you make edits to your code by providing feedback on your mistakes. This happens well before we try to run the code which is usually where we find problems in interpreted languages.

# Example 5-7. A broken version of our function.

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