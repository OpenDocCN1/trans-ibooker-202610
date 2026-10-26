```
}

void process()
{
    MyClass objects[12];

    // Do something with objects
    printMyClasses(objects, sizeof(objects)/sizeof(MyClass));
}
```

In the process() function, objects is of the "array of 12 MyClass items" type, but when it is passed to printMyClasses(), it is converted (by the compiler) into the "a pointer to MyClass" type. This is by design (inherited from C) and is known as array decay and can be the cause of errors by new programmers. We could attempt to declare printMyClasses() as follows:

```
void printMyClasses(MyClass objects[12], size_t number)
```

This would still suffer from array decay as the compiler changes the argument object to MyClass*; it does not keep the dimension information in this case. Array decay is the reason that we need to pass number to the printMyClasses() function: so that we know how many items are in the array. C++ provides two mechanisms for dealing with array decay:

+   - Use of iterators to pass a range into the method. STL containers (see the C++ Pre-Packaged Templates section in Chapter 2B, No Ducks Allowed – Templates and Deduction) offer the begin() and end() methods so that we can obtain iterators that allow algorithms to traverse an array or part thereof.

Note
For C++20, the ISO standard committee is considering the inclusion of a concept known as Ranges that will allow both the begin and end iterators to be captured in one object.

+   - Use of templates (see the Non-Type Template Arguments section in Chapter 2B, No Ducks Allowed – Templates and Deduction).