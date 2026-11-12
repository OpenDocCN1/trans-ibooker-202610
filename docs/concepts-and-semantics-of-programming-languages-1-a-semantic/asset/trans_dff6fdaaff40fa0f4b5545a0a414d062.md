

## 命题 4.5.- 取两个偏函数 \(f_b : \mathbf{M} \rightarrowtail \mathbf{IB}\) 和 \(f_0 : \mathbf{M} \rightarrowtail \mathbf{M}\)。函数 \(F : f \mapsto \mathbf{cond}(f_b, f, f_0)\) 是连续的。

证明.- 首先，让我们证明函数 \(F\) 是单调的。

设 \(f_1\) 和 \(f_2\) 为 \([\mathbf{M} \rightarrowtail \mathbf{M}]\) 中的两个函数，使得 \(f_1 \sqsubseteq f_2\)，并让我们展示 \(F(f_1) \sqsubseteq F(f_2)\)。假设 \(\mathsf{Mem} \in \mathbf{M}\) 使得 \((F(f_1))(\mathsf{Mem}) \neq \mathsf{undef}\)；我们展示 \((F(f_1))(\mathsf{Mem}) = (F(f_2))(\mathsf{Mem})\)。

如果 \(f_b(\mathsf{Mem}) = 1\)，则：

```
\((F(f_1))(\mathsf{Mem}) = f_1(\mathsf{Mem})\) and \((F(f_2))(\mathsf{Mem}) = f_2(\mathsf{Mem})\)
```

由于 \(f_1 \sqsubseteq f_2\) 且 \(f_1(\mathsf{Mem}) = (F(f_1))(\mathsf{Mem}) \neq \mathsf{undef}\)，我们有 \(f_1(\mathsf{Mem}) = f_2(\mathsf{Mem})\)，因此 \((F(f_1))(\mathsf{Mem}) = (F(f_2))(\mathsf{Mem})\)，从而确立 \(F(f_1) \sqsubseteq F(f_2)\)。如果 \(f_b(\mathsf{Mem}) = 0\)，则 \((F(f_1))(\mathsf{Mem}) = f_0(\mathsf{Mem})\) 且 \((F(f_2))(\mathsf{Mem}) = f_0(\mathsf{Mem})\)，故 \((F(f_1))(\mathsf{Mem}) = (F(f_2))(\mathsf{Mem})\)，这使我们能够确立 \(F(f_1) \sqsubseteq F(f_2)\)。如果 \(f_b(\mathsf{Mem}) = \mathsf{undef}\)，则 \((F(f_1))(\mathsf{Mem}) = \mathsf{undef}\)，这与假设矛盾。

让我们证明，对于 \(([\mathbf{M} \rightarrowtail \mathbf{M}], \sqsubseteq)\) 中的任何非空链 \(Y\)，\(\sqcup F(Y) = F(\sqcup Y)\)。

由于我们知道 \(F\) 是单调的，根据引理 4.5，我们得到 \(\sqcup F(Y) \sqsubseteq F(\sqcup Y)\)。仍需证明 \(F(\sqcup Y) \sqsubseteq \sqcup F(Y)\)，即 \(F(\sqcup Y) \sqsubseteq \sqcup \{F(f) \mid f \in Y\}\)。取 \(\mathsf{Mem}_1 \in \mathbf{M}\)。让我们证明，如果 \((F(\sqcup Y))(\mathsf{Mem}_1) = \mathsf{Mem}_2 \neq \mathsf{undef}\)，则 \((\sqcup F(Y))(\mathsf{Mem}_1) = \mathsf{Mem}_2\)。如果 \(f_b(\mathsf{Mem}_1) = 1\)，则 \((F(\sqcup Y))(\mathsf{Mem}_1) = (\sqcup Y)(\mathsf{Mem}_1) = \mathsf{Mem}_2\)，并且根据命题 4.4，存在 \(f \in Y\) 使得 \(f(\mathsf{Mem}_1) = \mathsf{Mem}_2\)。根据定义，对于所有 \(f \in Y\)，我们有 \(F(f) \sqsubseteq \sqcup F(Y)\)，因此得到 \((\sqcup F(Y))(\mathsf{Mem}_1) = \mathsf{Mem}_2\)。否则，如果 \(f_b(\mathsf{Mem}_1) = 0\)，则 \((F(\sqcup Y))(\mathsf{Mem}_1) = f_0(\mathsf{Mem}_1) = \mathsf{Mem}_2\)，并且对于某个函数 \(f \in Y\)：

```
\((F(f))(\mathsf{Mem}_1) = f_0(\mathsf{Mem}_1) = \mathsf{Mem}_2 \neq \mathsf{undef}\)
```

再次，根据定义，对于所有 \(f \in Y\)，我们有 \(F(f) \sqsubseteq \sqcup F(Y)\)，因此得到 \((\sqcup F(Y))(\mathsf{Mem}_1) = \mathsf{Mem}_2\)。最后，如果 \(f_b(\mathsf{Mem}_1) = \mathsf{undef}\)，则 \((F(\sqcup Y))(\mathsf{Mem}_1) = \mathsf{undef}\)，这与假设矛盾。

## 命题 4.6.- 取 \(f_0 : \mathbf{M} \rightarrowtail \mathbf{M}\)。函数 \(F : f \mapsto f \circ f_0\) 是连续的。

证明.- 首先，让我们证明 \(F\) 是单调的。设 \(f_1\) 和 \(f_2\) 为 \([\mathbf{M} \rightarrowtail \mathbf{M}]\) 中的两个函数，使得 \(f_1 \sqsubseteq f_2\)，并让我们证明 \(F(f_1) \sqsubseteq F(f_2)\)。取 \(\mathsf{Mem} \in \mathbf{M}\) 使得 \((F(f_1))(\mathsf{Mem}) \neq \mathsf{undef}\)：我们可以证明 \((F(f_1))(\mathsf{Mem}) = (F(f_2))(\mathsf{Mem})\)。根据定义，\((F(f_1))(\mathsf{Mem}) = f_1(f_0(\mathsf{Mem})) \neq \mathsf{undef}\)，故 \(f_0(\mathsf{Mem}) \neq \mathsf{undef}\)，并且由于 \(f_1 \sqsubseteq f_2\)，我们得到 \(f_1(f_0(\mathsf{Mem})) = f_2(f_0(\mathsf{Mem}))\)，从而得出结论。现在，我们必须证明 \(F\) 是连续的：

\((F(\sqcup Y))(\mathsf{Mem}_1) = (\sqcup Y)(f_0(\mathsf{Mem}_1)) = \mathsf{Mem}_2\)
\(\Leftrightarrow \exists f \in Y\ f(f_0(\mathsf{Mem}_1)) = \mathsf{Mem}_2\)（参见命题 4.4）
\(\Leftrightarrow \exists f \in Y\ (F(f))(\mathsf{Mem}_1) = \mathsf{Mem}_2\)
\(\Leftrightarrow (\sqcup F(Y))(\mathsf{Mem}_1) = \mathsf{Mem}_2\)（参见命题 4.4）

## 命题 4.7.-- 函数项 \(F : f \mapsto \text{cond}([\![e]\!]_{\text{Env}}, f \circ [\![c]\!]_{\text{Env}}, id)\) 是一个连续函数。

证明.-- 取 \(F_1 : f \mapsto \text{cond}([\![e]\!]_{\text{Env}}, f, id)\) 和 \(F_2 : f \mapsto f \circ [\![c]\!]_{\text{Env}}\)，则 \(F(f) = F_1(F_2(f)) = F_1(f \circ [\![c]\!]_{\text{Env}}) = \text{cond}([\![e]\!]_{\text{Env}}, f \circ [\![c]\!]_{\text{Env}}, id)\)，故 \(F = F_1 \circ F_2\)。根据命题 4.5，函数 \(F_1\) 是连续的，且根据命题 4.6，函数 \(F_2\) 是连续的。引理 4.6 从而得出结论。

## 4.7.1.2.4. ccpo 上连续函数的最小不动点

现在让我们展示如何对函数项 \(F\) 的最小不动点进行增量计算。该构造基于以下定理。

## 定理 4.2.-- 令 \((D, \sqsubseteq)\) 为一个 ccpo，\(f : D \to D\) 为一个连续函数：

\[
\text{lfp}(f) = \sqcup(\{f^n(\bot) \mid n \ge 0\})
\]

证明.-- 首先，让我们证明 \(\sqcup(\{f^n(\bot) \mid n \ge 0\})\) 的存在性。这可以通过对 \(n\) 进行归纳证明来轻松完成：

\[
\forall n \in \mathbb{N}\ \forall m \in \mathbb{N}\ m \ge n \Rightarrow f^n(\bot) \sqsubseteq f^m(\bot)
\]

（因为，根据引理 4.2，\(\bot\) 是 \(D\) 的最小元素，且 \(f\) 是连续的，因此也是单调的）。\(\{f^n(\bot) \mid n \ge 0\}\) 因此是一条链，并且由于 \((D, \sqsubseteq)\) 是一个 ccpo，\(\sqcup(\{f^n(\bot) \mid n \ge 0\})\) 存在。现在，我们可以证明 \(\sqcup(\{f^n(\bot) \mid n \ge 0\})\) 是 \(f\) 的一个不动点：

\[
f(\sqcup(\{f^n(\bot) \mid n \ge 0\})) = \sqcup(f(\{f^n(\bot) \mid n \ge 0\})) \quad \text{因为 } f \text{ 是连续的}
\]
\[
= \sqcup(\{f^n(\bot) \mid n \ge 1\}) = \sqcup(\{f^n(\bot) \mid n \ge 1\} \cup \{\bot\}) = \sqcup(\{f^n(\bot) \mid n \ge 0\})
\]

仍需证明 \(\sqcup(\{f^n(\bot) \mid n \ge 0\})\) 是 \(f\) 的最小不动点。设 \(d \in D\) 为 \(f\) 的一个不动点。根据引理 4.2，\(\bot \sqsubseteq d\)，因此对于任何自然数 \(n\)，我们有 \(f^n(\bot) \sqsubseteq f^n(d)\)（这可以通过对 \(n\) 进行归纳证明，因为 \(f\) 是连续的，因此也是单调的）。此外，我们通过对 \(n\) 进行归纳得到 \(\forall n \in \mathbb{N}\ f^n(d) = d\)，因为 \(d\) 是 \(f\) 的一个不动点。\(d\) 因此是 \(\{f^n(\bot) \mid n \ge 0\}\) 的一个上界，并且由于 \(\sqcup(\{f^n(\bot) \mid n \ge 0\})\) 是 \(\{f^n(\bot) \mid n \ge 0\}\) 的最小上界，我们得到 \(\sqcup(\{f^n(\bot) \mid n \ge 0\}) \sqsubseteq d\)。

## 4.7.1.2.5. 示例

考虑以下程序，它计算 ↓y = ↓x!，其中 ↓x 是一个严格正整数：

```
y := 1; while not(↓x = 1) do (y := ↓y × ↓x; x := ↓x - 1)
```

让我们计算在状态 (Env₀, Mem₀) 中执行此程序的结果，使得 Mem₀(Env₀(x)) = 5 且 Env₀(y) ∈ R。在以下内容中，我们假设 Env(x) 和 Env(y) 分别是 R 中的元素，分别记为 r_x 和 r_y。根据定义，与该程序各个组件相关的函数如下：

```
[[c1]]_Env : Mem ↦ Mem[ry := 1]

[[e]]_Env : Mem ↦ {
    1 if Mem(r_x) ≠ 1
    0 if Mem(r_x) = 1
}

[[c3]]_Env = [[c5]]_Env ∘ [[c4]]_Env
[[c3]]_Env : Mem ↦ [[c5]]_Env(Mem[ry := Mem(ry) × Mem(rx)])
           = Mem[ry := Mem(ry) × Mem(rx)][rx := Mem(rx) - 1]

[[c2]]_Env = lfp(F) where F : f ↦ cond([[e]]_Env, f ∘ [[c3]]_Env, id)
F(f) : Mem ↦ {
    f([[c3]]_Env(Mem)) if Mem(r_x) ≠ 1
    Mem              if Mem(r_x) = 1
}
```

函数 [[c3]]_Env 描述了执行循环体后获得的状态。当循环体执行两次时，函数 [[c3]]²_Env = [[c3]; c3]]_Env = [[c3]]_Env ∘ [[c3]]_Env 定义为：

```
Mem ↦ (
    Mem [
        ry := Mem(ry) × Mem(rx)
        rx := Mem(rx) - 1
        ry := Mem(ry) × Mem(rx) × (Mem(rx) - 1)
        rx := Mem(rx) - 1 - 1
    ]
)
= (
    Mem [
        ry := Mem(ry) × Mem(rx) × (Mem(rx) - 1)
        rx := Mem(rx) - 2
    ]
)
```

更一般地，当循环体执行 \(n\) 次时，函数 \(\llbracket c_3 \rrbracket^n_{\text{Env}}\) 定义为：

```
\(\text{Mem} \mapsto \left( \text{Mem} \begin{bmatrix} r_y := \text{Mem}(r_y) \times \text{Mem}(r_x) \times (\text{Mem}(r_x) - 1) \\ \vdots \\ r_x := \text{Mem}(r_x) - n \end{bmatrix} \right)\)
```

因此，在满足 \(\text{Mem}(r_x) = p\) 的状态中，我们有：

```
\(\llbracket c_3 \rrbracket^{p-1}_{\text{Env}}(\text{Mem}) = \left( \text{Mem} \begin{bmatrix} r_y := \text{Mem}(r_y) \times p \times (p - 1) \times \dots \times 2 \\ r_x := \text{Mem}(r_x) - 1 \end{bmatrix} \right)\)
\(= \text{Mem}[r_y := \text{Mem}(r_y) \times p!][r_x := \text{Mem}(r_x) - 1]\)
```

使用定理 4.2，我们可以从 \(F^0(\perp) = \perp : \text{Mem} \mapsto \text{undef}\) 计算 \(\text{lfp}(F) = \sqcup \{ F^n(\perp) \mid n \ge 0 \}\)。根据定义，我们有：

```
\(F^1(\perp) = F(\perp) : \text{Mem} \mapsto \begin{cases} \text{undef} & \text{if } \text{Mem}(r_x) \neq 1 \\ \text{Mem} & \text{if } \text{Mem}(r_x) = 1 \end{cases}\)
```

同样：

```
\(F^2(\perp) = F(F^1(\perp)) : \text{Mem} \mapsto \begin{cases} F^1(\perp)(\llbracket c_3 \rrbracket_{\text{Env}}(\text{Mem})) & \text{if } \text{Mem}(r_x) \neq 1 \\ \text{Mem} & \text{if } \text{Mem}(r_x) = 1 \end{cases}\)
```

并且由于 \(\llbracket c_3 \rrbracket_{\text{Env}}(\text{Mem})(r_x) = \text{Mem}(r_x) - 1\)，我们有：

```
\(F^2(\perp) : \text{Mem} \mapsto \begin{cases} \text{undef} & \text{if } \text{Mem}(r_x) \neq 1 \text{ and } \text{Mem}(r_x) \neq 2 \\ \llbracket c_3 \rrbracket_{\text{Env}}(\text{Mem}) & \text{if } \text{Mem}(r_x) = 2 \\ \text{Mem} & \text{if } \text{Mem}(r_x) = 1 \end{cases}\)
```

经过进一步迭代，我们得到：

```
\(F^3(\perp) = F(F^2(\perp)) : \text{Mem} \mapsto \begin{cases} F^2(\perp)(\llbracket c_3 \rrbracket_{\text{Env}}(\text{Mem})) & \text{if } \text{Mem}(r_x) \neq 1 \\ \text{Mem} & \text{if } \text{Mem}(r_x) = 1 \end{cases}\)
```

因此：

```
\(F^3(\perp) : \text{Mem} \mapsto \begin{cases} \text{undef} & \text{if } \text{Mem}(r_x) \neq 1 \text{ and } \text{Mem}(r_x) \neq 2 \text{ and } \text{Mem}(r_x) \neq 3 \\ \llbracket c_3 \rrbracket^2_{\text{Env}}(\text{Mem}) & \text{if } \text{Mem}(r_x) = 3 \\ \llbracket c_3 \rrbracket_{\text{Env}}(\text{Mem}) & \text{if } \text{Mem}(r_x) = 2 \\ \text{Mem} & \text{if } \text{Mem}(r_x) = 1 \end{cases}\)
```

通过归纳，我们得到：

```
\(F^n(\perp) : \text{Mem} \mapsto \begin{cases} \text{undef} & \text{if } \text{Mem}(r_x) \neq 1 \text{ and } \text{Mem}(r_x) \neq 2 \text{ and } \dots \text{ and } \text{Mem}(r_x) \neq n \\ \llbracket c_3 \rrbracket^{n-1}_{\text{Env}}(\text{Mem}) & \text{if } \text{Mem}(r_x) = n \\ \llbracket c_3 \rrbracket^{n-2}_{\text{Env}}(\text{Mem}) & \text{if } \text{Mem}(r_x) = n - 1 \\ \dots \\ \llbracket c_3 \rrbracket_{\text{Env}}(\text{Mem}) & \text{if } \text{Mem}(r_x) = 2 \\ \llbracket c_3 \rrbracket^0_{\text{Env}}(\text{Mem}) & \text{if } \text{Mem}(r_x) = 1 \end{cases}\)
```

