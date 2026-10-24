```
var b = 5.5;

assert(a.round() == 6);
```

An hexadecimal integer is obtained by prefixing it with `0x`:

```
assert(0x7E3 == 2019);
```

# Lists of Bytes and More: the dart:typed_data library

When reading binary data, using a List of 64-bit int s is extremely inefficient if all each of those integers is doing is clearly less than optimally efficient. That’s why the dart:typed_data contains the Uint8List class, which works exactly like the regular List, but with 8-bit integers.

# Type Casting

Type casting in Dart is done in one of the following two ways.

You can use an `as` expression to cast a value to one of its subtypes:

```
num a = 5;

var a_int = a as int;

assert(a_int is int);
```

or use cast to cast all values of List to a specified type:

```
List<num> l = [1, 2, 3];

var l_int = l.cast<int>();

assert(l_int[0] is int);
```