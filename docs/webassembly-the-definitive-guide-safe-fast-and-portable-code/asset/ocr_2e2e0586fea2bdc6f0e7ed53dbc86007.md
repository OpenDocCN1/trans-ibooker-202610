There are several pieces to this infrastructure but it is largely self-contained and I think ultimately pretty clear. For reasons that are not worth going into at the moment, we are going to use the C++ version of the clang compiler. We do not have time to teach you C++ in this chapter as well so I am not going to focus on too many specifics. There are cases where we need to make the C++ code behave like C, however, so just stick with me on this one.

We will start with some C/C++ code. The two languages are quite closely related, but C++ provides object-oriented programming features that make it a little easier to model a domain using natural concepts (e.g. orders, accounts, users, etc.) We are not going to focus on any of these distinctions, however, which is why I keep referring to the languages together. In Example 5-12 you can see some of the functionality we are going to use. To keep things manageable, I am not going to show you everything at this point.

# Example 5-12. Some C/C++ functions for us to call

```
#include "nanolibC/libc.h"
#include "nanolibC/libc_extra.h"

#define WASM_EXPORT __attribute__((visibility("default"))) extern "C"

WASM_EXPORT int* get_memory_for_int_array(int size) {
    return new int[size];
}

WASM_EXPORT void free_memory_for_int_array(int* arr) {
    delete[] arr;
}

WASM_EXPORT void mergeSort(char *p, int length) {
    int c, d, swap;

    for(c = 0; c < length - 1; c++) {
        for( d = 0; d < length - c - 1; d++) {
            if(p[d] > p[d+1]) {
                swap = p[d];
                p[d] = p[d+1];
                p[d+1] = swap;
            }
        }
    }
```