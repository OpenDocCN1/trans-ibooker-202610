Rules $F_{11}$ and $F_{12}$ are used to obtain the value associated with a variable \(x\) in an environment Env. If this variable does not belong to the domain of Env, the value Err\(_\text{U}\) is returned. Rules $F_{13}$ and $F_{14}$ describe the evaluation of expressions of the form let \(x = e_1\) in \(e_2\) in an environment Env. First, expression \(e_1\) in Env is evaluated, leading to a value noted \(v_1\). Next, \(e_2\) is evaluated in Env, extended with the binding \((x, v_1)\), to obtain the final result. If an error occurs when evaluating expression \(e_1\), then evaluating expression let \(x = e_1\) in \(e_2\) will result in the same error. Rules $F_{15}$ and $F_{16}$ define the value of a function, i.e. a closure containing the current environment (the one in which the definition of the function takes place).

Rules $F_{17}$ and $F_{18}$ describe the evaluation of an application \((e_1 e_2)\). This evaluation will not produce an error if \(e_1\) is evaluated as a closure and if the evaluation of \(e_2\) does not result in an error. Rule \(F_{17}\) applies to the case of an expression \(e_1\), which is evaluated as a non-recursive function, i.e. as a closure of the form \(\langle x, e_f, \text{Env}_f \rangle\). Argument \(e_2\) is evaluated in the current environment (in which the call takes place), Env, giving a value \(v_2\). The body of function \(e_1\), i.e \(e_f\), is evaluated in \(\text{Env}_f\), the definition environment of \(e_1\) extended by the binding of the formal parameter to the value of the actual parameter, \((x, v_2)\).

Rule \(F_{18}\) applies to cases where \(e_1\) is evaluated as a recursive function, i.e. a closure of the form \(\langle f, x, e_f, \text{Env}_f \rangle\). The evaluation of this application is similar to that defined by rule \(F_{17}\), but \(\text{Env}_f\) contains an additional binding \((f, \langle f, x, e_f, \text{Env}_f \rangle)\), used to evaluate applications corresponding to recursive calls to the function in \(e_f\). Note the use of the lexical binding mechanism: the free variables in a function body keep the values they had when the function was defined, no matter what happens later.

# EXAMPLE 3.4.- Again, let us consider the expression defined in [3.3]:

```
let x = 25 in let f = fun x -> x + 3 in (f 7) + x
```

The evaluation tree for this expression is given as:

![](bbox=[0.178, 0.731, 0.887, 0.863])