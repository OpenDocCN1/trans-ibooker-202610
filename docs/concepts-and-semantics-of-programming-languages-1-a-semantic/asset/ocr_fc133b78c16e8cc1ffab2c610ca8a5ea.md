and caught by constructs similar to those presented in section 8.1.4. The option to create exceptions inheriting from other exceptions introduces subtle differences in handlers’ ability to catch raised exceptions. A handler able to catch a given exception will also be able to catch all exceptions inheriting from this exception. Thus, the order of exception matching cases in a handler is significant in defining the case which effectively catches an exception.

# 8.3.1. Exceptions in OCaml

The match/with construct presented earlier is a simplified version of the construct used in OCaml. Instead of just one catching case, any number of cases may be used, as in a pattern matching. Thus, for a handler to catch different exceptions, instead of writing:

EXAMPLE 8.3.--

```
OCaml
try e with
| x -> )
match x with
| Ex1 (v) -> ...
| Ex2 -> ...
)
```

we may write, more directly:

EXAMPLE 8.4.--

```
OCaml
try e with
| Ex1 (v) -> ...
| Ex2 -> ...
```

# 8.3.2. Exceptions in Python

In example 8.5, the class MyExc inherits directly from the Exception base class in Python and introduces an exception.

EXAMPLE 8.5.--

```
Python
class MyExc (Exception) :
def what (self) : return ("MyExc")

class MyDerExc (MyExc) :
def what (self) : return ("MyDerExc")
```