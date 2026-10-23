# Table 7-1. *(continued)*

| No. # | Item to be checked | Description | Yes/No | Notes |
|---|---|---|---|---|
| 5 | **Check if too much memory is allocated.** |  |  |  |
| 6 | **Avoid wrong casts.** |  |  |  |

Allocating too much memory and external parameters represents a certain part of the size. If so, you are dealing with a wrong memory allocation and you will experience a *denial-of-service*. To avoid this from happening, it is better to follow the below criteria:

+   - `malloc(), calloc(), alloca()`
+   - No integer overflows
+   - Avoid arithmetical issues
+   - Verification for any possible operation with untrusted integer that could lead for an integer overflow.

Avoid coding like below. The compiler will think that `malloc` will return an int, which is totally incorrect. It will create a bug that can easily be exploited by hackers.

```
char *a = malloc(10) – bad cast
class BaseClass {};
class DerivedClass: public BaseClass {};

BaseClass b; BaseClass* pb;
DerivedClass d; DerivedClass* pd;

//good cast
pb = dynamic_cast<BaseClass*>(&d);

//bad cast
pd = dynamic_cast<DerivedClass*>(&b);
```