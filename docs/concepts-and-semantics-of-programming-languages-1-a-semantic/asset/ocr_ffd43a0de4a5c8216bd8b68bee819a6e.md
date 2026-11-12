# 6.4.4. Sum types

OCaml uses sum types that are more elaborate than simple enumerations of constructors. A sum type is used to group the values of several types into a single type. Take two types t1 and t2. Let us define a type t, in which all values are either the values of type t1, marked by C1, or the values of type t2, marked by C2. The construction of a sum type is similar to the disjoint sum operation on sets (hence the name sum type). C1 defines the injection of v1 of type t1 that results in the value C1(v1) of type t; similarly, v2 of type t2 is injected as C2(v2) in t. C1 and C2 are called the value constructors of type t.

The OCaml code presented in this book has already used a lot of sum types to represent the abstract syntaxes of languages and the different algebras encountered. In example 6.34, playing cards are encoded by a sum type.

EXAMPLE 6.34.–

```
OCaml
type card_t = Jack | Queen | King | Number of int
```

A card may be a picture card, identified by its name alone. Alternatively, it may be a number card, and the number must be known in order to differentiate it from the other cards in the suit. The value constructor Number is applied to values of the int type. The value constructor Jack defines a unique value of the type card_t.

A sum type may be polymorphic. Some of its constructors may be applied to types written using type variables, introduced by the definition of the sum type. The predefined lists in the standard library of OCaml are given by a recursive and polymorphic type definition, similar to that shown in example 6.35.

EXAMPLE 6.35.–

```
OCaml
type 'a list = Nil | Cons of ('a * 'a list)
```

In C/C++, there is no syntactic construct for value constructors. Programmers must encode constructors themselves, using union and struct types. A union type is literally a disjoined sum of the different types mentioned in its definition. A value of the union is represented on a memory range whose size is the maximum of the sizes of the values of these different types. Thus, it differs from a record, where data is aggregated. Each field of the union allows access to the data of the union, which will be interpreted as having the type associated with the field. This point is illustrated in example 6.36.