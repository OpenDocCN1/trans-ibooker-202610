Claim 2. $d \circ h = h \circ d.$
We have
\[
\begin{aligned}
d\left(\int_{0}^{1} t^{s-1} f(tr^{1},\dots,tr^{m})\,dt\right) \\
&= \sum_{j} \frac{\partial}{\partial r^{j}}\left(\int_{0}^{1} t^{s-1} f(tr^{1},\dots,tr^{m})\,dt\right) d(r^{j}) \quad [\text{Th 15.4.7}] \\
&= \sum_{j}\left(\int_{0}^{1} \frac{\partial}{\partial r^{j}}\left(t^{s-1} f(tr^{1},\dots,tr^{m})\right) dt\right) d(r^{j}) \quad [\text{Th 10.5.5}] \\
&= \sum_{j}\left(\int_{0}^{1} t^{s} \frac{\partial f}{\partial r^{j}}(tr^{1},\dots,tr^{m})\,dt\right) d(r^{j}),
\end{aligned}
\]
hence
\[
\begin{aligned}
h\left(d\left(f(r^{1},\dots,r^{m}) d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}})\right)\right) \\
&= h\left(d(f)^{(r^{1},\dots,r^{m})} \wedge d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}})\right) \\
&= h\left(\left[\sum_{j} \frac{\partial f}{\partial r^{j}}(r^{1},\dots,r^{m}) d(r^{j})\right] \wedge d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}})\right) \\
&= \sum_{j} h\left(\frac{\partial f}{\partial r^{j}}(r^{1},\dots,r^{m}) d(r^{j}) \wedge d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}})\right) \\
&= \sum_{j}\left[\left(\int_{0}^{1} t^{s} \frac{\partial f}{\partial r^{j}}(tr^{1},\dots,tr^{m}) dt\right) d(r^{j}) \wedge d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}})\right] \\
&= \left[\sum_{j} \frac{\partial}{\partial r^{j}}\left(\int_{0}^{1} t^{s-1} f(tr^{1},\dots,tr^{m}) dt\right) d(r^{j})\right] \wedge d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}}) \\
&= d\left(\int_{0}^{1} t^{s-1} f(tr^{1},\dots,tr^{m}) dt\right) \wedge d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}}) \\
&= d\left(\left[\int_{0}^{1} t^{s-1} f(tr^{1},\dots,tr^{m}) dt\right] d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}})\right) \\
&= d\left(h\left(f(r^{1},\dots,r^{m}) d(r^{i_{1}}) \wedge \cdots \wedge d(r^{i_{s}})\right)\right),
\end{aligned}
\]
where the first and seventh equalities follow from Theorem 16.1.2, the second and sixth equalities from Theorem 15.4.7, and the fifth equality from Theorem 10.5.5. This proves the claim.
Then
\[
\begin{aligned}
\omega &= h \circ \mathcal{L}_{R}(\omega) & [\text{Claim 1}] \\
&= (h \circ d \circ i_{R} + h \circ i_{R} \circ d)(\omega) & [\text{Th 16.5.3}] \\
&= h \circ d \circ i_{R}(\omega) & [\omega \text{ is closed}] \\
&= d \circ h \circ i_{R}(\omega) & [\text{Claim 2}] \\
&= d\left(h \circ i_{R}(\omega)\right).
\end{aligned}
\]