Rule F1 expresses the fact that a constant in the language is evaluated as itself. Rules F2, F3, F4, F5 and F6 indicate that the evaluation of the application of a unary or binary operator to one or two expressions is obtained by applying this operator to the result of the evaluation of the subexpressions involved. Of course, if an error is encountered when evaluating one of these expressions, any application of the operator will return the same error. Rules F7, F8, F9 and F10 deal with the evaluation of conditional expressions of the form if e1 then e2 else e3. Depending on the result of the evaluation of e1, either expression e2 or e3 will be evaluated. Once again, if an error is encountered when evaluating e1, evaluation of the conditional expression will result in the same error. Furthermore, if the evaluation of e1 returns a non-boolean value, then a type error (ErrTT) will occur when evaluating the conditional expression.

# 3.3.2. Call-by-value

DEFINITION 3.1.- Call-by-value: the call-by-value mechanism defines a strategy for evaluating function applications, indicating that the argument (i. e. the actual parameter) must always be evaluated. This strategy is extended to the expressions let x = e1 in e2, where e1 is always evaluated.

The rules set out in Box 3.3 define the operational semantics of Exp2. These rules apply a call-by-value mechanism.

(F11) Env |- x ~~> Env(x) (x in dom(Env))   (F12) Env |- x ~~> ErrU (x not in dom(Env))

   (F13)  Env |- e1 ~~> v1   (x, v1) in Env |- e2 ~~> v2
         -----------------------------------------------
         Env |- let x = e1 in e2 ~~> v2   (v1 in V)

         (F14) Env |- e1 ~~> v1
         -----------------------
         Env |- let x = e1 in e2 ~~> v1   (v1 in I)

   (F15) Env |- fun x -> e ~~> <x, e, Env>   (F16) Env |- rec f x = e ~~> <f, x, e, Env>

   Env |- e1 ~~> <x, e, ef, Envf>   Env |- e2 ~~> v2
   (x, v2) + Envf |- e ~~> v
   --------------------------------
   (F17) Env |- (e1 e2) ~~> v   (v2 not in I)

   Env |- e1 ~~> <f, x, ef, Envf>   Env |- e2 ~~> v2
   <f, <f, x, ef, Envf>> + (x, v2) + Envf + ef ~~> v   (v2 not in I)
   ------------------------------------------------
   (F18) Env |- (e1 e2) ~~> v

   Env |- e1 ~~> v1   Env |- e2 ~~> v2   (v1 in I, v2 in I)
   -----------------------------------------
   (F19) Env |- (e1 e2) ~~> v2

         Env |- e1 ~~> v
         ---------------
   (F20) Env |- (e1 e2) ~~> ErrTT   (v not in I)

# Box 3.3. Operational semantics of Exp2: call-by-value