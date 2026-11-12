two new names, a and b, into the namespace for record field names in the environment, while recording the fact that these belong to the type t. When a record expression is analyzed, its fields must all belong to the same type, guiding its verification. Hence, two different record types cannot share the same field names (without significant effort on the part of the programmer). However, if type definitions are carried out in different compilation units or modules, ambiguity can be avoided by specifying the compilation unit containing these definitions. This point is illustrated in example 6.29.

# EXAMPLE 6.29.-

```
```ocaml
# type foo = { a : int ; b : float } ; ;
type foo = { a : int ; b : float ; }
# type bar = { a : int ; b : float } ; ;
type bar = { a : int ; b : float ; }
# let v = { a = 42 ; b = 3.14 } ;;
val v : bar = {a = 42; b = 3.14}
```
```

The expression bound to the value v is inferred as being of type bar, as it was responsible for the most recent binding of the field name a to a record type. Values of the foo type can no longer be created as the corresponding field names are masked. Note, however, that it is still possible to manipulate values of type foo by acting on their fields. Using a form of type annotation with the capacity to override inference, a function get_foo_a can be written to fetch the value of field a from a value of type foo.

# EXAMPLE 6.30.-

```
```ocaml
# let get_foo_a (x : foo) = x.a ;;
val get_foo_a : foo -> int = <fun>
```
```

The second method, used in C, among others, makes use of the type assigned to variables which take record type values. In this case, there is no need to specify field names in record values. Thus, the value { 42, 3.14 } does not have a type per se, except that of a two-field structure in which the first field is an integer and the second is a floating point number. However, at the point of assignment or initialization, the type given to the receiving variable at the time of its definition will be checked for compatibility with the fields of the expression. This means that two different record types may share the same field names, as shown in example 6.31.