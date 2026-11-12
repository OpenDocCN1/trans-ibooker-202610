
we can obtain the tree:

```
(C_i) \xrightarrow{1} (C_j) \xrightarrow{2} (C_k)
(C_3) \xrightarrow{} (C_3) \xrightarrow{} (C_3)
<Mem, (c_1 ; c_2) ; c_3> \rightarrow>_{Env} Mem_3
```

and vice versa.

- 2) Let \( c_1 \) be the instruction while \( e \) do \( c \) and \( c_2 \) the instruction if \( e \) then \( (c ; c_1) \) else skip. Let Mem\(_1\) and Mem\(_2\) be two memory states. We have to prove that:

<Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\) \leftrightarrow <Mem\(_1\), \( c_2 \)> \rightarrow>_{Env} Mem\(_2\)

We have to construct an inference tree for <Mem\(_1\), \( c_2 \)> \rightarrow>_{Env} Mem\(_2\) from an inference tree for <Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\), and vice versa.

(\(\Rightarrow\)) If <Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\), then two cases are possible.

### Case 1: The last applied rule is \( C_6 \):

[[e]]_{Env}^{Mem_1} = (true, Mem_1)   (C_j) ... (C_k)
(C_6)
<Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\)   [A.5]

and, the following inference tree can be constructed:

[[e]]_{Env}^{Mem_1} = (true, Mem_1)   (C_j) ... (C_k)
(C_4)
<Mem\(_1\), \( c_2 \)> \rightarrow>_{Env} Mem\(_2\)   [A.6]

### Case 2: The last applied rule is \( C_7 \):

(C_7) [[e]]_{Env}^{Mem_1} = (false, Mem\(_2\))
<Mem\(_1\), \( c_1 \)> \rightarrow>_{Env} Mem\(_2\)   [A.7]

and the following inference tree can be constructed:

```
⟦e⟧_Env^Mem1 = (false, Mem2)   (C2)   ⟨Mem2, skip⟩ →Env Mem2  
                                    ⟨Mem1, c2⟩ →Env Mem2       [A.8]  
(C5) ───────────────────────────────────────────────────
```

(⇐) If ⟨Mem1, c2⟩ →Env Mem2 then two cases are possible. Either the last applied rule is C4, and the tree [A.5] can be constructed from the tree [A.6], or the last applied rule is C5, and the tree [A.7] can be constructed from the tree [A.8]. Both cases conclude the proof.

## Solution to exercise 4.2

Throughout this exercise, we assume that for any expression e, ⟦e⟧_Env^Mem1 = (true, Mem2) (respectively, ⟦e⟧_Env^Mem1 = (false, Mem2)) if and only if ⟦not e⟧_Env^Mem1 = (false, Mem2) (respectively, ⟦not e⟧_Env^Mem1 = (true, Mem2)).

- 1) The semantics of the repeat until loop can be defined as follows:

```
⟨Mem1, c⟩ →Env Mem2   ⟦e⟧_Env^Mem2 = (true, Mem3)  
(C8) ────────────────────────────────────────────  
        ⟨Mem1, repeat c until e⟩ →Env Mem3
```

```
⟨Mem1, c⟩ →Env Mem2   ⟦e⟧_Env^Mem2 = (false, Mem3)   ⟨Mem3, repeat c until e⟩ →Env Mem4  
(C9) ────────────────────────────────────────────────────────────────────────  
                  ⟨Mem1, repeat c until e⟩ →Env Mem4
```

- 2) Execution tree for the program c:

```
repeat (z := z * ↓x; x := ↓x - 1) until ↓x = 0  
       c1      c2      e
```

```
∇1 ⟦e⟧_Env^Mem2 = (false, Mem2)   (C8)   ...  
(C9) ───────────────────────────────────────────  
      ⟨Mem, repeat c1 ; c2 until e⟩ →Env Mem3
```

where ∇1 is the tree:

```
Env(z) = rz    Env(x) = rx  
⟦z * ↓x⟧_Env^Mem = (6, Mem)   ⟦↓x - 1⟧_Env^Mem1 = (1, Mem1)  
(C1) ⟨Mem, c1⟩ →Env Mem [rz := 6]   (C1) ⟨Mem1, c2⟩ →Env Mem1 [rx := 1]  
(C3) ─────────────────────────────────────────────────────  
           ⟨Mem, c1 ; c2⟩ →Env Mem2
```

and ∇2 is the tree:

```
\((C_1) \frac{\text{Env}(z) = r_z \llbracket \downarrow z * \downarrow x \rrbracket^{Mem_2}_{Env} = (6, Mem_2)}{\langle Mem_2, c_1 \rangle \to_{Env} Mem_2} \quad (C_1) \frac{\text{Env}(x) = r_x \llbracket \downarrow x - 1 \rrbracket^{Mem_2}_{Env} = (0, Mem_2)}{\langle Mem_2, c_2 \rangle \to_{Env} Mem_2 [r_x := 0]}\)
\((C_3) \frac{}{\langle Mem_2, c_1 ; c_2 \rangle \to_{Env} Mem_3}\)
```

- 3) Let \(c_1\) be the instruction repeat \(c\) until \(e\) and \(c_2\) the instruction \(c\) ; if \(e\) then skip else \(c_1\). An inference tree for \(\langle Mem, c_1 \rangle \to_{Env} Mem'\) can be constructed from the inference tree for \(\langle Mem, c_2 \rangle \to_{Env} Mem'\) and vice versa. It suffices to consider the possible rules, which may be used to construct these trees and to examine the two possible values of \(e\). In both cases, we simply “combine” the inference trees as follows.

### Case 1: When \(\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')\), starting from the tree:

```
\((C_i) \frac{\nabla}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad (C_8) \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')}{\langle Mem, c_1 \rangle \to_{Env} Mem'}\)
```

we can obtain the tree:

```
\((C_i) \frac{\nabla}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')}{(C_4)} \quad (C_2) \frac{\langle Mem', \text{skip} \rangle \to_{Env} Mem'}{\langle Mem_1, \text{if e then skip else } c_1 \rangle \to_{Env} Mem'}\)
\((C_3) \frac{}{\langle Mem, c_2 \rangle \to_{Env} Mem'}\)
```

and vice versa.

### Case 2: When \(\llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2)\), starting from the tree:

```
\((C_i) \frac{\nabla 1}{\langle Mem, c \rangle \to_{Env} Mem_1} = \llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2) \quad (C_j) \frac{\nabla 2}{\langle Mem_2, c_1 \rangle \to_{Env} Mem'}\)
\((C_9) \frac{}{\langle Mem, c_1 \rangle \to_{Env} Mem'}\)
```

we can obtain the tree:

```
\((C_i) \frac{\nabla 1}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2)}{(C_5)} \quad (C_j) \frac{\nabla 2}{\langle Mem_2, c_1 \rangle \to_{Env} Mem'}\)
\((C_3) \frac{\langle Mem_1, \text{if e then skip else } c_1 \rangle \to_{Env} Mem'}{\langle Mem, c_2 \rangle \to_{Env} Mem'}\)
```

and vice versa.

- 4) Let \( c'_1 \) be the instruction while not \( e \) do \( c \) and \( c'_2 \) be the instruction repeat \( c \) until not \( e \) (see section 4.8.3).

For (P1), let us prove by structural induction over \(\langle \text{Mem}_H, c_H \rangle \to_{\text{Env}} \text{Mem}_H\) (using the induction scheme given in Box 4.4) that if:

```
\langle \text{Mem}_H, c_H \rangle \to_{\text{Env}} \text{Mem}_H = \langle \text{Mem}, c_1 \rangle \to_{\text{Env}} \text{Mem}'
```

then \(\langle \text{Mem}, c_2 \rangle \to_{\text{Env}} \text{Mem}'\). If \(\langle \text{Mem}_H, c_H \rangle \to_{\text{Env}} \text{Mem}_H\) was obtained using one of the rules \( C_1 \) to \( C_5 \) or \( C_8 \) or \( C_9 \), then the equality given above is false, which concludes the proof. Hence, we only need to consider rules \( C_6 \) and \( C_7 \).

### Case 1: If \(\langle \text{Mem}, c_1 \rangle \to_{\text{Env}} \text{Mem}'\) was obtained using rule \( C_6 \):

```
          [[e]]^{\text{Mem}}_{\text{Env}} = (true, \text{Mem}_1) \quad (C_i) \quad \frac{1}{\langle \text{Mem}_1, c \rangle \to_{\text{Env}} \text{Mem}_2} \quad \dots \quad \frac{}{\langle \text{Mem}_2, c_1 \rangle \to_{\text{Env}} \text{Mem}'} \quad (C_j)
----------------------------------------------------------------------------------------------------------------
                                             \langle \text{Mem}, c_1 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_6)
```

then, by induction hypothesis, there exists an inference tree for \(\langle \text{Mem}_2, c_2 \rangle \to_{\text{Env}} \text{Mem}'\). There are two subcases.

### Case 1.a: If this tree has been obtained using rule \( C_4 \):

```
          [[e]]^{\text{Mem}_2}_{\text{Env}} = (true, \text{Mem}_3) \quad (C_k) \quad \frac{2}{\langle \text{Mem}_3, c'_2 \rangle \to_{\text{Env}} \text{Mem}'}
-----------------------------------------------------------------------------------------------
                                      \langle \text{Mem}_2, c_2 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_4)
```

then we construct the tree \(\nabla_3\):

```
          \frac{1}{\langle \text{Mem}_1, c \rangle \to_{\text{Env}} \text{Mem}_2} \quad [[\text{not } e]]^{\text{Mem}_2}_{\text{Env}} = (false, \text{Mem}_3) \quad \frac{2}{\langle \text{Mem}_3, c'_2 \rangle \to_{\text{Env}} \text{Mem}'}
-------------------------------------------------------------------------------------------------------------------
                                    \langle \text{Mem}_1, c'_2 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_9) \quad (C_i) \quad (C_k)
```

We get the conclusion, since we can construct the tree:

```
          [[e]]^{\text{Mem}}_{\text{Env}} = (true, \text{Mem}_1) \quad (C_9) \quad \frac{3}{\langle \text{Mem}_1, c'_2 \rangle \to_{\text{Env}} \text{Mem}'}
-----------------------------------------------------------------------------------
                                  \langle \text{Mem}, c_2 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_4)
```

### Case 1.b: If this tree has been obtained using rule \( C_5 \):

```
          [[e]]^{\text{Mem}_2}_{\text{Env}} = (false, \text{Mem}') \quad (C_2) \quad \langle \text{Mem}', \text{skip} \rangle \to_{\text{Env}} \text{Mem}'
-----------------------------------------------------------------------------------
                                    \langle \text{Mem}_2, c_2 \rangle \to_{\text{Env}} \text{Mem}' \quad (C_5)
```
