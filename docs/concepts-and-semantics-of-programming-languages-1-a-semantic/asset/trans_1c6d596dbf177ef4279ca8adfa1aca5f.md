

## 框 4.2. Lang3 的大步操作语义

$(C_1) \frac{\text{Env}(x) = r \in \mathbf{R} \quad \llbracket e \rrbracket_{\text{Env}}^{\text{Mem}_1} = (v, \text{Mem}_2) \in \mathbf{V} \times \mathbf{M}}{\langle \text{Mem}_1, x := e \rangle \rightarrow_{\text{Env}} \text{Mem}_2[r := v]}$

$(C_2) \frac{}{\langle \text{Mem}, \text{\textbf{跳过}} \rangle \rightarrow_{\text{Env}} \text{Mem}}$

$(C_3) \frac{\langle \text{Mem}, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}_1 \quad \langle \text{Mem}_1, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{\langle \text{Mem}, c_1 ; c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}$

$(C_4) \frac{\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{\textit{真}}, \text{Mem}_1) \quad \langle \text{Mem}_1, c_1 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{\langle \text{Mem}, \text{\textbf{如果}} e \text{\textbf{则}} c_1 \text{\textbf{否则}} c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}$

$(C_5) \frac{\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{\textit{假}}, \text{Mem}_1) \quad \langle \text{Mem}_1, c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}{\langle \text{Mem}, \text{\textbf{如果}} e \text{\textbf{则}} c_1 \text{\textbf{否则}} c_2 \rangle \rightarrow_{\text{Env}} \text{Mem}_2}$

$(C_6) \frac{\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{\textit{真}}, \text{Mem}_1) \quad \langle \text{Mem}_1, c \rangle \rightarrow_{\text{Env}} \text{Mem}_2 \quad \langle \text{Mem}_2, \text{\textbf{当}} e \text{\textbf{执行}} c \rangle \rightarrow_{\text{Env}} \text{Mem}_3}{\langle \text{Mem}, \text{\textbf{当}} e \text{\textbf{执行}} c \rangle \rightarrow_{\text{Env}} \text{Mem}_3}$

$(C_7) \frac{\llbracket e \rrbracket_{\text{Env}}^{\text{Mem}} = (\text{\textit{假}}, \text{Mem}_1)}{\langle \text{Mem}, \text{\textbf{当}} e \text{\textbf{执行}} c \rangle \rightarrow_{\text{Env}} \text{Mem}_1}$

规则 \(C_2\) 仅指示执行指令 skip 不会影响状态。规则 \(C_3\) 描述了执行两条指令序列期间内存的演变。规则 \(C_4\) 和 \(C_5\) 指定与条件语句相关的内存转换；它们包含一个应用条件，该条件取决于表达式 \(e\) 的求值结果。

最后，规则 \(C_6\) 和 \(C_7\) 与 **当** \(e\) **执行** \(c\) 循环相关，并指定根据条件表达式 \(e\) 的值，循环体 \(c\) 的执行应重复多少次。与其他指令不同，**当** \(e\) **执行** \(c\) 允许我们编写执行不会终止的程序，因此，这种程序无法由推导树描述，因为这样的树是有限的树。对于这种非终止程序 \(c\)，不存在内存状态 \(\text{Mem}'\) 使得 \(\langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}'\)。例如，取程序 \(\text{\textit{foo}} = \text{\textbf{当}} \text{\textit{真}} \text{\textbf{执行}} c\)，不存在内存状态 \(\text{Mem}'\) 使得 \(\langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} \text{Mem}'\)。事实上，这个程序包含一个无限循环，因此不可能为它构建有限执行树：

$\frac{ \begin{array}{c} \llbracket \text{\textit{真}} \rrbracket_{\text{Env}}^{\text{Mem}} \\ (C_2) \frac{ \langle \text{Mem}, \text{\textbf{跳过}} \rangle \rightarrow_{\text{Env}} \text{Mem} }{ } \\ (C_6) \frac{ \vdots }{ \langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} ? } \end{array} }{ \langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} ? }$ \\
\((C_6) \frac{ \dots }{ \langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} ? }\)

由于 \(\langle \text{Mem}, \text{\textit{foo}} \rangle \rightarrow_{\text{Env}} ?\) 既出现在根节点，也出现在前提中。大步操作语义使用推导树——这些树是有限的——因此无法考虑非终止程序的执行。一种更精细的方法，适合描述非终止程序的执行，将在第 4.4.2 节中介绍。

让我们实现一个函数，给定状态 (Env, Mem) 和程序 c，计算新内存 Mem’，使得 \(\langle\text{Mem}, c\rangle \rightarrow_\text{Env} \text{Mem}'\)。然而请注意，就像第 3 章中定义的求值函数一样，这不符合数学函数。实际上，如果将此函数应用于在给定状态下执行不会终止的程序，则该函数可能不会终止。当执行导致无限循环时就会出现这种情况（如我们在第 3 章中看到的，对应于递归函数应用的表达式，求值导致了无限次递归调用）。此函数复制了框 4.2 中语义规则描述的执行机制。因此，它不是组合式的：程序的执行不仅仅对应于其各个组件的执行。规则 \(C_6\) 描述了使用组件 c 的执行来执行指令 **当** 执行 c，但也包括循环本身的执行。我们将在第 4.7.1 节回到这一点。由于我们假设程序执行不会产生类型错误，因此这里只需引入一个异常 Exec_Error，在执行出错时引发。再次说明，随着语言 \(\text{Lang}_3\) 的扩展，此函数的定义将被扩展。

```
Python
def exec_pgm4(env,mem,c):
    if isinstance(c,IAffect4):
        r = valeur_de(env,c.symb)
        if r is None: raise ValueError
        if isinstance(r,VCste4) and isinstance(r.cst,CRef4):
            v,mem1 = eval_exp4(env,mem,c.exp)
            if isinstance(v,VCste4):
                return write_mem(mem1,r.cst.cst_adr,v.cst)
            raise ValueError
        raise ValueError
    if isinstance(c,ISkip4): return mem
    if isinstance(c,ISeq4):
        return exec_pgm4(env,exec_pgm4(env,mem,c.pgm1),c.pgm2)
    if isinstance(c,ICond4):
        b,mem1 = eval_exp4(env,mem,c.exp)
        if isinstance(b,VCste4) and isinstance(b.cst,CBool4):
            if b.cst.cst_bool: return exec_pgm4(env,mem1,c.pgm1)
            else: return exec_pgm4(env,mem1,c.pgm2)
        raise ValueError
    if isinstance(c,IWhile4):
        b,mem1 = eval_exp4(env,mem,c.exp)
        if isinstance(b,VCste4) and isinstance(b.cst,CBool4):
            if b.cst.cst_bool:
                return exec_pgm4(env,exec_pgm4(env,mem1,c.pgm),c)
            else: return mem1
        raise ValueError
    if ...:
        raise ValueError
```

```
OCaml
exception Exec_Error

let rec exec_pgm4 env mem c = match c with
  | Iaffect4 (x, e) -> (
    match ((valeur_de env x), (eval_exp4 env mem e)) with
    | (Some (VCste4 (CRef4 a)), (VCste4 v, mem1)) -> write_mem mem1 a v
    | _ -> raise Exec_Error)
  | ISkip4 -> mem
  | ISeq4 (c1, c2) -> exec_pgm4 env (exec_pgm4 env mem c1) c2
  | ICond4(e,c1,c2) -> (match eval_exp4 env mem e with
    | (VCste4 (CBool4 true), mem1) -> exec_pgm4 env mem1 c1
    | (VCste4 (CBool4 false), mem1) -> exec_pgm4 env mem1 c2
    | _ -> raise Exec_Error)
  | IWhile4 (e, c1) -> (match eval_exp4 env mem e with
    | (VCste4 (CBool4 true), mem1) -> exec_pgm4 env (exec_pgm4 env mem1 c1) c
    | (VCste4 (CBool4 false), mem1) -> mem1
    | _ -> raise Exec_Error)
  | ...
val exec_pgm4 : (?a * ?a valeurs4) list -> (int * const4) list -> ?a pgm4
       -> (int * const4) list
```

## 示例 4.3. 示例 4.1 中程序的执行：

```
while not(|x| = y) do if |x| ≤ |y| then y := |y| - |x| else x := |x| - |y|
   e_w           e       c_1           c_2
          c_w
```

在状态 (Env, Mem) = ((y, ry) ⊕ (x, rx) ⊕ Env0, Mem0[rx := 4][ry := 2]) 下，由以下推导树描述：

[[e_w]]^{\text{Mem}}_{\text{Env}} = (\text{真}, \text{Mem})     ∇   ∇
                  1   2
(C6) <Mem, 当 e_w 执行 c_w> →^*_{\text{Env}} Mem[rx := 2]

其中 ∇1 是树：

[[e]]^{\text{Mem}}_{\text{Env}} = (\text{假}, \text{Mem})   (C1) Env(x) = rx   [[x - |y|]]^{\text{Mem}}_{\text{Env}} = (2, \text{Mem})
(C5) -------------------------------------------------------------
     <Mem, c2> →^*_{\text{Env}} Mem[rx := 2]
     -------------------------------------------------------------
     <Mem, c_w> →^*_{\text{Env}} Mem[rx := 2]

其中 ∇2 是树：

[[e_w]]^{\text{Mem[rx:=2]}}_{\text{Env}} = (\text{假}, \text{Mem[rx := 2]})
(C5) ----------------------------------------------------------
     <Mem[rx := 2], 当 e_w 执行 c_w> →^*_{\text{Env}} Mem[rx := 2]

