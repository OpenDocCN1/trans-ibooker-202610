```
if isinstance(op,Leq2):
    if isinstance(v1,VInt2) and isinstance(v2,VInt2):
        return Val2(VBool2(v1.val.int <= v2.val.int))
    return Err2(E_Typ2)
raise ValueError
```

```
OCaml
let apply2_2_unary_op op v = match op with
| Not2 -> (match v with
| VBool2 b -> Val2 (VBool2 (not b))
| _ -> Err2 E_Typ2)
val apply2_unary_op : unary_op -> 'a valeur2 -> 'b val_err2

let apply2_binary_op op v1 v2 = match op with
| Plus2 -> (match (v1, v2) with
| (VInt2 n1, VInt2 n2) -> Val2 (VInt2 (n1 + n2))
| _ -> Err2 E_Typ2)
| Minus2 -> (match (v1, v2) with
| (VInt2 n1, VInt2 n2) -> Val2 (VInt2 (n1 - n2))
| _ -> Err2 E_Typ2)
| Mult2 -> (match (v1, v2) with
| (VInt2 n1, VInt2 n2) -> Val2 (VInt2 (n1 * n2))
| _ -> Err2 E_Typ2)
| Div2 -> (match (v1, v2) with
| (VInt2 n1, VInt2 n2) ->
    if n2 = 0 then Err2 E_Exec2 else Val2 (VInt2 (n1 / n2))
| _ -> Err2 E_Typ2)
| And2 -> (match (v1, v2) with
| (VBool2 b1, VBool2 b2) -> Val2 (VBool2 (b1 && b2))
| _ -> Err2 E_Typ2)
| Or2 -> (match (v1, v2) with
| (VBool2 b1, VBool2 b2) -> Val2 (VBool2(b1 || b2))
| _ -> Err2 E_Typ2)
| Eq2 -> (match (v1, v2) with
| (VInt2 n1, VInt2 n2) -> Val2 (VBool2 (n1 = n2))
| (VBool2 b1, VBool2 b2) -> Val2 (VBool2 (b1 = b2))
| _ -> Err2 E_Typ2)
| Leq2 -> (match (v1, v2) with
| (VInt2 n1, VInt2 n2) -> Val2 (VBool2 (n1 <= n2))
| _ -> Err2 E_Typ2)
val apply2_binary_op : bin_op -> 'a valeur2 -> 'b valeur2 -> 'c val_err2
```

# 3.2.4. Closures

The language Exp2 allows us to define functions and apply them to actual parameters, also called arguments. Let f be a function with formal parameter x and body ef . To evaluate (fe) in a current environment Enva , we must evaluate e in this environment to obtain a value v. The body ef must then be evaluated in an environment Env in which x is bound to v: this binding allows us to replace the formal parameter with the actual parameter. So what is this environment Env? Let f be the expression fun x1 -> x1 + x2: evaluation of (f 2) creates the binding (x1,2),