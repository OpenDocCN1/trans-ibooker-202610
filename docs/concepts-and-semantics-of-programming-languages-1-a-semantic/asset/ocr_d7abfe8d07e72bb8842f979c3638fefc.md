Imagine that we inadvertently define both red = 36 and green = 36. No error will be raised, despite the fact that these two traffic light values, which should be disjunct, are now equal. An expression used to test a traffic light value will no longer differentiate between a red light and a green light.

We explicitly chose the integer values to assign to the light values. There is no real reason for this choice, given that the values used to encode the colors have no influence on the algorithms acting upon them. Certain languages include a construct that allow users to perform tests enumerating all possible cases of a type’s values and verifying exhaustiveness (no values are left out). In a case where light values are encoded by integers, we would need to write, at best, a general case that should never occur, or, at worst, all other possible cases that should never occur. The capacity to define a new type with two-by-two disjunct values, only compatible with other values of this type, eliminates these drawbacks. An enumerated type definition of this kind enumerates the names given to values, known as _value constructors_.

Languages such as Java, C and C++ allow these definitions to be created by means of the enum type construct. Once value constructors have been introduced, they can be used in the same ways as any other value. This is illustrated in example 6.33.

# EXAMPLE 6.33.-

```
enum signal_t { Green, Yellow, Red } ;

enum signal_t next (enum signal_t state) {
if (state == Green) return Yellow ;
if (state == Orange) return Red ;
return Green ;
}
```

The language compiler is in charge of encoding these values by integers internally. Value constructors may be more or less strongly typed depending on the language. In C, they remain integers in their own right: it is always possible to call the next function with any given integer. In C++ and Java, constructors are only values of the type which declaration introduced them and are not compatible with integers. For example, calling next using 0 as an argument in C++ results in a type error:

```
foo.cpp:14:3: error: no matching function for call to ’next’
next (0) ;
^~~
bar.cpp:4:15: note: candidate function not viable: no known conversion from
’int’ to ’enum signal_t’ for 1st argument
enum signal_t next (enum signal_t state)
^
1 error generated.
```