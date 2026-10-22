+   5 A sub-Uint8Array is created that covers the portion referenced by the previously returned “pointer”.
+   6 The reverse() method from the module is invoked.
+   7 We rely upon JavaScript functionality to shuffle some data.
+   8 The mergeSort() method is invoked with a reference to the “pointer”.
+   9 The memory is freed on the module side.

We start off by serving up the HTML and WebAssembly module over HTTP as we have done previously. The module is loaded and instantiated using the same utility library as before even though it was generated via a completely different process. Once the module instance is available, we assign the variable to another variable defined outside of the lexical scope of this block so we can use it elsewhere.

Because the C/C++ side of our code does not realize what is going on, we have to allocate enough memory from its perspective to store some data in from the JavaScript side. Previously we just wrote data directly into the exported Memory instances. Because we are going to be emulating pointers, we have to create something on that side that will look appropriate. We invoke the get_memory_for_int_array() function and ask it to allocate room for ten integers. The function returns a pointer on the C/C++ side. From this side it is more rightly considered a “pointer”. It is not directly a reference to a location in the heap as you saw earlier. Instead, it is an index into the underlying buffer that the small libc implementation allocated the data into. We will use this reference as an offset into the memory when we pass it back to the other side.

We surround the underlying ArrayBuffer with a Uint8Array wrapper so we can write 8-bit integers easily from this side. If you revisit the code in Example 5-12, you might notice that our sorting and reversing functions