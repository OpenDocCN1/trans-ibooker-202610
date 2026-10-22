```
v2[2] = 99; ⑱

assert( v1 != v2 ); ⑲

auto const v3{ v1 };  ⑳

v3[2] = 99;  // Compilation error!
```

We start with a std::vector called v1, filled with five integers. In the next line, we create a copy of v1, called v2 (⑱). A real copy, sometimes also referred to as a deep copy: the vector v2 now contains its own chunk of memory, its own integers, and it does not refer to the integers in v1.26 We can assert that by comparing the two vectors (they prove to be equal; ⑰), but the addresses of the first elements are different (⑰). And changing one element in v2 (see ⑱), has the effect that the two vectors are not equal anymore (⑲). Yes, both vectors have their own arrays. They do not share their content, i.e. they do not try to “optimize” the copy operation. You might have heard about such techniques, for instance the copy-on-write technique. And yes, you might even be aware that this was a common implementation for std::string prior to C++11. Since C++11, however, std::string is no longer allowed to use copy-on-write, due to its requirements formulated in the C++ standard. The reason is that this “optimization” easily proves to be a pessimization in a multithreaded world. Hence we can count on the fact that copy construction creates a real copy.

Last, but not least, we create another copy called v3, which we declare as const (⑳). If we now try to change a value of v3 we will get a compilation error. This shows, that a const vector does not just prevent adding and removing elements, but that all elements are also considered to be const.

From a semantic perspective, this means that a std::vector, just as any container in the STL, is considered to be a value. Yes, a value, like for instance an int. If we copy a value, we do not just copy a part of the value, but the entire value. If we make a value const, it is not just partially const but completely const. That is the rationale of value semantics.