# EXAMPLE 6.56.-

```
Coq Welcome to Coq 8.8.2 (November 2018)

Coq < Definition f (x : nat) := x.
f is defined

Coq < Definition g (y : nat) := if true then y else 0.
g is defined

Coq < Goal f = g.
1 subgoal

================================
f = g

Unnamed_thm < compute.
1 subgoal

================================
(fun x : nat => x) = (fun y : nat => y)

Unnamed_thm < auto.
No more subgoals.

Unnamed_thm < Qed.
Unnamed_thm is defined
```

We begin by defining two functions f and g and precise our goal which is to prove their equality (using the Goal command). We use a tactic that is native to Coq to compute and rewrite the terms found in the goal to prove (using the compute command). Coq is then able to eliminate the test (false) found in g, producing two function bodies that are identical, apart from the names of the bound variables. Finally, a conclusion of equality is reached automatically (auto command) despite this difference in the variable names.

Of course, this power comes both from the fact that the developer guides Coq through the proof process and from the fact that the functions being compared are relatively simple. The automatic process only works for relatively simple functions. In more complex cases, the responsibility for proof falls to the user, and may become arbitrarily complicated (if not impossible) depending on the way in which functions are defined. For example, the equality of the functions f x = x + x and g y = 2 * y cannot be verified automatically, and the proof can be done, but requires significant work.