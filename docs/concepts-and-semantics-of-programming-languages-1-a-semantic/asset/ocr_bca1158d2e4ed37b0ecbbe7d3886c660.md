The evaluation of the expressions in examples 3.2 and 3.3 is obtained in the following way:

```
Python
e25 = Letin2("x",Cste2(CInt2(25)),
       Letin2("f",Fun2("x",B_Op2(Plus2(),Var2("x"),Cste2(CInt2(3)))),
              B_Op2(Plus2(),App2(Var2("f"),Cste2(CInt2(7))),Var2("x"))))
>>> (eval2([],e25)).val.val_int
35
>>> (eval2([],App2(a_fact,Cste2(CInt2(3))))).val.val_int
6
OCaml
let e25 =
    Letin2 ("x", Cste2 (CInt2 25),
            Letin2 ("f", Fun2 ("x", B_Op2 (Plus2, Var2 "x", Cste2 (CInt2 3))),
                    B_Op2 (Plus2, App2 (Var2 "f", Cste2 (CInt2 7)), Var2 "x")))
val e25 : string val_err2
# (eval2 [] e25);;
- : string val_err2 = Val2 (VInt2 35)
# (eval2 [] (App2(a_fact,Cste2(CInt2(3)))));;
- : string val_err2 = Val2 (VInt2 6)
```

# 3.2.5.1. Order of evaluation of subexpressions

The order in which the subexpressions of an expression are evaluated has an effect on the result of the final evaluation. The evaluation function defined in Table 3.3 arbitrarily establishes the order of evaluation of the subexpressions of e1 op e2: we have chosen to evaluate e1 and then e2. If evaluation of e1 triggers an error, then e2 will not be evaluated. The implementation of a mathematically commutative binary operator thus loses this property. For example, if a variable x does not belong to the domain of environment Env, then [[(3 / 0) + x]]Env ≠ [[x + (3 / 0)]]Env. Indeed, we have:

[[(3 / 0) + x]]Env = [[3 / 0]]Env = ErrTE [[x + (3 / 0)]]Env = [[x]]Env = ErrTU

The same goes for some so-called “lazy” boolean operators. For example, when evaluating e1 and e2, if e1 is evaluated as false, the result false may be returned without e2 being evaluated (as the conjunction value will be false in any case); hence, execution errors in e2 will not be detected.

On the other hand, there is no specified evaluation order for e1 and e2 in the evaluation of application (e1 e2). Nevertheless, when the semantics is implemented in a compiler or an interpreter, a choice must be made. For example, in the interpreter eval2 presented earlier, e1 is evaluated before e2. In some “lazy” languages, argument e2 of a function is evaluated only when necessary (if e1 is a constant function, there is no need to evaluate the argument; this means that execution errors in the argument will not be detected).