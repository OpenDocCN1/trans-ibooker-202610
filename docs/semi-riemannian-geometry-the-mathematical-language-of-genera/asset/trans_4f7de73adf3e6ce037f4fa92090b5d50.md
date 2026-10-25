

## 8.2 向量空间的方向

**在整个章节中，** \(\mathbb{R}^m\) 假定具有标准方向。

我们现在用多向量给出方向的一个等价方法。设 \(V\) 是一个维数为 \(m\) 的向量空间。在本上下文中，\(\Lambda^m(V)\) 中的一个非零多向量称为 \(V\) 上的 **方向多向量**。例如，设 \((h_1,\ldots,h_m)\) 是 \(V\) 的一个基，\((\theta^1,\ldots,\theta^m)\) 是其对偶基。由定理 7.2.9，\(\theta^1\wedge\cdots\wedge\theta^m(h_1,\ldots,h_m)=1\)，所以 \(\theta^1\wedge\cdots\wedge\theta^m\) 是 \(V\) 上的一个方向多向量。更一般地，我们有以下结果。

### 定理 8.2.1.

设 \(V\) 是一个维数为 \(m\) 的向量空间，\(\varpi\) 是 \(\Lambda^m(V)\) 中的一个多向量。那么 \(\varpi\) 是方向多向量当且仅当 \(\varpi(h_1,\ldots,h_m)\neq 0\) 对某个（从而对每一个）基 \((h_1,\ldots,h_m)\) 成立。

**证明.** 设 \(\mathcal{H}=(h_1,\ldots,h_m)\) 是 \(V\) 的一个基，\((\theta^1,\ldots,\theta^m)\) 是其对偶基。我们由定理 7.2.13(b) 得到

\[
\varpi=\varpi(h_1,\ldots,h_m)\,\theta^1\wedge\cdots\wedge\theta^m.
\]

因此，\(\varpi\) 非零当且仅当 \(\varpi(h_1,\ldots,h_m)\neq 0\)。设 \(\mathcal{F}=(f_1,\ldots,f_m)\) 是 \(V\) 的另一个基。由定理 7.3.3(b)，

\[
\varpi(h_1,\ldots,h_m)=\det\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)\varpi(f_1,\ldots,f_m),
\]

并且由定理 2.4.12，\(\det([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}})\neq 0\)。因此，\(\varpi(h_1,\ldots,h_m)\neq 0\) 当且仅当 \(\varpi(f_1,\ldots,f_m)\neq 0\)。结果得证。

### 定理 8.2.2.

设 \(V\) 是一个向量空间，\(\mathcal{H}=(h_1,\ldots,h_m)\) 和 \(\mathcal{F}=(f_1,\ldots,f_m)\) 是 \(V\) 的基，\((\theta^1,\ldots,\theta^m)\) 是对应于 \(\mathcal{H}\) 的对偶基。那么以下等价：

- (a) \(\mathcal{H}\) 和 \(\mathcal{F}\) 是相容的。
- (b) \(\theta^1\wedge\cdots\wedge\theta^m(f_1,\ldots,f_m)>0\)。
- (c) 对某个（从而对每一个）\(V\) 上的方向多向量 \(\varpi\)，\(\varpi(h_1,\ldots,h_m)\) 与 \(\varpi(f_1,\ldots,f_m)\) 具有相同的符号。

**注.** 由定理 8.2.1，我们有 \(\varpi(h_1,\ldots,h_m),\varpi(f_1,\ldots,f_m)\neq 0\)，所以部分 (c) 中的断言是有意义的。

**证明.** (a) \(\Longleftrightarrow\) (b)：由定理 7.2.9 和定理 7.3.3(b)，

\[
\det\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)\theta^1\wedge\cdots\wedge\theta^m(f_1,\ldots,f_m)=\theta^1\wedge\cdots\wedge\theta^m(h_1,\ldots,h_m)=1.
\]

结果得证。

(a) \(\Longleftrightarrow\) (c)：由定理 7.3.3(b)，

\[
\varpi(h_1,\ldots,h_m)=\det\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)\varpi(f_1,\ldots,f_m),
\]

并且由定理 2.4.12，\(\det([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}})\neq 0\)。结果得证。

### 定理 8.2.3.

设 \(V\) 是一个向量空间，\(\varpi\) 是 \(V\) 上的一个方向多向量，\(\mathfrak{D}\) 是满足 \(\varpi(h_1,\ldots,h_m)>0\) 的基 \((h_1,\ldots,h_m)\) 的集合。那么 \(\mathfrak{D}\) 是 \(V\) 的一个方向，称为由 \(\varpi\) **诱导的方向**。

**证明.** 设 \(\mathcal{H}=(h_1,\ldots,h_m)\) 和 \(\mathcal{F}=(f_1,\ldots,f_m)\) 是 \(V\) 的基，且 \(\varpi(h_1,\ldots,h_m)>0\)。那么

- \(\mathcal{F}\) 在 \(\mathfrak{D}\) 中
- \(\Leftrightarrow \varpi(f_1,\ldots,f_m)>0\)
- \(\Leftrightarrow \varpi(h_1,\ldots,h_m)\) 与 \(\varpi(f_1,\ldots,f_m)\) 具有相同的符号
- \(\Leftrightarrow \mathcal{H}\) 与 \(\mathcal{F}\) 相容，

其中最后的等价由定理 8.2.2 得到。因此，\(\mathfrak{D}\) 是与 \(\mathcal{H}\) 相容的 \(V\) 的基的集合；即 \(\mathfrak{D}=[\mathcal{H}]\)。

### 例 8.2.4 (\(\mathbb{R}^m\))。

设 \(\mathcal{E}=(e_1,\ldots,e_m)\) 是 \(\mathbb{R}^m\) 的标准基，\((\xi^1,\ldots,\xi^m)\) 是其对偶基，\(\Omega=\xi^1\wedge\cdots\wedge\xi^m\)。由定理 7.2.9 可知 \(\Omega(e_1,\ldots,e_m)=1\)，所以 \(\Omega\) 是 \(\mathbb{R}^m\) 上的一个方向多向量，且 \(\mathcal{E}\) 在由 \(\Omega\) 诱导的 \(\mathbb{R}^m\) 的方向中。因此，\(\Omega\) 诱导出 \(\mathbb{R}^m\) 的标准方向。

设 \(V\) 是一个向量空间，\(\varpi\) 和 \(\vartheta\) 是 \(V\) 上的方向多向量。由定理 7.2.12(d) 和定理 8.2.1，\(\varpi\) 张成 \(\Lambda^m(V)\)（\(\vartheta\) 亦然）。于是 \(\varpi=c\vartheta\) 对某个非零实数 \(c\) 成立。我们称 \(\varpi\) 与 \(\vartheta\) 相容，并记作 \(\varpi\sim\vartheta\)，如果 \(c>0\)。容易证明 \(\sim\) 是 \(V\) 上方向多向量集合上的一个等价关系，并且恰好有两个等价类。包含 \(\varpi\) 的等价类记作 \([\varpi]\)。根据定义，\([\varpi]\) 包含所有与 \(\varpi\) 相容的 \(V\) 上的多向量。显然，\(\varpi\) 与 \(-\varpi\) 不相容。因此，对任意 \(V\) 上的方向多向量 \(\varpi\)，方向多向量的等价类为 \([\varpi]\) 和 \([-\varpi]\)。

设 \(\mathcal{H}\) 是 \(V\) 的一个基。显然，同一等价类中的方向多向量诱导出相同的方向，而不同等价类中的方向多向量诱导出相反的方向。因此我们有一个双射

\[
\iota : \{[\varpi], [-\varpi]\} \longrightarrow \{[\mathcal{H}], [-\mathcal{H}]\}
\]

它把 \([\varpi]\) 和 \([-\varpi]\) 分别赋给由 \(\varpi\) 和 \(-\varpi\) 诱导的方向。这表明我们可以用 \(V\) 的基（的等价类）或 \(V\) 上的方向多向量（的等价类）来指定方向。对于计算而言，后一种方法通常更方便。

### 定理 8.2.5（子空间的诱导方向）。

设 \((V,\mathfrak{D})\) 是一个维数 \(m\ge 2\) 的定向向量空间，\(U\) 是 \(V\) 的一个 \((m-1)\) 维子空间，\(v\) 是 \(V\setminus U\) 中的一个向量。那么：

- (a) 存在唯一的 \(U\) 的方向 \(\mathcal{D}_U\)，称为由 \(v\) **诱导的方向**，使得 \((h_1,\ldots,h_{m-1})\) 是相对于 \(\mathcal{D}_U\) 正向的 \(U\) 的基当且仅当 \((v,h_1,\ldots,h_{m-1})\) 是相对于 \(\mathcal{D}\) 正向的 \(V\) 的基。
- (b) 如果 \(\varpi\) 是诱导 \(\mathcal{D}\) 的 \(V\) 上的方向多向量，则 \(\iota_v(\varpi)|_U\) 是诱导 \(\mathcal{D}_U\) 的 \(U\) 上的方向多向量。
- (c) \(\mathcal{D}_U\) 与诱导 \(\mathcal{D}\) 的 \(V\) 上方向多向量的选择无关。

**证明.** (a), (b)：设 \((h_1,\ldots,h_{m-1})\) 是 \(U\) 中 \((m-1)\) 个向量。因为 \(v\) 在 \(V\setminus U\) 中，由定理 1.1.2，\((h_1,\ldots,h_{m-1})\) 是 \(U\) 的基当且仅当 \((v,h_1,\ldots,h_{m-1})\) 是 \(V\) 的基。假设 \((h_1,\ldots,h_{m-1})\) 确实是 \(U\) 的基。显然 \(\iota_v(\varpi)|_U\) 是 \(\Lambda^{m-1}(U)\) 中的一个多向量。由定义，

\[
\iota_v(\varpi)|_U(h_1,\ldots,h_{m-1})=\varpi(v,h_1,\ldots,h_{m-1}). \tag{8.2.2}
\]

我们由定理 8.2.1 得到 \(\varpi(v,h_1,\ldots,h_{m-1})\ne 0\)，然后由 (8.2.2) 得到 \(\iota_v(\varpi)|_U(h_1,\ldots,h_{m-1})\ne 0\)。这表明 \(\iota_v(\varpi)|_U\) 是非零的，因而是 \(U\) 上的方向多向量。设 \(\mathcal{D}_U\) 是由 \(\iota_v(\varpi)|_U\) 诱导的 \(U\) 的方向。由 (8.2.2) 和定理 8.2.3 可知，\((h_1,\ldots,h_{m-1})\) 相对于 \(\mathcal{D}_U\) 正向当且仅当 \((v,h_1,\ldots,h_{m-1})\) 相对于 \(\mathcal{D}\) 正向。因此，\(\mathcal{D}_U\) 具有所需性质。唯一性由（几乎是显然的）观察可知，向量空间的方向由它包含的基决定。

(c)：这由部分 (a) 可得。

需要说明的是，与定理 8.2.5 的部分 (c) 相反，\(U\) 上诱导的方向与 \(V\setminus U\) 中向量的选择无关。特别地，如果 \(v\) 诱导出 \(\mathcal{D}_U\)，则 \(-v\) 诱导出 \(-\mathcal{D}_U\)。

### 定理 8.2.6.

设 \((V,\mathcal{D})\) 和 \((\widetilde V,\widetilde{\mathcal{D}})\) 是定向向量空间，\(A:V\longrightarrow\widetilde V\) 是一个线性同构，且 \(A(\mathcal{D})=\{A(\mathcal{H}):\mathcal{H}\in\mathcal{D}\}\)。那么要么 \(A(\mathcal{D})=\widetilde{\mathcal{D}}\)，此时 \(A\) 称为 **保向** 的，要么 \(A(\mathcal{D})=-\widetilde{\mathcal{D}}\)，此时 \(A\) 称为 **反向** 的。

**证明.** 设 \(\mathcal{H}\) 和 \(\mathcal{F}\) 是 \(\mathcal{D}\) 中的基，注意到由定理 1.1.13(a)，\(A(\mathcal{H})\) 和 \(A(\mathcal{F})\) 是 \(\widetilde V\) 的基。设 \([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}=[a^i_j]\)。我们由 (2.2.6) 和 (2.2.7) 得到 \(h_j=\sum_i a^i_j f_i\)，于是 \(A(h_j)=\sum_i a^i_j A(f_i)\)，所以 \([\operatorname{id}_{A(\mathcal{H})}^{A(\mathcal{F})}]=[\operatorname{id}_{\mathcal{H}}^{\mathcal{F}}]\)。根据定义，\(\mathcal{H}\) 与 \(\mathcal{F}\) 相容，因此 \(A(\mathcal{H})\) 与 \(A(\mathcal{F})\) 也是相容的。因此，要么 \(A(\mathcal{H})\) 与 \(A(\mathcal{F})\) 在 \(\widetilde{\mathcal{D}}\) 中，要么它们在 \(-\widetilde{\mathcal{D}}\) 中。因为 \(\mathcal{H}\) 与 \(\mathcal{F}\) 是任意的，要么 \(A(\mathcal{D})\subseteq\widetilde{\mathcal{D}}\) 要么 \(A(\mathcal{D})\subseteq-\widetilde{\mathcal{D}}\)。由定理 1.1.9，\(A^{-1}\) 是线性同构，所以类似的论证表明要么 \(A^{-1}(\widetilde{\mathcal{D}})\subseteq\mathcal{D}\) 要么 \(A^{-1}(\widetilde{\mathcal{D}})\subseteq-\mathcal{D}\)，于是要么 \(\widetilde{\mathcal{D}}\subseteq A(\mathcal{D})\) 要么 \(\widetilde{\mathcal{D}}\subseteq A(-\mathcal{D})\)。容易用 (8.2.1) 证明对 \(\mathcal{D}\) 中的任意基 \(\mathcal{H}\) 都有 \(A(-\mathcal{H})=-A(\mathcal{H})\)，所以要么 \(\widetilde{\mathcal{D}}\subseteq A(\mathcal{D})\) 要么 \(-\widetilde{\mathcal{D}}\subseteq A(\mathcal{D})\)。结果得证。

