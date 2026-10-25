graph for you.

Chapter 13, “Advanced Use of ‘numpy’,” covers the plotting library. This chapter deals with **numpy** basics.

# ## 12.2 USING THE “ARRAY” PACKAGE

The generic **array** package doesn’t do much, but it conceptually provides a foundation for how **numpy** works. This package supports one-dimensional arrays only. One advantage of this package is that it doesn’t need to be downloaded.

```
import array
```

This package, along with all the other packages introduced in this chapter, deals with arrays in the strict C and C++ sense, instead of lists.

So what are these things called “arrays”?

Like a list, an *array* is an ordered collection in memory in which elements can be referred to by index number. But unlike lists, arrays are assumed to contain fixed-length data. Data is contiguous, meaning that all elements are placed next to each other in memory.

Figure 12.1. helps illustrate the difference. In a Python list, a number of references are involved, although you don’t normally see these. (In C they would be pointers.)

A list object has a reference to the location of the actual list, which can be moved, but each object in this list is a reference to