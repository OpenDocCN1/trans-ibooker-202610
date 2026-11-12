and $\nabla_2$ is the tree:

```
\((C_1) \frac{\text{Env}(z) = r_z \llbracket \downarrow z * \downarrow x \rrbracket^{Mem_2}_{Env} = (6, Mem_2)}{\langle Mem_2, c_1 \rangle \to_{Env} Mem_2} \quad (C_1) \frac{\text{Env}(x) = r_x \llbracket \downarrow x - 1 \rrbracket^{Mem_2}_{Env} = (0, Mem_2)}{\langle Mem_2, c_2 \rangle \to_{Env} Mem_2 [r_x := 0]}\)
\((C_3) \frac{}{\langle Mem_2, c_1 ; c_2 \rangle \to_{Env} Mem_3}\)
```

3) Let \(c_1\) be the instruction repeat \(c\) until \(e\) and \(c_2\) the instruction \(c\) ; if \(e\) then skip else \(c_1\). An inference tree for \(\langle Mem, c_1 \rangle \to_{Env} Mem'\) can be constructed from the inference tree for \(\langle Mem, c_2 \rangle \to_{Env} Mem'\) and vice versa. It suffices to consider the possible rules, which may be used to construct these trees and to examine the two possible values of \(e\). In both cases, we simply “combine” the inference trees as follows.

Case 1: When \(\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')\), starting from the tree:

```
\((C_i) \frac{\nabla}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad (C_8) \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')}{\langle Mem, c_1 \rangle \to_{Env} Mem'}\)
```

we can obtain the tree:

```
\((C_i) \frac{\nabla}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (true, Mem')}{(C_4)} \quad (C_2) \frac{\langle mem', \text{skip} \rangle \to_{Env} Mem'}{\langle Mem_1, \text{if e then skip else } c_1 \rangle \to_{Env} Mem'}\)
\((C_3) \frac{}{\langle mem, c_2 \rangle \to_{Env} Mem'}\)
```

and vice versa.

Case 2: When \(\llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2)\), starting from the tree:

```
\((C_i) \frac{\nabla 1}{\langle Mem, c \rangle \to_{Env} Mem_1} = \llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2) \quad (C_j) \frac{\nabla 2}{\langle Mem_2, c_1 \rangle \to_{Env} Mem'}\)
\((C_9) \frac{}{\langle Mem, c_1 \rangle \to_{Env} Mem'}\)
```

we can obtain the tree:

```
\((C_i) \frac{\nabla 1}{\langle Mem, c \rangle \to_{Env} Mem_1} \quad \frac{\llbracket e \rrbracket^{Mem_1}_{Env} = (false, Mem_2)}{(C_5)} \quad (C_j) \frac{\nabla 2}{\langle Mem_2, c_1 \rangle \to_{Env} Mem'}\)
\((C_3) \frac{\langle Mem_1, \text{if then skip else } c_1 \rangle \to_{Env} Mem'}{\langle mem, c_2 \rangle \to_{Env} Mem'}\)
```

and vice versa.