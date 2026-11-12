# EXAMPLE 6.14.-

```
```ocaml
# (+) ; ;
- : int -> int -> int = <fun>
# (.) ; ;
- : float -> float -> float = <fun>
# 3 + 4.5 ; ;
Error: This expression has type float but an expression was expected of type
int
```
```

Any attempt to use a value of the wrong type will result in a typing error.
Developers must explicitly specify the conversion of one of the operands using
functions from the language’s standard library, as shown in example 6.15.

# EXAMPLE 6.15.-

```
```ocaml
# (float_of_int 3) +. 4.5 ; ;
- : float = 7.5
# 3 + (int_of_float 4.5) ; ;
- : int = 7
```
```

This choice prevents any “silent” value conversions: the program does exactly
what the developer specifies. The loss of flexibility resulting from this choice is
counterbalanced by increased ease of understanding and more reliable
maintainability. Other languages carry out implicit conversions between integers and
floating point numbers according to their own specific rules. For example, the C
standard (ISO/IEC 9899:201x) stipulates that if one of the operands of an arithmetic
operator is a floating point number, then the other operand must be converted to a
floating point number, and the result will also be of this type. In cases where the
result is assigned to a variable, the type of the variable has no impact on the type of
the arithmetic computation. Once obtained, the result will be converted to conform to
the type of the variable.

# 6.2. Arrays

An _array_ is a data structure that groups data elements of the same type into a
contiguous sequence of fixed size. Any element in the array can be accessed in a
constant time via an indexing mechanism. Given the location of the first element of
an array in memory (the _base address_) and the size of a single element, an element
with index _i_ can be accessed at the address base + _i_ × size of an element. The indices
of the elements in the array are therefore integer values (or, more generally, in certain
languages, values of a discrete and totally ordered type). As the size of an array is
fixed, attempting to access an element outside the array is a programming error, and
may not immediately, or indeed necessarily, trigger an execution error: the error is