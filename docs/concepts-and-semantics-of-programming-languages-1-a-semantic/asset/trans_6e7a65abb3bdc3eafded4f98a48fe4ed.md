
这两种方法是等价的。

| e | [[e]]_Env | |
|---|---|---|
| v | v | (v ∈ K) |
| x | Env(x) | 如果 x ∈ dom(Env) (x ∈ X) |
| x | Err_U | 如果 x ∉ dom(Env) |
| op e1 | op_V [[e1]]_Env | 如果 [[e1]]_Env ∈ V (op ∈ {not}) |
| op e1 | [[e1]]_Env | 如果 [[e1]]_Env ∈ IE |
| e1 op e2 | [[e1]]_Env op_V [[e2]]_Env | (op ∈ {+, -, *, /, and, or, =, ≤}) 如果 [[e1]]_Env ∈ V 且 [[e2]]_Env ∈ V |
| e1 op e2 | [[e1]]_Env | 如果 [[e1]]_Env ∈ IE |
| e1 op e2 | [[e2]]_Env | 如果 [[e1]]_Env ∈ V 且 [[e2]]_Env ∈ IE |
| if e1 then e2 else e3 | [[e2]]_Env | 如果 [[e1]]_Env = true |
| if e1 then e2 else e3 | [[e3]]_Env | 如果 [[e1]]_Env = false |
| if e1 then e2 else e3 | Err_T | 如果 [[e1]]_Env ∈ V \ IB |
| if e1 then e2 else e3 | [[e1]]_Env | 如果 [[e1]]_Env ∈ IE |
| let x = e1 in e2 | [[e2]]_(x,v)⊕Env | 如果 [[e1]]_Env = v ∈ V |
| let x = e1 in e2 | [[e1]]_Env | 如果 [[e1]]_Env ∈ IE |
| fun x ↦ e | <x, e, Env> | |
| rec f x = e | <f, x, e, Env> | |
| (e1 e2) | [[ef]]_(x,v)⊕Env_f | 如果 [[e1]]_Env = <x, ef, Env_f> 且 [[e2]]_Env = v ∈ V |
| (e1 e2) | [[ef]]_(f,f,x,ef,Env_f))⊕(x,v)⊕Env_f | 如果 [[e1]]_Env = <f, x, ef, Env_f> 且 [[e2]]_Env = v ∈ V |
| (e1 e2) | [[e2]]_Env | 如果 [[e1]]_Env ∈ F 且 [[e2]]_Env ∈ IE |
| (e1 e2) | Err_T | 如果 [[e1]]_Env ∉ F |

### 表 3.3. Exp2 中表达式的求值

注.— 与第 2 章中介绍的语言 Def1 不同，Exp2 不提供将绑定添加到当前环境所需的 let x = e; 构造。但是，let x = e1 in e2 构造使得可以模拟它。要在环境 [(x1, v1), (x2, v2), · · · , (xn, vn)] 中求值表达式 e，我们只需考虑以下表达式：

```
let x1 = v1 in
let x2 = v2 in
...
let xn = vn in e
```

示例 3.2.– [3.3] 中定义的表达式求值如下：

```
[[let x = 25 in let f = fun x ↦ x + 3 in (f 7) + x]]Env0
= [[let f = fun x ↦ x + 3 in (f 7) + x]]Env1
= [[(f 7) + x]]Env2 = [[(f 7)]]Env2 + [[x]]Env2 = [[x + 3]](x,7)⊕Env1 + 25
= 7 + 3 + 25 = 35
```

其中：

```
ENV1 = (x, 25) ⊕ ENV0 since [[25]]Env0 = 25
ENV2 = (f, ⟨x, x + 3, ENV1⟩) ⊕ ENV1 since [[fun x ↦ x + 3]]Env1 = ⟨x, x + 3, ENV1⟩
```

请注意，变量 x 的每个出现都以适应其含义的方式进行处理。

示例 3.3.– 表达式 (efact 3) 的求值过程如下，其中 efact 是 [3.2] 中定义的表达式：

```
[[((rec fact n = if n ≤ 1 then 1 else (fact (n - 1)) * n) 3)]]Env0
efact
= [[if n ≤ 1 then 1 else (fact (n - 1)) * n]]Env1
(since [[efact]]Env0 = Ffact and [[3]]Env0 = 3)
= [[(fact (n - 1)) * n]]Env1
(since [[n ≤ 1]]Env1 = false since [[n]]Env1 = 3)
= [[(fact (n - 1))]]Env1 * [[n]]Env1
= [[if n ≤ 1 then 1 else (fact (n - 1)) * n]]Env2 * [[n]]Env1
(since [[fact]]Env1 = Ffact and [[n - 1]]Env1 = [[n]]Env1 - 1 = 3 - 1 = 2)
= [[(fact (n - 1)) * n]]Env2 * [[n]]Env1
(since [[n ≤ 1]]Env2 = false since [[n]]Env2 = 2)
= [[(fact (n - 1))]]Env2 * [[n]]Env2 * [[n]]Env1
= [[if n ≤ 1 then 1 else (fact (n - 1)) * n]]Env3 * [[n]]Env2 * [[n]]Env1
(since [[fact]]Env2 = Ffact and [[n - 1]]Env2 = [[n]]Env2 - 1 = 2 - 1 = 1)
= [[1]]Env3 * [[n]]Env2 * [[n]]Env1
(since [[n ≤ 1]]Env3 = true since [[n]]Env3 = 1)
= 1 * 2 * 3 = 6
```

其中：

```
Ffact = ⟨fact, n, if n ≤ 1 then 1 else (fact (n - 1)) * n, ENV0⟩
ENV1 = (fact, Ffact) ⊕ (n, 3) ⊕ ENV0
ENV2 = (fact, Ffact) ⊕ (n, 2) ⊕ ENV0
ENV3 = (fact, Ffact) ⊕ (n, 1) ⊕ ENV0
```

请注意，每次递归调用都在闭包 Ffact 中包含的环境 ENV0 上求值，并将 n 绑定到其当前值。

表 3.3 中所示 **Exp**$_2$ 表达式的求值函数可直接定义如下：

```python
def eval2(env,e):
    if isinstance(e,Cste2):
        if isinstance(e.cste,CInt2): return Val2(VInt2(e.cste.cst_int))
        if isinstance(e.cste,VBool2): return Val2(VBool2(e.cste.cst_bool))
    if isinstance(e,Var2):
        x = valeur_de(env,e.symb)
        if x is None: return Err2(E_Undef2)
        return Val2(x)
    if isinstance(e,U_Op2):
        a = eval2(env,e.exp1)
        if isinstance(a,Val2): return apply2_unary_op(e.op,a.val)
        return a
    if isinstance(e,B_Op2):
        a1 = eval2(env,e.exp1)
        if isinstance(a1,Val2):
            a2 = eval2(env,e.exp2)
            if isinstance(a2,Val2):
                return apply2_binary_op(e.op,a1.val,a2.val)
            return a2
        return a1
    if isinstance(e,If2):
        a1 = eval2(env,e.exp1)
        if isinstance(a1,Val2):
            if isinstance(a1.val,VBool2):
                if a1.val.val_bool == True: return eval2(env,e.exp2)
                return eval2(env,e.exp3)
        return Err2(E_Type2)
        return a1
    if isinstance(e,Letin2):
        a1 = eval2(env,e.exp1)
        if isinstance(a1,Val2):
            ne = ajout_liaison_env(env,e.symb,a1.val)
            return eval2(ne,e.exp2)
        return a1
    if isinstance(e,Fun2): return Val2(Clos2(e.param,e.corps,env))
    if isinstance(e,Rec2):
        return Val2(ClosRec2(e.nom,e.param,e.corps,env))
    if isinstance(e,App2):
        a1 = eval2(env,e.exp1)
        if isinstance(a1,Val2):
            if isinstance(a1.val,Clos2):
                a2 = eval2(env,e.exp2)
                if isinstance(a2,Val2):
                    ne = ajout_liaison_env(a1.val.envdef,a1.val.param,a2.val)
                    return eval2(ne,a1.val.corps)
                return a2
            if isinstance(a1.val,ClosRec2):
                a2 = eval2(env,e.exp2)
                if isinstance(a2, Val2):
                    ne = ajout_liaison_env(ajout_liaison_env(a1.val.envdef,
                        a1.val.nomf,
                        a1.val.param, a2.val),
                    return eval2(ne, a1.val.corps)
                return a2
            return Err2(E_Typ2)
        return a1
raise ValueError
```

```ocaml
let rec eval2 env e = match e with
| Cste2 (CInt2 n) -> Val2 (VInt2 n)
| Cste2 (CBool2 n) -> Val2 (VBool2 n)
| Var2 x -> (match valeur_de_env x with
| Some v -> Val2 v | None -> Err2 E_Undef2)
| U_Op2 (op, e1)-> (match eval2 env e1 with
| Val2 v -> apply2_unary_op op v | z -> z)
| B_Op2 (op, e1, e2) -> (match eval2 env e1 with
| Val2 v1 -> (match eval2 env e2 with
| Val2 v2 -> apply2_binary_op op v1 v2 | z -> z)
| z -> z)
| If2 (e1, e2, e3) -> (match eval2 env e1 with
| Val2 (VBool2 true) -> eval2 env e2
| Val2 (VBool2 false) -> eval2 env e3
| Val2 x -> Err2 E_Typ2
| z -> z)
| Letin2 (x, e1, e2) -> (match eval2 env e1 with
| Val2 v -> eval2 (ajout_liaison_env x v) e2 | z -> z)
| Fun2 (x, e1) -> Val2 (Clos2 (x, e1, env))
| Rec2 (f, x, e1) -> Val2 (ClosRec2 (f, x, e1, env))
| App2 (e1, e2) -> (match eval2 env e1 with
| Val2 (Clos2 (pf, cf, ef)) -> (match eval2 env e2 with
| Val2 v -> eval2 (ajout_liaison_env ef pf v) cf | z -> z)
| Val2 (ClosRec2 (nf, pf, cf, ef)) -> (match eval2 env e2 with
| Val2 v ->
eval2 (ajout_liaison_env
(ajout_liaison_env
ef nf (ClosRec2(nf,pf,cf,ef))) pf v) cf
| z -> z)
| _ -> Err2 E_Typ2)
val eval2 : (’a * ’a valeur2) list -> ’a exp2 -> ’a val_err2
```

示例 3.2 和 3.3 中表达式的求值可通过以下方式得到：

```python
e25 = Letin2("x",Cste2(CInt2(25)),
       Letin2("f",Fun2("x",B_Op2(Plus2(),Var2("x"),Cste2(CInt2(3)))),
              B_Op2(Plus2(),App2(Var2("f"),Cste2(CInt2(7))),Var2("x"))))
>>> (eval2([],e25)).val.val_int
35
>>> (eval2([],App2(a_fact,Cste2(CInt2(3))))).val.val_int
6
```

```ocaml
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

## 3.2.5.1. 子表达式求值顺序

表达式的子表达式求值顺序会影响最终求值的结果。表 3.3 中定义的求值函数任意确定了 e1 op e2 子表达式的求值顺序：我们选择先求值 e1，再求值 e2。如果求值 e1 触发错误，则不会求值 e2。因此，数学上交换的二元运算符的实现失去了这一属性。例如，如果变量 x 不属于环境 Env 的定义域，则 [[(3 / 0) + x]]Env ≠ [[x + (3 / 0)]]Env。确实，我们有：

[[(3 / 0) + x]]Env = [[3 / 0]]Env = ErrTE [[x + (3 / 0)]]Env = [[x]]Env = ErrTU

同理，一些所谓的“惰性”布尔运算符也是如此。例如，在求值 e1 和 e2 时，如果 e1 求值为 false，则可能在不求值 e2 的情况下返回结果 false（因为无论何种情况合取值都为 false）；因此，e2 中的执行错误不会被检测到。

另一方面，在应用 (e1 e2) 的求值中，未指定 e1 和 e2 的求值顺序。尽管如此，当语义在编译器或解释器中实现时，必须做出选择。例如，在前面介绍的解释器 eval2 中，e1 在 e2 之前求值。在某些“惰性”语言中，函数的参数 e2 仅在必要时才求值（如果 e1 是常量函数，则无需求值参数；这意味着参数中的执行错误不会被检测到）。
