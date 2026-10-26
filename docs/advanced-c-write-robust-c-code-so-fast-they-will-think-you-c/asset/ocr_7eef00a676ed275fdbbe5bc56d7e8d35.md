When re-compiling our new version, there are no problems with the **Dialog.hpp** and **DialogImpl.cpp** files, but our client **main.cpp** reports the following errors (with the gcc compiler), as you can see below:

![](bbox=[0.07, 0.16, 0.87, 0.4])

The first error reports **invalid application of 'sizeof' to incomplete type 'Dialog::DialogImpl'**. The problem is that in **main.cpp** file the compiler is trying to call the destructor of **Dialog** for us when **main()** function ends. As we discussed in *Chapter 2A, No Ducks Allowed – Types and Deduction* the compiler will generate a destructor for us (as we removed it). This generated destructor will call the destructor of **unique_ptr** which is the cause of the error. If we look at **line 76**, of **unique_ptr.h** file, we'll find the following implementation of the **operator()** function for the default deleter used by **unique_ptr** (the deleter is the function called by **unique_ptr** when it destroys the object it points to):

```
```cpp
void

operator()(_Tp* __ptr) const
{
    static_assert(!is_void<_Tp>::value, "can't delete pointer to incomplete type");
    static_assert(sizeof(_Tp)>0, "can't delete pointer to incomplete type");
    delete __ptr;
}
```
```