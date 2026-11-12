For other expressions, evaluation order is dictated by the construct semantics. For example, evaluation of if e1 then e2 else e3 begins with the evaluation of e1 and continues with evaluation of either e2 or e3 depending on the returned boolean value. Evidently, if the evaluation of expression e1 results in an error, then no subexpression will be evaluated.

# 3.2.5.2. Lexical and dynamic scope

The same program may produce different results depending on the chosen scope mechanism. Consider the following expression:

```
let x2 = 3 in
let f = fun x1 ↦ x1 + x2 in
let x2 = 8 in (f 2)   [3.4]
```

Using lexical scope, this expression is evaluated as follows:

```
[[let x2 = 3 in let f = fun x1 ↦ x1 + x2 in let x2 = 8 in (f 2)]]Env
= [[let f = fun x1 ↦ x1 + x2 in let x2 = 8 in (f 2)]](x2,3)⊕Env
= [[let x2 = 8 in (f 2)]](f,⟨x1,x1+x2,⊕(x2,3)⊕Env)⊕(x2,3)⊕Env
= [[(f 2)]](x2,8)⊕(f,⟨x1,x1+x2,⊕(x2,3)⊕Env)⊕(x2,3)⊕Env
= [[x1 + x2]](x1,2)⊕(x2,3)⊕Env = 2 + 3 = 5
```

Using dynamic scope, the evaluation rules shown in Table 3.3 are used, except those concerning definition and application of functions: in this case, the definition environment of the function is not recorded in the closure. If an application is evaluated in an environment Env_a, the function body is evaluated in the extension of Env_a with the binding of the formal parameter to the value of the argument. In the previous example, this evaluation is done as follows:

```
[[let x2 = 3 in let f = fun x1 ↦ x1 + x2 in let x2 = 8 in (f 2)]]Env
= [[let f = fun x1 ↦ x1 + x2 in let x2 = 8 in (f 2)]](x2,3)⊕Env
= [[let x2 = 8 in (f 2)]](f,⟨x1,x1+x2⟩)⊕(x2,3)⊕Env
= [[(f 2)]](x2,8)⊕(f,⟨x1,x1+x2⟩)⊕(x2,3)⊕Env
= [[x1 + x2]](x1,2)⊕(x2,8)⊕(f,⟨x1,x1+x2⟩)⊕(x2,3)⊕Env = 2 + 8 = 10
```

The dynamic scope mechanism presents two drawbacks. First, if the body of a function contains free variables, its meaning depends on the current value (in Env_a) of the free variables that may change from one call to another. This is particularly problematic when attempting to review source code: the meaning of the function being modified along the execution. Furthermore, a free variable may be bound to a value of a different type in the course of execution, and this may lead to a typing error when evaluating the function application. For example, if x2 in expression [3.4] is associated with a boolean value instead of the integer value 8, then evaluating (f 2) will involve adding 2 to a boolean, resulting in an error. However, during the definition