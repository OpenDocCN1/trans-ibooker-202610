next group of bits represents the exponent. Finally, the *significand* (also known as a *mantissa*) is coded over the remaining bits. As a first approximation – which is technically erroneous, but sufficient to establish an intuitive understanding – we may consider that the value of a floating point number is *sign × mantissa × 2^exponent*.

As in the case of integers, certain languages, such as C/C++, allow users to specify the size of floating point numbers; others, such as Java, OCaml and Python, have a predefined default size.

![](bbox=[0.25, 0.26, 0.75, 0.31])

**Figure 6.1.** Encoding a floating point number over 32 bits. For a color version of this figure, see www.iste.co.uk/hardin/programming1.zip

Floating point numbers are encoded over a finite number of bits, meaning that the set of these numbers is finite. As floating point numbers are indeed rational, any rational (or real) number falling between two consecutive floating point numbers cannot be represented exactly. In other terms, there are “holes” between two consecutive rational numbers exactly represented using floating point numbers, as shown in example 6.11.

# EXAMPLE 6.11.--

```
```c
#include <stdio.h>

int main () {
    float f11 = 48431.1231 ;
    float f12 = 48431.1239 ;
    float f13 = 48431.1250 ;

    printf ("f11: %f f12: %f f13: %f\n", f11, f12, f13) ;
    printf ("f11 == f12 ? %d\n", (f11 == f12)) ;
    printf ("f12 == f13 ? %d\n", (f12 == f13)) ;
    printf ("f11 == f13 ? %d\n", (f11 == f13)) ;

    f11 = f11 + 0.0001 ;
    printf ("f11 + 0.0001: %f\n", f11) ;

    return (0) ;
}
```
```

```
```text
$ ./a.out
f11: 48431.125000   f12: 48431.125000   f13: 48431.125000
f11 == f12 ? 1
f12 == f13 ? 1
f11 == f13 ? 1
f11 + 0.0001: 48431.125000
```
```