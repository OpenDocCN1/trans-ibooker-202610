# 4.5.2. Program termination

The existence of an inference tree describing the execution of a program guarantees that this execution is finite. As all inference trees are finite, showing that a program $c$ terminates from a starting state $(\text{Env}, \text{Mem})$ using big-step operational semantics comes down to proving the existence of a memory state $\text{Mem}'$ such that $\langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}'$.

EXAMPLE 4.7.- Returning to the program fragment used in example 4.1:

while not(↓x = ↓y) do if ↓x ≤↓y then y := ↓y - ↓x else x := ↓x - ↓y

let us show that the execution of this program terminates when starting from any memory state storing strictly positive integer values at locations $x$ and $y$. Formally, we must prove that:

\(\forall \text{Env} \in \mathbf{E} \forall \text{Mem} \in \mathbf{M}\)
\((\text{Mem}(\text{Env}(x)) \ge 1 \text{ and } \text{Mem}(\text{Env}(y)) \ge 1) \Rightarrow \exists \text{Mem}' \in \mathbf{M} \langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}'\)

To prove this property, given an environment $\text{Env} \in \mathbf{E}$, we define the set:

\(M_{\text{Env}} = \{ \text{Mem} \mid \text{Mem}(\text{Env}(x)) \ge 1 \text{ and } \text{Mem}(\text{Env}(y)) \ge 1 \} \subseteq \mathbf{M}\)

and we must prove that:

\(\forall \text{Mem} \in M_{\text{Env}} \exists \text{Mem}' \in \mathbf{M} \langle \text{Mem}, c \rangle \rightarrow_{\text{Env}} \text{Mem}'\)

The proof is obtained by well-founded induction. First, we define a relation $\preceq_{\text{Env}}$ over the elements of $M_{\text{Env}}$. Then we show that this relation is a well-founded order relation, that is, there is no strictly decreasing infinite sequence of values according to $\preceq_{\text{Env}}$. Finally, we prove that for any element $\text{Mem} \in M_{\text{Env}}$, under the hypothesis $P_{\text{Env}}(\text{Mem}')$ for all $\text{Mem}' \prec_{\text{Env}} \text{Mem}$, we can get $P_{\text{Env}}(\text{Mem})$.

Since at least one value associated with the two variables $x$ and $y$ decreases for each execution of the loop body, the relation $\preceq_{\text{Env}}$ over $M_{\text{Env}}$ can be defined as follows:

\(\text{Mem}_1 \preceq_{\text{Env}} \text{Mem}_2 \Leftrightarrow \left( \begin{array}{l} \text{Mem}_1(\text{Env}(x)) \le \text{Mem}_2(\text{Env}(x)) \\ \text{and } \text{Mem}_1(\text{Env}(y)) \le \text{Mem}_2(\text{Env}(y)) \\ \text{and } \forall z \in \mathbf{X} \setminus \{x, y\} \text{ Mem}_1(\text{Env}(z)) = \text{Mem}_2(\text{Env}(z)) \end{array} \right)\)