about WebAssembly versions of libraries. They provide good performance gains but are completely portable and simplify their deployment model.

# WebAssembly and Node.js

While most people think of WebAssembly as a client-side browser technology, Node.js has had support for invoking functions from WebAssembly modules almost as long as the browsers have. I am not going to create any actual “servers” as I think that may distract from the points I want to make, but you clearly could set up a REST API or something and still use the features I am mentioning.

Let’s start with a simple example in Example 8-4.

Example 8-4. A simple program to add two numbers

```
#include <stdio.h>

int add(int x, int y) {
    return x + y;
}

int main() {
    printf("The sum of 2 and 3 is: %d\n", add(2,3));
    return 0;
}
```

With clang, this is trivially compiled and executed.

```
brian@tweezer ~/g/w/s/c/node> clang add.c
brian@tweezer ~/g/w/s/c/node> ./a.out
The sum of 2 and 3 is: 5
```

If we update the source code to include some Emscripten-related macros, we can easily run it in Node.js as we have seen previously. I also removed the main() method so our module will no longer expect an implementation of the printf() function since we will be operating in the world of server side JavaScript. The updated code is in Example 8-5.