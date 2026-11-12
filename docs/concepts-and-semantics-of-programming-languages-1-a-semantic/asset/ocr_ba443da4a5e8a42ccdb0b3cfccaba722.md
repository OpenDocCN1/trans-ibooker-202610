and the following inference tree can be constructed:

```
⟦e⟧_Env^Mem1 = (false, Mem2)   (C2)   ⟨Mem2, skip⟩ →Env Mem2  
                                    ⟨Mem1, c2⟩ →Env Mem2       [A.8]  
(C5) ───────────────────────────────────────────────────
```

(⇐) If ⟨Mem1, c2⟩ →Env Mem2 then two cases are possible. Either the last applied rule is C4, and the tree [A.5] can be constructed from the tree [A.6], or the last applied rule is C5, and the tree [A.7] can be constructed from the tree [A.8]. Both cases conclude the proof.

# Solution to exercise 4.2

Throughout this exercise, we assume that for any expression e, ⟦e⟧_Env^Mem1 = (true, Mem2) (respectively, ⟦e⟧_Env^Mem1 = (false, Mem2)) if and only if ⟦not e⟧_Env^Mem1 = (false, Mem2) (respectively, ⟦not e⟧_Env^Mem1 = (true, Mem2)).

+   1) The semantics of the repeat until loop can be defined as follows:

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

+   2) Execution tree for the program c:

```
repeat (z := z * ↓x; x := ↓x - 1) until ↓x = 0  
       c1      c2      e
```

```
∀1 ⟦e⟧_Env^Mem2 = (false, Mem2)   (C8)   ...  
(C9) ───────────────────────────────────────────  
      ⟨Mem, repeat c1 ; c2 until e⟩ →Env Mem3
```

where ∀1 is the tree:

```
Env(z) = rz    Env(x) = rx  
⟦z * ↓x⟧_Env^Mem = (6, Mem)   ⟦↓x - 1⟧_Env^Mem1 = (1, Mem1)  
(C1) ⟨Mem, c1⟩ →Env Mem [rz := 6]   (C1) ⟨Mem1, c2⟩ →Env Mem1 [rx := 1]  
(C3) ─────────────────────────────────────────────────────  
           ⟨Mem, c1 ; c2⟩ →Env Mem2
```