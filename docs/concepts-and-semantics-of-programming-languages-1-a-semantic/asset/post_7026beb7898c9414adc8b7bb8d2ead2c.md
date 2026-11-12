
Rules $F_{11}$ and $F_{12}$ are used to obtain the value associated with a variable \(x\) in an environment Env. If this variable does not belong to the domain of Env, the value Err\(_\text{U}\) is returned. Rules $F_{13}$ and $F_{14}$ describe the evaluation of expressions of the form let \(x = e_1\) in \(e_2\) in an environment Env. First, expression \(e_1\) in Env is evaluated, leading to a value noted \(v_1\). Next, \(e_2\) is evaluated in Env, extended with the binding \((x, v_1)\), to obtain the final result. If an error occurs when evaluating expression \(e_1\), then evaluating expression let \(x = e_1\) in \(e_2\) will result in the same error. Rules $F_{15}$ and $F_{16}$ define the value of a function, i.e. a closure containing the current environment (the one in which the definition of the function takes place).

Rules $F_{17}$ and $F_{18}$ describe the evaluation of an application \((e_1 e_2)\). This evaluation will not produce an error if \(e_1\) is evaluated as a closure and if the evaluation of \(e_2\) does not result in an error. Rule \(F_{17}\) applies to the case of an expression \(e_1\), which is evaluated as a non-recursive function, i.e. as a closure of the form \(\langle x, e_f, \text{Env}_f \rangle\). Argument \(e_2\) is evaluated in the current environment (in which the call takes place), Env, giving a value \(v_2\). The body of function \(e_1\), i.e \(e_f\), is evaluated in \(\text{Env}_f\), the definition environment of \(e_1\) extended by the binding of the formal parameter to the value of the actual parameter, \((x, v_2)\).

Rule \(F_{18}\) applies to cases where \(e_1\) is evaluated as a recursive function, i.e. a closure of the form \(\langle f, x, e_f, \text{Env}_f \rangle\). The evaluation of this application is similar to that defined by rule \(F_{17}\), but \(\text{Env}_f\) contains an additional binding \((f, \langle f, x, e_f, \text{Env}_f \rangle)\), used to evaluate applications corresponding to recursive calls to the function in \(e_f\). Note the use of the lexical binding mechanism: the free variables in a function body keep the values they had when the function was defined, no matter what happens later.

### EXAMPLE 3.4.

Again, let us consider the expression defined in [3.3]:

```
let x = 25 in let f = fun x -> x + 3 in (f 7) + x
```

The evaluation tree for this expression is given as:

![](img/c9a176746efdbd36ba74d71dfdc86e86_72_0.png)

where \(\nabla_1\) is the evaluation tree:

```
\((F_{11})\overline{\text{Env}_2 \vdash f \rightsquigarrow F_f} \quad (F_1)\overline{\text{Env}_2 \vdash 7 \rightsquigarrow 7} \quad \nabla_2\)
\((F_{17})\overline{\text{Env}_2 \vdash (f \ 7) \rightsquigarrow 10} \quad (F_{11})\overline{\text{Env}_2 \vdash x \rightsquigarrow 25}\)
\((F_4)\overline{(f, F_f) \oplus \text{Env}_1 \vdash (f \ 7) + x \rightsquigarrow 35}\)
```

where \(\nabla_2\) is the evaluation tree:

```
\((F_{11})\overline{(x, 7) \oplus \text{Env}_1 \vdash x \rightsquigarrow 7} \quad (F_{11})\overline{(x, 7) \oplus \text{Env}_1 \vdash 3 \rightsquigarrow 3}\)
\((F_4)\overline{(x, 7) \oplus \text{Env}_1 \vdash x + 3 \rightsquigarrow 10}\)
```

### EXAMPLE 3.5.

The evaluation of \((e_{fact} \ 3)\) where \(e_{fact}\) is the expression defined in [3.2]:

```
(rec fact n = if n <= 1 then 1 else (fact (n - 1)) * n)
```

is done as follows:

```
\((F_{16})\overline{\text{Env}_0 \vdash e_{fact} \rightsquigarrow \langle fact, n, e_1, \text{Env}_0 \rangle} \quad (F_1)\overline{\text{Env}_0 \vdash 3 \rightsquigarrow 3} \quad \nabla_1\)
\((F_{18})\overline{F_{fact}}\)
\(\text{Env}_0 \vdash (e_{fact} \ 3) \rightsquigarrow 6\)
```

where \(\nabla_1\) is the evaluation tree:

```
\((F_4)\overline{\text{Env}_1 \vdash n \rightsquigarrow 3} \quad (F_1)\overline{\text{Env}_1 \vdash 1 \rightsquigarrow 1} \quad \nabla_2 \quad (F_{11})\overline{\text{Env}_1 \vdash n \rightsquigarrow 3}\)
\((F_4)\overline{\text{Env}_1 \vdash n \le 1 \rightsquigarrow false} \quad (F_4)\overline{\text{Env}_1 \vdash e_2 \rightsquigarrow 6}\)
\((F_8)\overline{\langle fact, F_{fact} \rangle \oplus (n, 3) \oplus \text{Env}_0 \vdash e_1 \rightsquigarrow 6}\)
```

where \(\nabla_2\) is the evaluation tree:

```
\((F_{11})\overline{\text{Env}_1 \vdash fact \rightsquigarrow F_{fact}} \quad (F_4)\overline{\text{Env}_1 \vdash n \rightsquigarrow 3} \quad (F_1)\overline{\text{Env}_1 \vdash 1 \rightsquigarrow 1} \quad \nabla_3\)
\((F_{11})\overline{\text{Env}_1 \vdash fact \rightsquigarrow F_{fact}}\)
\(\text{Env}_1 \vdash (fact (n - 1)) \rightsquigarrow 2\)
```

where \(\nabla_3\) is the evaluation tree:

![](img/c9a176746efdbd36ba74d71dfdc86e86_74_0.png)

where \(\nabla_4\) is the evaluation tree:

![](img/c9a176746efdbd36ba74d71dfdc86e86_74_1.png)

where \(\nabla_5\) is the evaluation tree:

![](img/c9a176746efdbd36ba74d71dfdc86e86_74_2.png)

### REMARK.

Termination of the evaluation mechanism: using the syntactic construct rec f x = e, expressions may be defined for which evaluation does not terminate. For example, from expression e_foo defined by

```
rec f x = (f x) [3.5]
```

we can construct the expression:

```
let foo = (rec f x = (f x)) in (foo 2) [3.6]
```

The evaluation of this expression does not terminate. The construction of the evaluation tree for this expression is never-ending:

![](img/c9a176746efdbd36ba74d71dfdc86e86_74_3.png)

![](img/c9a176746efdbd36ba74d71dfdc86e86_74_4.png)

where the construction of \(\nabla_1\) is given as:

![](img/c9a176746efdbd36ba74d71dfdc86e86_74_5.png)

where the construction of \(\nabla_2\) is given as:

(F_{11}) \frac{\text{Env}_2 \vdash f \rightsquigarrow F_{foo}}{} \quad (F_{11}) \frac{\text{Env}_2 \vdash x \rightsquigarrow 2}{} \quad (F_{18}) \frac{\text{Env}_2 \vdash (fx) \rightsquigarrow}{} \quad \vdots \\
(F_{18}) \frac{((f, F_{foo}) \oplus (x, 2) \oplus \text{Env} \vdash (fx) \rightsquigarrow)}{\text{Env}_2}

This semantics can only be used to describe terminating executions. In other terms, Env \(\vdash e \rightsquigarrow v\) expresses the fact that \(v\) is the value of \(e\) obtained by a finite evaluation tree, constructed using the inference rules presented in Box 3.2 and Box 3.3. There is no such evaluation tree for expression [3.6]. This semantics is called *big-step operational semantics*: Env \(\vdash e \rightsquigarrow v\) does not describe the computation, only the result. To describe and reason using computations, including non-terminating computations, the semantics may be formalized as *small-step semantics* (or reduction semantics), a subject discussed at length in Chapter 4.

## 3.3.3. Recursive and mutually recursive functions

Two occurrences of \(e_f\) occur on either side of the symbol \(\vdash\) in rule \(F_{18}\), which concerns the application of a recursive function. At first glance, this may seem paradoxical: evaluating \(e_f\) on the right leads us to evaluate \(e_f\) on the left. As we saw in section 3.2.5, this question may be expressed directly by using rule \(F_{16}\) and binding \(f\) to the closure:

\(F = \langle x, e, (f, F) \oplus \text{Env} \rangle\)

Once again, \(F\) appears on both sides of the equals sign. The existence of value \(F\) can be proven using the mathematical theory of the least fixpoint (LFP), briefly presented in section 4.7.1; a more detailed description can be found in [JAU 16]. Taking this approach, rules \(F_{16}\), \(F_{17}\) and \(F_{18}\) can be condensed into two rules:

(N_{16}) \text{Env} \vdash \text{rec } f \ x = e \rightsquigarrow \langle x, e, (f, F) \oplus \text{Env} \rangle \stackrel{\text{Env}_f}{=} F

(N_{17\_18}) \frac{\text{Env} \vdash e_1 \rightsquigarrow \langle x, e, \text{Env}_f \rangle \quad \text{Env} \vdash e_2 \rightsquigarrow v_2}{(x, v_2) \oplus \text{Env}_f \vdash e \rightsquigarrow v} \text{Env} \vdash (e_1 e_2) \rightsquigarrow v

Applications of recursive and non-recursive functions are based on the same mechanism. The LFP theory allows us to be sure that the evaluation of any expression with a finite number of recursive calls will terminate. Furthermore, the same theory can be used to prove that the two approaches define the same semantics for \(\text{Exp}_2\).
