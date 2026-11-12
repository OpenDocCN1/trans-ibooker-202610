

采用动态作用域机制，使用 Python 时，我们添加以下类：

```
class Ferm4:
    def __init__(self,param,pgm):
        self.param = param
        self.pgm = pgm
```

而使用 OCaml 时，和类型 ’a valeurs4 的定义通过构造函数进行扩展：

```
| Ferm4 of ’a * ’a pgm4
```

为了在考虑过程定义的同时扩展关系 →Def3，我们需要修改在第 4.3 节中定义的函数 trans_def4。

使用 Python 时，在静态作用域的情况下，这是通过将以下指令插入函数代码中完成的：

```
if isinstance(d,Let_proc4):
    return (ajout_liaison_env(env,d.name,Ferm4(d.symb,d.pgm,d.env)),mem)
```

而在动态作用域的情况下，插入以下指令：

```
if isinstance(d,Let_proc4):
    return (ajout_liaison_env(env,d.name,Ferm4(d.symb,d.pgm)),mem)
```

使用 OCaml 时，在静态作用域下，我们使用以下模式扩展函数 trans_def4：

```
| Let_proc4(p,x,c) -> ((ajout_liaison_env env p (Ferm4(x,c,env))),mem)
```

在动态作用域下，我们添加以下模式：

```
| Let_proc4(p,x,c) -> ((ajout_liaison_env env p (Ferm4(x,c))),mem)
```

## 4.6.2.2. 过程调用机制

### 4.6.2.2.1. 按值调用

在调用过程时，按值调用机制传递一个值，该值是在调用的当前环境中对表达式 e 求值得到的结果。使用此机制，Lang$_3$ 中过程调用的语法结构如下所示：

call p(e)

为了在 Python 中添加过程调用的语法结构，我们定义以下类：

```
class ICall4:
    def __init__(self,name,exp):
        self.name = name
        self.exp = exp
```

使用 OCaml 时，和类型 ’a pgm4（参见第 4.1 节）的定义通过以下构造函数进行扩展：

```
| ICall4 of ’a * ’a exp4
```

方框 4.6 介绍了该调用机制的不同语义。在第一种版本中，传递的值被视为常量：参数与该值的绑定直接出现在环境中（因此该值不能被修改）。

在第二种版本中，过程的参数被视为过程局部的可变变量，并使用传递的值进行初始化。在此情况下，会进行一次分配，参数引用该位置，并且该位置的内容可以由过程体中的指令修改。与块的情况一样，一旦过程执行完毕，该引用可能仍然可访问，前提是它被赋值给了过程“外部”的变量。这两种版本均针对静态和动态作用域机制进行了介绍。

### 按值调用：常量参数

静态作用域   \( (C_9) \)   
\( \text{Env}(p) = \langle x, c, \text{Env}_p \rangle \quad \llbracket e \rrbracket^{\text{Mem}}_{\text{Env}} = (v, \text{Mem}') \\ \text{Env}' = (p, \langle x, c, \text{Env}_p \rangle) \oplus \langle x, v \rangle \oplus \text{Env}_p \\ \langle \text{Mem}', c \rangle \rightarrow_{\text{Env}'} \text{Mem}'' \\ \hline \langle \text{Mem}, \text{call } p(e) \rangle \rightarrow_{\text{Env}} \text{Mem}'' \)

动态作用域   \( (C_9) \)   
\( \text{Env}(p) = \langle x, c \rangle \quad \llbracket e \rrbracket^{\text{Mem}}_{\text{Env}} = (v, \text{Mem}') \\ \text{Env}' = (p, \langle x, c \rangle) \oplus \langle x, v \rangle \oplus \text{Env} \\ \langle \text{Mem}', c \rangle \rightarrow_{\text{Env}'} \text{Mem}'' \\ \hline \langle \text{Mem}, \text{call } p(e) \rangle \rightarrow_{\text{Env}} \text{Mem}'' \)

### 按值调用：可变参数

静态作用域   \( (C_9) \)   
\( \text{Env}(p) = \langle x, c, \text{Env}_p \rangle \quad \llbracket e \rrbracket^{\text{Mem}}_{\text{Env}} = (v, \text{Mem}') \\ \text{Env}' = (p, \langle x, c, \text{Env}_p \rangle) \oplus \langle x, r_x \rangle \oplus \text{Env}_p \\ \langle \text{Mem}'[r_x := v], c \rangle \rightarrow_{\text{Env}'} \text{Mem}'' \\ \hline \langle \text{Mem}, \text{call } p(e) \rangle \rightarrow_{\text{Env}} \text{Mem}'' \)

动态作用域   \( (C_9) \)   
\( \text{Env}(p) = \langle x, c \rangle \quad \llbracket e \rrbracket^{\text{Mem}}_{\text{Env}} = (v, \text{Mem}') \\ \text{Env}' = (p, \langle x, c \rangle) \oplus \langle x, r_x \rangle \oplus \text{Env} \\ \langle \text{Mem}'[r_x := v], c \rangle \rightarrow_{\text{Env}'} \text{Mem}'' \\ \hline \langle \text{Mem}, \text{call } p(e) \rangle \rightarrow_{\text{Env}} \text{Mem}'' \)

### 按引用调用

静态作用域   \( (C_9) \)   
\( \text{Env}(p) = \langle x_p, c, \text{Env}_p \rangle \quad \text{Env}(x) = r_x \in \mathbf{R} \\ \text{Env}' = (p, \langle x_p, c, \text{Env}_p \rangle) \oplus \langle x_p, r_x \rangle \oplus \text{Env}_p \\ \langle \text{Mem}, c \rangle \rightarrow_{\text{Env}'} \text{Mem}' \\ \hline \langle \text{Mem}, \text{call } p(x) \rangle \rightarrow_{\text{Env}} \text{Mem}' \)

动态作用域   \( (C_9) \)   
\( \text{Env}(p) = \langle x_p, c \rangle \quad \text{Env}(x) = r_x \in \mathbf{R} \\ \text{Env}' = (p, \langle x_p, c \rangle) \oplus \langle x_p, r_x \rangle \oplus \text{Env} \\ \langle \text{Mem}, c \rangle \rightarrow_{\text{Env}'} \text{Mem}' \\ \hline \langle \text{Mem}, \text{call } p(x) \rangle \rightarrow_{\text{Env}} \text{Mem}' \)

### 方框 4.6. 大步操作语义：过程调用

使用静态作用域机制和按常量参数传值调用，我们向 Python 函数 exec_pgm4（参见第 4.4.1 节）的代码中添加以下指令：

```
if isinstance(c, ICall4):
    v,mem1 = eval_exp4(env,mem,c.exp)
    fp = valeur_de(env,c.name)
    if fp is None: raise ValueError
    if isinstance(fp, Ferm4) and isinstance(v, VCste4):
        enve = ajout_liaison(ajout_liaison_env(fp.env,fp.param,v),
            c.name,fp)
        return exec_pgm4(enve,mem1,fp.pgm)
    raise ValueError
raise ValueError
```

在这种情况下，使用 OCaml 时，函数 exec_pgm4 的定义被扩展以包含以下模式：

```
| ICall4(p,e) -> (match ((valeur_de env p),(eval_exp4 env mem e)) with
    (Some(Ferm4(xp,cp)),(VCste4(v),mem1))
    -> let enve = (ajout_liaison_env
        (ajout_liaison_env envp xp (VCste4(v)))
        p (Ferm4(xp,cp,envp)))
    in (exec_pgm4 enve mem1 cp)
| _ -> raise Exec_Error)
```

使用动态作用域机制和按常量参数传值调用，我们向 Python 函数 exec_pgm4 的代码中添加以下指令：

```
if isinstance(c, ICall4):
    v,mem1 = eval_exp4(env,mem,c.exp)
    fp = valeur_de(env,c.name)
    if fp is None:
        raise ValueError
    if isinstance(fp, Ferm4) and isinstance(v, VCste4):
        enve = ajout_liaison(ajout_liaison_env(env,fp.param,v),
            c.name,fp)
        return exec_pgm4(enve,mem1,fp.pgm)
    raise ValueError
```

在这种情况下，使用 OCaml 时，函数 exec_pgm4 的定义被扩展以包含以下模式：

```
| ICall4(p,e) -> (match ((valeur_de env p),(eval_exp4 env mem e)) with
    (Some(Ferm4(xp,cp)),(VCste4(v),mem1))
-> let env = (ajout_liaison_env
(ajout_liaison_env env xp (VCste4(v)))
p (Ferm4(xp,cp)))
in (exec_pgm4 env mem1 cp)
| _ -> raise Exec_Error)
```

我们仅考虑上述两种情况的实现。其他情况（可变参数和按引用调用）以类似方式获得。

## 4.6.2.2.2. 按引用调用

按引用调用机制向过程传递一个值，该值是调用当前环境中存在的可变变量的引用。使用此机制，Lang3 中过程调用的语法结构为：

call p(x)

其中 x 是调用环境中的一个可变变量。执行 p 的过程体时，其内容可能会被直接修改。

该机制的正式定义在方框 4.6 中给出。与按值调用机制不同（在按值调用中，参数被视为过程局部变量），此情况下不进行分配。该机制将在第 7.4 节中更详细地讨论，我们将在其中解释使用指针的实现。

## 4.7. 其他方法

到目前为止，只考虑了小步和大步操作语义。两种情况都涉及定义一个转换关系，以表征由程序执行（以及程序中出现的表达式的求值，因为所选表达式语言允许副作用）导致的状态转换。在本节中，我们介绍了命令式语言语义的另外两种经典方法。为求简化，我们假设表达式的求值不会修改内存的状态，并写作 [[e]]Env Mem = v 以表示在状态 (Env, Mem) 中对 e 的求值返回值 v。我们使用语言 Lang3（不包含块和过程），其语法定义在表 4.3 中。

### 4.7.1. 直表示语义

#### 4.7.1.1. 与程序关联的偏函数

函数 exec_pgm4（在第 4.4.1 节中定义）计算在状态 (Env, Mem) 中执行程序 c 后获得的内存状态 Mem'。然而，如我们所述，exec_pgm4 不对应于一个数学函数，因为

