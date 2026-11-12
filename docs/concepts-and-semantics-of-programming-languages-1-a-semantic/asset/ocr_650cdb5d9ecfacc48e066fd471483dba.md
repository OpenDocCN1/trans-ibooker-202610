and so:

```
(Env_0, Mem_0) \xrightarrow{x:=++x}_{\text{Lang}_1} (Env_0, Mem'_0[Env_0(x) := 3 + Mem_0(Env_0(x))])
```

We therefore obtain etat_1 = [((x, r_x)], Mem'_0[r_x := 3+2]) = [((x, r_x)], [(r_x, 5)]].

6) Let (Env_0, Mem_0) be an arbitrary state. If x \notin \mathbf{X}^{ref}_{Env_0}, or if the value stored in Mem_0 for x is not an integer, the three assignments lead to the same state (Env_0, Mem_0). Otherwise, by definition:

```
[[x++]]^{Mem_0}_{Env_0} = \left( \begin{array}{c} Mem_0(Env_0(x)), \\ (Env_0, Mem_0[Env_0(x) := Mem_0(Env_0(x)) + 1]) \end{array} \right) \text{Mem}_1 \\
[[++x]]^{Mem_0}_{Env_0} = \left( \begin{array}{c} Mem_0(Env_0(x)) + 1, \\ (Env_0, Mem_0[Env_0(x) := Mem_0(Env_0(x)) + 1]) \end{array} \right) \text{Mem}_2 \\
[[1]]^{Mem_0}_{Env_0} = (1, (Env_0, Mem_0))
```

and thus:

```
(Env_0, Mem_0) \xrightarrow{x:=x++}_{\text{Lang}_1} (Env_0, Mem_1[Env_0(x) := Mem_0(Env_0(x))]) \\
\phantom{(Env_0, Mem_0)} \xrightarrow{x:=x++}_{\text{Lang}_1} \underline{= Mem_0} \\
(Env_0, Mem_0) \xrightarrow{x:=++x}_{\text{Lang}_1} (Env_0, Mem_2[Env_0(x) := Mem_0(Env_0(x)) + 1]) \\
\phantom{(Env_0, Mem_0)} \xrightarrow{x:=++x}_{\text{Lang}_1} \underline{= Mem_2 = Mem_0[Env_0(x) := 1 + Mem_0(Env_0(x))]} \\
(Env_0, Mem_0) \xrightarrow{x:=+1}_{\text{Lang}_1} (Env_0, Mem_0[Env_0(x) := 1 + Mem_0(Env_0(x))]) \\
\phantom{(Env_0, Mem_0)} \xrightarrow{x:=+1}_{\text{Lang}_1} \underline{= Mem_2}
```

The two assignments x := + + x and x := + 1 thus result in the same state when they are executed from the same state, but this state is different to that which would be reached by executing the assignment x := x + +, which does not modify the state in which it is executed in any way.

# A.2. Semantics of functional features

# Solution to exercise 3.1

```
[[fun x \mapsto let y = 3 in x + y]]_{Env} = \langle x, \text{let } y = 3 \text{ in } x + y, Env \rangle \\
[[let y = 3 in fun x \mapsto x + y]]_{Env} = \langle x, x + y, (y, 3) \oplus Env \rangle
```