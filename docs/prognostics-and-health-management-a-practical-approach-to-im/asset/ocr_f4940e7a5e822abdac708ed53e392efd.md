![](bbox=[0.178, 0.106, 0.694, 0.359])

**Figure 1.36** Shape of the objective function.

and the expected cycle length is given as $TR(T)+\int_0^T tf(t)\,dt$, which can be rewritten as $\int_0^T R(t)\,dt$. If identical cycles repeat indefinitely, then the expected cost per unit time can be obtained based on the renewal theory as

$$\frac{c_pR(T)+c_fF(T)}{\int_0^T R(t)\,dt}$$  (1.123)

which is minimized (Barlow and Proschan 1965; Aven and Jensen 1999). In many cases – for example, in fast-changing technologies – the optimum must be obtained based on the single-cycle criteria. The cost per unit time is given as

$$\begin{cases}\dfrac{c_f}{X}, & \text{if } X\le T\\[4pt]\dfrac{c_p}{T}, & \text{if } X\ge T\end{cases}$$

with expected value

$$\frac{c_p}{T}R(T)+\int_0^T\frac{c_f}{t}f(t)\,dt$$  (1.124)

which is then minimized. In the case of several distribution types, the optimal solution can be obtained easily.

Consider first the model in Eq. (1.123). Its derivative has the same sign as

$$(-c_pf(T)+c_ff(T))\int_0^T R(t)\,dt-(c_pR(T)+c_fF(T))R(T)$$

or

$$(c_f-c_p)\left\{\int_0^T R(t)\,dt\,p(T)+R(T)\right\}-c_f$$  (1.125)