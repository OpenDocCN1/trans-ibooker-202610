The following table summarizes the definitions of the operators in interval algebra:

| Name | Notation | Definition |
|---|---|---|
| Union | $(i_1 \text{ union } i_2)$ | $(Min(b_1, b_2) : Max(e_1, e_2))$, when $(i_1 \text{ merges } i_2)$; NULL otherwise |
| Intersect | $(i_1 \text{ intersect } i_2)$ | $(Max(b_1, b_2) : Min(e_1, e_2))$, when $(i_1 \text{ overlaps } i_2)$; NULL otherwise |
| Minus | $(i_1 \text{ minus } i_2)$ | $(b_1 : Min(b_2 - 1, e_1))$, when $(b_1 < b_2)$ AND $(e_1 \le e_2)$; $(Max(e_2 + 1, b_1) : e_1)$, when $(b_1 \ge b_2)$ AND $(e_1 > e_2)$; NULL otherwise |

The following figure shows the interval algebra operators graphically:

![](bbox=[0.32, 0.38, 0.78, 0.8])