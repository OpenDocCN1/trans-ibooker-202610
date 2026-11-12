# EXAMPLE 7.10.-

```
```cpp
#include <stdio.h>

void f (int& v) { v = v + 1 ; }

int main () {
int i = 46 ;
f (i) ;
printf("i = %d\n", i) ;
return 0 ;
}
```
```

The formal parameter v of f is a reference to an integer. Thus, during the call f(i), v becomes an alias of i; in other terms, i and v denote the same location in the memory, although the two identifiers are not in the same scope. Thus, the incrementation of v in f is visible in i in the caller function, and the program returns i = 47.

Given that a reference is an alias, it is entirely possible to create a reference to a reference, or to pass a reference to a function accepting a reference as an argument; the reference always refers to the same initial variable. This is illustrated in example 7.11.

# EXAMPLE 7.11.-

```
```cpp
#include <stdio.h>

void f (int& v) { v = v + 1 ; }

int main () {
int i = 46 ;
int &j = i ;
int &k = j ;
f (i) ;
f (j) ;
f (k) ;
printf("i = %d j = %d k = %d\n", i, j, k) ;
return (0) ;
}
```
```

Creating j as a reference to i, and k as a reference to j, identifiers j and k are both aliases of i. In the context of calls to f with each of these identifiers as arguments, the formal parameter v also becomes an alias of i. Three incrementations are therefore applied, and the program returns i = 49 j = 49 k = 49.

The same applies in the context of a function call: the argument passed during the call for a parameter of reference type must designate a memory area. Once again, the purpose of this mechanism is to create aliases for identifiers. Thus, example 7.12 will be rejected by the compiler.