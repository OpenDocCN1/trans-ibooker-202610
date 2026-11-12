```
Python

def trans_lang1(st,a):
  (env,mem) = st
  (x,e) = a
  v = valeur_de(env,x)
  if isinstance(v,CRef1):
    ve = eval_expi(env,m,e)
    if isinstance(v,VCste1):
      return (env,write_mem(mem,v.cst_adr,ve.cste))
  raise ValueError

def trans_lang1_exec(st,la):
  (env,mem) = st
  if len(la) == 0:return (env,mem)
  else: return trans_lang1_exec(trans_lang1((env,mem),la[0]),la[1:])
```

```
OCaml
let trans_lang1 (env,mem) (x, e) = match valeur_de env x with
  | Some (CRef1 (Ref_Var1 y)) -> (match eval_expi env e with
    | VCste1 v -> (env, (write_mem mem (Ref_Var1 y) v))
    | Erreur1 -> failwith "Eval error")
  | _ -> failwith "Undefined var"
val trans_lang1 :
  (?a * 'b refer const1) list * ('b refer * 'b refer const1) list
  -> 'a * 'a expi
  -> ('a * 'b refer const1) list * ('b refer * 'b refer const1) list
let trans_lang1_exec (env, mem) la =
  (List.fold_left trans_lang1 (env, mem) la)
val trans_lang1_exec :
  (?a * 'b refer const1) list * ('b refer * 'b refer const1) list
  -> ('a * 'a expi) list
  -> ('a * 'b refer const1) list * ('b refer * 'b refer const1) list
```

Considering example 2.4, we obtain:

```
Python
ex_la0 = [[("y",Plus1(Bang1("y"),Var1("x"))), ("y",Cste1(8))]]
(ex_e1,ex_m1) = trans_lang1_exec((ex_e0,ex_m0),ex_la0)
>>> eval_expi(ex_e1,ex_m1,Bang1("y")).cste.cst_int
8
```

```
OCaml
let ex_la0 = [ ("y", Plus1 (Bang1 "y",Var1 "x")); ("y", Cste1 8) ]
val ex_la0 : (string * string expi) list
# (trans_lang1_exec (trans_defi_exec ([],[]) ex_ld0) ex_la0)) ;;
- : (string * string refer const1) list *
  (string refer * string refer const1) list
= ([("x", CInt1 5); ("y", CRef1 (Ref_Var1 "y"))], [(Ref_Var1 "y", CInt1 8)])
```