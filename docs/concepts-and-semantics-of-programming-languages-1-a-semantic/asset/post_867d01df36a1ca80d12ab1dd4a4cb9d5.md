
# Solution to exercise 3.2

- 1) We have [[let f = fun y -> e in (f x)]]Env = [[(f x)]]Env1 = [[e]]Env2 and [[(f x)]]Env = [[e]]Env3 where:

```
Env1 = (f, <y, e, Env>) ⊕ Env
Env2 = (y, [x]Env1) ⊕ Env = (y, 4) ⊕ Env
Env3 = (y, [x]Env) ⊕ (x, 3) ⊕ Env0 = (y, 4) ⊕ (x, 3) ⊕ Env0
```

We have Env2(y) = Env3(y) = 4 and Env2(x) = 4 ≠ Env3(x) = 3. In order that [[e]]Env2 = [[e]]Env3, the expression e must not contain any free occurrences of x (since f is not a recursive function, expression e will not contain any free occurrences of f).

- 2) When e = y + x, we have:

```
[[let f = fun x -> x + (f x) in (f x)]]Env
= [[(f x)]]Env1 + [[(f x)]]Env2 = [[x + (f x)]]Env2 + [[(f x)]]Env2
= [[x]]Env2 + [[y + x]]Env3 = [[x]]Env2 + [[y]]Env3 + [[x]]Env3 = 4 + (4 + 3) = 11
```

where:

```
Env1 = (f, <x, x + (f x), Env>) ⊕ Env
Env2 = (x, [x]Env1) ⊕ Env = (x, 4) ⊕ Env
Env3 = (y, [x]Env2) ⊕ (x, 3) ⊕ Env0 = (y, 4) ⊕ (x, 3) ⊕ Env0
```

# Solution to exercise 3.3

- 1) We note:

```
let x = 3 in (let f = e1 in (let g = e2 in (let h = e3 in e4)))
```

Evaluation tree for e in environment Env0:

(F15) Env1 |- e1 ~~ Ff       (F13)     1   2
                              -------------------------------------
                              (f, Ff) + Env1 |- e2' ~~ v
                                          Env2
(F1) Env0 |- 3 ~~ 3   (F13) -------------------------------------
                                          (x, 3) + Env0 |- e1' ~~ v
                                                  Env1
(F13) ------------------------------------------------
                                      Env0 |- e ~~ v

where $F_f = \langle x, \text{let } y = x \text{ in } y, \text{Env}_1 \rangle$ and $\nabla_1$ and $\nabla_2$ are the evaluation trees:

\nabla_1 :
   (F11) Env2 |- x ~~ 3     (F15) (y, 3) (+) Env2 + fun x -> y ~~ Fg
   ---------------------------------------------------
   (F13) ----------------------- Env2 |- e2 ~~ Fg

\nabla_2 :
      (F15) Env3 |- e3 ~~ Fh
   -----------------------------------
   (h, Fh) (+) Env3 |- e4 ~~ v
   -----------------------------------
                   Env4
   (F13) ----------------------- (g, Fg) (+) Env2 |- e3' ~~ v
   -------------------------------------------
                   Env3

where $F_g = \langle x, y, (y, 3) (+) \text{Env}_2 \rangle$ and $F_h = \langle k, \text{fun } x \mapsto (k x), \text{Env}_3 \rangle$.

## 2) Evaluation tree for $e_4 = ((h f) 2)$ in environment $\text{Env}_4$:

      V   V   V           V   V   V
   1   2   3             4   5   6
(F17) Env4 |- (h f) ~~ F   (F1) Env4 |- 2 ~~ 2   (F17) (x, 2) (+) Env5 |- (k x) ~~ 2
   ---------------------------------------------------
                   Env6
(F17) -------------------------------------------------
                   Env4 |- e4 ~~ 2

where:
\nabla_1 : (F11) \text{Env}_4 |- h ~~ Fh         \nabla_2 : (F11) \text{Env}_4 |- f ~~ Ff

\nabla_3 : (F15) \underline{(k, Ff) (+) \text{Env}_3 |- \text{fun } x \mapsto (k x) ~~ F}
             \text{Env}_5

with $F = \langle x, (k x), \text{Env}_5 \rangle$ and:
\nabla_4 : (F11) \text{Env}_6 |- k ~~ Ff         \nabla_5 : (F11) \text{Env}_6 |- x ~~ 2

\nabla_6 : (F13) ...
   (F11) \text{Env}_7 |- x ~~ 2   (F11) (y, 2) (+) \text{Env}_7 |- y ~~ 2
   ---------------------------------------------------
   (x, 2) (+) \text{Env}_1 |- \text{let } y = x \text{ in } y ~~ 2
             \text{Env}_7

## 3) Evaluation tree for $e_4 = ((h g) 2)$ in environment $\text{Env}_4$:

      V   V   V           V   V   V
   1   2   3             4   5   6
(F17) Env4 |- (h g) ~~ F   (F1) Env4 |- 2 ~~ 2   (F17) (x, 2) (+) Env5 |- (k x) ~~ 3
   ---------------------------------------------------
                   Env6
(F17) -------------------------------------------------
                   Env4 |- e4 ~~ 3

where:

\(\nabla_1 : (F_{11})_{\text{Env}_4 \vdash h \rightsquigarrow F_h} \qquad \nabla_2 : (F_{11})_{\text{Env}_4 \vdash g \rightsquigarrow F_g}\)

\(\nabla_3 : (F_{15})_{\frac{(k, F_g) \oplus \text{Env}_3 \vdash \text{fun } x \mapsto (k \, x) \rightsquigarrow F}{\text{Env}_5}}\)

with \(F = \langle x, (k \, x), \text{Env}_5 \rangle\) and:

\(\nabla_4 : (F_{11})_{\text{Env}_6 \vdash k \rightsquigarrow F_g} \qquad \nabla_5 : (F_{11})_{\text{Env}_6 \vdash x \rightsquigarrow 2}\)

\(\nabla_6 : (F_{11})_{\frac{(x, 2) \oplus (y, 3) \oplus \text{Env}_2 \vdash y \rightsquigarrow 3}{\text{Env}_7}}\)

# Solution to exercise 3.4

Let \(e'_1\) be the subexpression let \(f = e_1\) in \(e\).

- 1) Evaluation tree for \(e_0\) in environment \(\text{Env}_0\):

\(\begin{array}{c} \nabla \\ \text{( )} \\ \underline{(f, [e_1, \text{Env}_1]) \oplus \text{Env}_1 \vdash e \rightsquigarrow v} \\ \text{Env}_2 \\ (F'_{13}) \\ \underline{(x, [2, \text{Env}_0]) \oplus \text{Env}_0 \vdash e'_1 \rightsquigarrow v} \\ \text{Env}_1 \\ (F'_{13}) \\ \underline{\text{Env}_0 \vdash e_0 \rightsquigarrow v} \end{array}\)

- 2) Evaluation tree for \(e\) in environment \(\text{Env}_2\):

\(\begin{array}{c} \text{(F}_1 \text{)} \underline{\text{Env}_2 \vdash 3 \rightsquigarrow 3} \qquad \text{(F}_1 \text{)} \underline{\text{Env}_0 \vdash 2 \rightsquigarrow 2} \\ \text{(F}'_{11} \text{)} \underline{\text{Env}_3 \vdash x \rightsquigarrow 3} \qquad \text{(F}'_{11} \text{)} \underline{\text{Env}_4 \vdash x \rightsquigarrow 2} \\ \text{(F}'_{14} \text{)} \underline{\text{Env}_1 \vdash e_1 \rightsquigarrow F_f} \qquad \text{(F}_4 \text{)} \underline{\text{Env}_4 \vdash y \rightsquigarrow 3} \\ \text{(F}'_{11} \text{)} \underline{\text{Env}_3 \vdash f \rightsquigarrow F_f} \\ \qquad \qquad \qquad \qquad \underline{\text{(y, [x, Env}_3 \text{])} \oplus \text{Env}_1 \vdash y + x \rightsquigarrow 5} \\ \qquad \qquad \qquad \qquad \text{Env}_4 \\ \text{(F}'_{16} \text{)} \\ \qquad \qquad \qquad \qquad \underline{\text{(x, [3, Env}_2 \text{])} \oplus \text{Env}_2 \vdash (f \, x) \rightsquigarrow 5} \\ \qquad \qquad \qquad \qquad \text{Env}_3 \\ \text{(F}'_{13} \text{)} \\ \qquad \qquad \qquad \qquad \underline{\text{Env}_2 \vdash e \rightsquigaro 5} \end{array}\)

with \(F_f = \langle y, y + x, \text{Env}_1 \rangle\).

- 3) Let \( e' \) be the expression fun \( y \mapsto y + x \). Evaluation tree for \( e \) in environment \( \text{Env}_2 \):

\( (F_1) \overline{\text{Env}_2 \vdash 3 \leadsto 3} \)
\( (F'_{11}) \overline{\text{Env}_1 \vdash x \leadsto 3} \quad (F_1) \overline{\text{Env}_2 \vdash 3 \leadsto 3} \)
\( (F'_{11}) \overline{\text{Env}_1 \vdash x \leadsto 3} \quad (F'_{11}) \overline{\text{Env}_4 \vdash x \leadsto 3} \)
\( (F'_{14}) \overline{\text{Env}_3 \vdash e' \leadsto \langle y, y+x, \text{Env}_3 \rangle} \quad (F_4) \overline{[y, [x, \text{Env}_3]] \oplus \text{Env}_3 \vdash y+x \leadsto 6} \)
... 
\( (F'_{16}) \overline{\dots \vdash 6} \)
\( (F'_{13}) \overline{\dots \vdash 6} \)

- 4) We note:

rec g z = fun k -> if z = 0 then (k x) else ((g (z - 1)) k)

with annotations \( e_5, e_6, e_4, e_3 \).

Evaluation tree for \( e \) in environment \( \text{Env}_2 \):

Tree involving \( (F'_{16}) \) and \( (F'_{13}) \).
Result: \( \text{Env}_2 \vdash e \leadsto 4 \).

Construction of tree \( \nabla_1 \):

Tree involving \( (F'_{15}), (F'_{11}), (F'_{14}), (F'_{16}) \).
Result: \( \text{Env}_3 \vdash (g(x-1)) \leadsto F_k \).

with:

\( F_g = \langle z, e_3, (g, [e_2, \text{Env}_2]) \oplus \text{Env}_2 \rangle \) (underlined \( \text{Env}_g \))

and:

\( F_k = \langle k, e_4, \text{Env}_4 \rangle \)

## Construction of tree $\nabla_2$:

$(F_1) \overline{\text{Env}_0 \vdash 2 \rightsquigarrow 2}$
$(F'_{11}) \overline{\text{Env}_3 \vdash x \rightsquigarrow 2} (F_1) \overline{\text{Env}_3 \vdash 1 \rightsquigarrow 1}$
$(F_4) \overline{\text{Env}_3 \vdash x - 1 \rightsquigarrow 1}$
$(F'_{11}) \dots (F_1) \overline{\text{Env}_5 \vdash 0 \rightsquigarrow 0}$
$(F_4) \dots \overline{\text{Env}_5 \vdash z = 0 \rightsquigarrow false} \nabla_3$
$(F_8) \overline{(k, [f, \text{Env}_3]) \oplus \text{Env}_4 \vdash e_4 \rightsquigarrow 4} \text{Env}_5$

## Construction of tree $\nabla_3$:

$(F'_{15}) \overline{\text{Env}_2 \vdash e_2 \rightsquigarrow F_g}$
$(F'_{11}) \overline{\text{Env}_5 \vdash g \rightsquigarrow F_g} (F'_{14}) \overline{(z, [z - 1, \text{Env}_5]) \oplus \text{Env}_g \vdash e_3 \rightsquigarrow F'_k}$
$(F'_{16}) \dots \nabla_4$
$(F'_{16}) \dots$
$\text{Env}_5 \vdash e_6 \rightsquigarrow 4$
with $F'_k = \langle k, e_4, \text{Env}_6 \rangle$.

## Construction of tree $\nabla_4$:

$(F'_{14}) \overline{\text{Env}_1 \vdash e_1 \rightsquigarrow \langle y, y + x, \text{Env}_1 \rangle} F_p$
$(F'_{11}) \overline{\text{Env}_3 \vdash f \rightsquigarrow F_p}$
$(F'_{11}) \overline{\text{Env}_5 \vdash k \rightsquigarrow F_p}$
$(F'_{11}) \overline{\text{Env}_7 \vdash k \rightsquigarrow F_p} \nabla_6$
$(F'_{16}) \overline{\text{Env}_7 \vdash (k x) \rightsquigarrow 4} \nabla_5$
$(F_7) \dots$
$(k, [k, \text{Env}_5]) \oplus \text{Env}_6 \vdash e_4 \rightsquigarrow 4 \text{Env}_7$

## Construction of tree $\nabla_5$:

$(F_1) \overline{\text{Env}_0 \vdash 2 \rightsquigarrow 2}$
$(F'_{11}) \overline{\text{Env}_3 \vdash x \rightsquigarrow 2} (F_1) \overline{\text{Env}_3 \vdash 1 \rightsquigarrow 1}$
$(F_4) \overline{\text{Env}_3 \vdash x - 1 \rightsquigarrow 1}$
$(F'_{11}) \dots (F_1) \overline{\text{Env}_5 \vdash 1 \rightsquigarrow 1}$
$(F_4) \dots \overline{\text{Env}_5 \vdash z - 1 \rightsquigarrow 0}$
$(F'_{11}) \dots \overline{\text{Env}_7 \vdash z \rightsquigarrow 0} (F_1) \overline{\text{Env}_7 \vdash 0 \rightsquigarrow 0}$
$(F_4) \dots \overline{\text{Env}_7 \vdash z = 0 \rightsquigarrow true}\)
