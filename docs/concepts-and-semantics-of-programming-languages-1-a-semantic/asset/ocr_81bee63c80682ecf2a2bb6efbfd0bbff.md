Its AST is:

![](bbox=[0.17, 0.12, 0.81, 0.23])

All operators introduced in the programs, written in Python and OCaml, end with 2 in reference to the name **Exp2**. The set of constants **K** contains the relative integers and booleans, and can be represented as follows:

```
```python
class CInt2:
    def __init__(self,cst_int):
        self.cst_int = cst_int

class CBool2:
    def __init__(self,cst_bool):
        self.cst_bool = cst_bool
```
```ocaml
type const2 = CInt2 of int | CBool2 of bool
```
```

Let us define the operators used in expressions of **Exp2**:

```
```python
class Not2:
    pass
class Plus2:
    pass
class Minus2:
    pass
class Mult2:
    pass
class Div2:
    pass
class And2:
    pass
class Or2:
    pass
class Eq2:
    pass
class Leq2:
    pass
```
```ocaml
type unary_op2 = Not2
type bin_op2 = Plus2 | Minus2 | Mult2 | Div2 | And2 | Or2 | Eq2 | Leq2
```
```

Expressions of **Exp2** can now be defined as follows:

```
```python
class Cste2:
    def __init__(self,cste):
        self.cste = cste
class Var2:
    def __init__(self,symb):
        self.symb = symb
class U_Op2:
    def __init__(self,op,exp1):
        self.op = op
        self.exp1 = exp1
class Letin2:
    def __init__(self,symb,exp1,exp2):
        self.symb = symb
        self.exp1 = exp1
        self.exp2 = exp2
class Fun2:
    def __init__(self,param,corps):
        self.param = param
        self.corps = corps
```
```