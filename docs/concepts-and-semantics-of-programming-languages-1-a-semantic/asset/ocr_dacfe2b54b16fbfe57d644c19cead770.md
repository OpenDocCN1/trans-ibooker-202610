```
| Eq4 -> (match (v1, v2) with
| (VCste4 (CInt4 n1), VCste4 (CInt4 n2)) -> VCste4 (CBool4 (n1 = n2))
| _ ->> raise Eval_Error)
| Leq4 -> (match (v1, v2) with
| (VCste4 (CInt4 n1), VCste4 (CInt4 n2)) -> VCste4 (CBool4 (n1 <= n2))
| _ ->> raise Eval_Error)
val apply4_binary_op : bin_op -> 'a valeurs4 -> 'b valeurs4 -> 'c valeurs4
```

It is now possible to define the function that evaluates an expression. Throughout this chapter, a state will be represented by an environment binding some values of V to variable symbols, and by a memory binding constants in K ∪ R to references (represented by integers).

```
def eval_exp4(env,mem,e):
    if isinstance(e,Cste4): return (VCste4(e.cste),mem)
    if isinstance(e,Var4):
        x = valeur_de(env,e.symb)
        if x is None: raise ValueError
        return (x,mem)
    if isinstance(e,U_Op4):
        v,memv = eval_exp4(env,mem,e.exp1)
        return (apply4_unary_op(e.op,v),memv)
    if isinstance(e,B_Op4):
        v1,mem1 = eval_exp4(env,mem,e.exp1)
        v2,mem2 = eval_exp4(env,mem1,e.exp2)
        return (apply4_binary_op(e.op,v1,v2),mem2)
    if isinstance(e,Ref4):
        v,mem1 = eval_exp4(env,mem,e.exp)
        r,mem2 = add_mem(mem1,v.cst)
        return (VCste4(CRef4(r)),mem2)
    if isinstance(e,Deref4):
        v1,mem1 = eval_exp4(env,mem,e.exp)
        if isinstance(v1,VCste4) and isinstance(v1.cst,CRef4):
            v2 = valeur_ref(mem1.v1.cst.cst_adr)
            if v2 is None: raise ValueError
            else: return (VCste4(v2),mem1)
        raise ValueError
    raise ValueError
```

```
let rec eval_exp4 env mem e = match e with
| Cste4 k ->> (VCste4 k, mem)
| Var4 x ->>
    match valeur_de env x with Some(v) ->> (v, mem) | _ ->> raise Eval_Error)
| U_Op4 (op, e1) ->> (match eval_exp4 env mem e1 with
    | (VCste4 v, mem1) ->> ((apply4_unary_op op (VCste4 v)), mem1)
    | _ ->> raise Eval_Error)
| B_Op4(op,e1,e2) ->> (match eval_exp4 env mem e1 with
    | (VCste4 v1, mem1) ->> (match eval_exp4 env mem1 e2 with
        | (VCste4 v2, mem2) ->>
            ((apply4_binary_op op (VCste4 v1) (VCste4 v2)), mem2)
```