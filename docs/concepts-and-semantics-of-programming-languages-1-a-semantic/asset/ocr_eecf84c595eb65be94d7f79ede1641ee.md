When \(v_x < v_y\), the quotient of the division of \(v_x\) by \(v_y\) is:

\([[c_d]]_{Env}(Mem)(a) = 0,\)

while the remainder of this division is

\([[c_d]]_{Env}(Mem)(b) = v_x.\)

When \(v_x \ge v_y\), we have:

\(v_x = (v_y \times [[c_d]]_{Env}(Mem)(r_a)) + [[c_d]]_{Env}(Mem)(r_b)\)

with \([[c_d]]_{Env}(Mem)(r_b)) < v_y\) which, again, concludes the proof.

2) Let us prove the total correctness of the following Hoare triple:

\(\{ \underline{x \ge 0 \text{ and } y > 0} \}_{P_1} c_d \{ \underline{x = (\downarrow a \times y) + \downarrow b \text{ and } \downarrow b < y} \}_{P_2}\)

First, we use rules (Seq) and (ConsD):

\(\nabla_2\)
\((W_T)\)
\(\downarrow a\)
\(\{x = (\downarrow a \times y) + \downarrow b\}\)
\(\{P_1\} c_w \{ \text{and } \downarrow b \ge 0 \text{ and } y > 0 \text{ and not } \downarrow b \ge y \}\)
\((Cons_G) \downarrow 1\)
\(\{P_1\} c_1 ; c_2 (P_1) (Cons_D)\)
\(\---------------------------------------------------\)
\((Seq)\)
\(\{P_1\} c_d {P_2}\)

The application of rule (ConsD) is based on the following implication:

\((x = (\downarrow a \times y) + \downarrow b \text{ and } \downarrow b \ge 0 \text{ and } y > 0 \text{ and not } \downarrow b \ge y) \Rightarrow P_2\)

The loop invariant \(P_I\) used here is:

\(x = (\downarrow a \times y) + \downarrow b \text{ and } \downarrow b \ge 0 \text{ and } y > 0\)

and the variant is \(V = \downarrow b\). Since \(P_I \Rightarrow \downarrow b \ge 0\), the rule \((W_T)\) can be applied. The inference tree \(\nabla_2\) is constructed as follows:

\((Aff)\)
\(\{P'_I \text{ and } \downarrow b - y < n\} \downarrow c_3 \{P'_I \text{ and } \downarrow b < n\}\)
\((Cons_G)\)
\(\{P_I \text{ and } \downarrow b \ge y \text{ and } \downarrow b = n\} \downarrow c_3 \{P'_I \text{ and } \downarrow b < n\}\)
\((Aff)\)
\(\{P'_I \text{ and } \downarrow b < n\} \downarrow c_4 \{P_I \text{ and } \downarrow b < n\}\)
\((Seq)\)
\(\{P_I \text{ and } \downarrow b \ge y \text{ and } \downarrow b = n\} c_3 ; c_4 \{P_I \text{ and } \downarrow b < n\}\)