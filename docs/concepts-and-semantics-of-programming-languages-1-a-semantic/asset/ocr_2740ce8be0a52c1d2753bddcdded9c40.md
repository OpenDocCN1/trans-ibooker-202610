Now, considering example 2.3, we obtain:

```
Python
ex__ld0 = [Var_def1("y",Cste1(2)), Let_def1("x",Plus1(Bang1("y",Cste1(3)))]

(ex_e0,ex_m0) = trans_def1_exec(([],[]),ex__ld0)
>>> eval_exp1(ex_e0,ex_m0,Var1("x")).cste.cst_int
5
>>> eval_exp1(ex_e0,ex_m0,Bang1("y")).cste.cst_int
2
```

```
OCaml
let ex__ld0 = [ Var_def1 ("y", Cste1 2);
              Let_def1 ("x", Plus1 (Bang1 "y", Cste1 3)) ]
val ex__ld0 : string def_list
# (trans_def1_exec ( [], []) ex__ld0 ) ;;
- : (string * string refer const1) list *
    (string refer * string refer const1) list
= ([("x", CInt1 5); ("y", CRef1 (Ref_Var1 "y"))], [(Ref_Var1 "y", CInt1 2)])
```

# ### 2.3.2. Assignment

The language **Lang**$_1$ extends **Def**$_1$ by adding _assignment_. The syntax of an assignment instruction is:

$x := e$

where \(x \in \mathbf{X}\) and \(e \in \mathbf{Exp}_1\). When the mutable variable \(x\) is already bound in the current environment, this instruction enables us to modify the value of !\(x\). Formally, execution of the instruction \(x := e\) modifies the memory of the current state, and it is described by the following transition:

\((\text{Env}, \text{Mem}) \xrightarrow{x:=e}_{\mathbf{Lang}_1} (\text{Env}, \text{Mem}[\text{Env}(x) := \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}}])\)

NOTE.- Once again, if the identifier \(x\) is not bound in the environment or if the evaluation of \(e\) results in an error, no state is generated and evaluation stops.

EXAMPLE 2.4.- Based on the state obtained in example 2.3, the following two assignments can be executed:

([(x, 5), (y, ry)], [(ry, 2)])
\(\xrightarrow{y:=y+x}_{\mathbf{Lang}_1} ([(x, 5), (y, ry)], [(ry, 7)])\)
\(\xrightarrow{y:=8}_{\mathbf{Lang}_1} ([(x, 5), (y, ry)], [(ry, 8)])\)

Representing the abstract syntax of the assignment \(x := e\) by the pair \((x, e)\), the relation \(\rightarrow_{\mathbf{Lang}_1}\) and the iteration of this relation from a sequence of assignments are implemented as follows: