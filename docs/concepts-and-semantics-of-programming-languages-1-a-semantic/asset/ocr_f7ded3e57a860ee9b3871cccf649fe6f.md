The existence of a pair $(r, v)$ in a memory records that an initialization or a writing operation has been carried out at this location. Every referenced memory cell may be consulted through *reading* and can be assigned a new value by *writing*. In this case, the value previously held in the cell is deleted, it has "crashed".

Writing a value $v$ at an address $r$ transforms a memory $\text{Mem}$ into a memory denoted as $\text{Mem}[r := v]$; if a value was stored at this location $r$ in $\text{Mem}$, then this "old" value is replaced by $v$; otherwise, a new pair is added to $\text{Mem}$ to take account of the writing operation. There is no masking, contrary to the case of the environments. Writing a new value $v$ at a location $r$ that already contains a value deletes the old value $\text{Mem}(r)$:

```
[(r_1, v_1), \cdots, (r_i, v_i), \cdots, (r_n, v_n)][r_i := v'_i] = [(r_1, v_1), \cdots, (r_i, v'_i), \cdots, (r_n, v_n)]
```

The domain of a memory $\text{dom}(\text{Mem})$ depends on the current environment and represents the space of references, which are accessible (directly or indirectly) from bound and non-masked identifiers in the current execution environment. The addition of a binding $(x, r)$ to an environment $\text{Env}$ has a twofold effect, creating $(x, r) \oplus \text{Env}$ and extending $\text{Mem}$ to $\text{Mem}[r := v]$.

NOTE.— Depending on the language or program in question, the value $v$ may be supplied just when $x$ is introduced, or later, or never. If no value is provided prior to its first use, the result of the program is unpredictable, leading to errors called initialization errors. Indeed, a location always contains a value, which does not need to be suited to the current computation if it has not been explicitly determined by the program.

Note that the addition of a binding $(x, r)$ in an environment $\text{Env}$, of which the domain contains $x$, may mask a previous binding of $x$ in $\text{Env}$, but will not add a new pair $(r, v)$ to $\text{Mem}$ if $r$ was already present in the domain of $\text{Mem}$. Thus, any list of pairs representing a memory cannot contain two different pairs for the same reference. The memory $\text{Mem}[r := v]$ verifies the following property:

```
\text{Mem}[r := v](r') = \begin{cases} v & \text{if } r = r' \\ \text{Mem}(r') & \text{if } r \neq r' \end{cases}
```

The memory $(\text{Mem}[r_1 := v_1])[r_2 := v_2]$ is denoted as $\text{Mem}[r_1 := v_1][r_2 := v_2]$.

For example, $(\text{Mem}[r := v_1][r := v_2])(r) = v_2$.

> The function *valeur_ref* computes the value stored at a given location. If nothing has previously been written at this location, the function returns a special value (None), indicating the absence of a known value (i.e. a value resulting from initialization or computation).