

现在我们已经有了标量积空间 $(\Lambda^s(V), \mathfrak{g}^\Lambda)$，第 4.5 节中的构造将产生相应的对偶标量积空间 $(\Lambda^s(V)^*, \mathfrak{g}^{\Lambda^*})$ 以及相关的平映射和尖映射：

```
F^\Lambda : \Lambda^s(V) \longrightarrow \Lambda^s(V)^* \quad \text{and} \quad S^\Lambda : \Lambda^s(V)^* \longrightarrow \Lambda^s(V).
```

根据定义，

```
\eta^{F^\Lambda}(\zeta) = \langle \eta, \zeta \rangle^\Lambda \tag{7.5.12}
```

对于 $\Lambda^s(V)$ 中的所有多重余向量 $\eta, \zeta$。

## 第8章

## 定向

定向与“侧向性”有关。这个概念在直觉上显而易见，但要将其以数学上严格的方式表述却出乎意料地困难。在第8.1节中，我们基于具体例子作一些观察，然后给出 $\mathbb{R}^m$ 定向的初步定义。在第8.2节中，利用我们最近掌握的多重向量知识，这些想法将发展成为一个适用于任意向量空间的计算框架。

### 8.1 $\mathbb{R}^m$ 的定向

设 $\mathcal{H}=(h_1,\ldots,h_m)$ 是 $\mathbb{R}^m$ 的一个基。$\mathbb{R}^m$ 中每一个点对应一个由该点关于 $\mathcal{H}$ 的分量组成的 $m$ 元实数组。当 $m=1,2,3$ 时，我们可以将这个 $m$ 元组绘制在轴标签为 $h_1,\ldots,h_m$ 的直角坐标系中。我们利用这种方法的几何直观来引出定向的定义。

考虑 $\mathbb{R}^2$ 的基 $\mathcal{E}=(e_1,e_2)$、$(e_2,-e_1)$ 和 $(e_2,e_1)$，其中 $\mathcal{E}$ 是标准基。在图 8.1.1(a)–(c) 中，这些基被描绘为直角坐标系的坐标轴。图 8.1.1(a) 中的构型，我们称之为 $\mathbb{R}^2$ 的标准构型，被认为具有**逆时针定向**，因为将 $e_1$ 转到 $e_2$ 的 90° 旋转是逆时针方向的。图 8.1.1(b) 中的构型也被认为具有逆时针定向，因为其坐标轴可以绕原点刚性旋转，得到与图 8.1.1(a) 相同的构型。另一方面，图 8.1.1(c) 中的构型被认为具有**顺时针定向**，因为其坐标轴不能以这种方式刚性旋转（等价地，将 $e_1$ 转到 $e_2$ 的 90° 旋转是顺时针方向的）。这些观察结果总结在表 8.1.1 的前三列中。

现在考虑 $\mathbb{R}^3$ 的基 $\mathcal{E}=(e_1,e_2,e_3)$、$(e_2,-e_1,e_3)$ 和 $(e_2,e_1,e_3)$，其中 $\mathcal{E}$ 是标准基。在图 8.1.2(a)–(c) 中，这些基被描绘

![](img/d712710a8eb78c0959204afd5dde4c91_171_0.png)

**图 8.1.1.** $\mathbb{R}^2$ 中的定向

| 图 | $\mathcal{H}$ | 定向 | $[\operatorname{id}_{\mathbb{R}^2}]_{\mathcal{E}}^{\mathcal{H}}$ | $\det([\operatorname{id}_{\mathbb{R}^2}]_{\mathcal{E}}^{\mathcal{H}})$ |
|---|---|---|---|---|
| 8.1.1(a) | $(e_1,e_2)$ | 逆时针 | $\begin{bmatrix}1&0\\0&1\end{bmatrix}$ | $1$ |
| 8.1.1(b) | $(e_2,-e_1)$ | 逆时针 | $\begin{bmatrix}0&1\\-1&0\end{bmatrix}$ | $1$ |
| 8.1.1(c) | $(e_2,e_1)$ | 顺时针 | $\begin{bmatrix}0&1\\1&0\end{bmatrix}$ | $-1$ |

**表 8.1.1.** $\mathbb{R}^2$ 中的定向

为直角坐标系的坐标轴。图 8.1.2(a) 中的构型，我们称之为 $\mathbb{R}^3$ 的标准构型，被认为具有**右手定向**，因为你可以用右手握住正 $e_3$ 轴，拇指指向正 $e_3$ 轴方向，其余手指绕 $e_3$ 轴相对于 $e_1e_2$ 平面逆时针弯曲。图 8.1.2(b) 中的构型也被认为具有右手定向，因为其坐标轴可以刚性旋转，得到与图 8.1.2(a) 相同的构型。另一方面，图 8.1.2(c) 中的构型被认为具有**左手定向**，因为它不能以这种方式刚性旋转（等价地，你可以用左手握住正 $e_3$ 轴，拇指指向正 $e_3$ 轴方向，其余手指绕 $e_3$ 轴相对于 $e_1e_2$ 平面顺时针弯曲）。这些观察结果总结在表 8.1.2 的前三列中。

在表中，我们还给出了将标准基 $\mathcal{E}$ 变为基 $\mathcal{H}$ 的基变换矩阵及其行列式的值。关键观察结果是，对于这些例子，定向保持不变当且仅当 $\det\left([\operatorname{id}_{\mathbb{R}^m}]_{\mathcal{E}}^{\mathcal{H}}\right)>0$。特别地，$\mathcal{E}$ 具有标准定向。

### 8.2 向量空间的定向

在第 8.1 节中，我们通过将 $\mathbb{R}^m$ 的基与标准基进行比较，定义了它具有一定定向的含义。这种定向方法在 $\mathbb{R}^m$ 中有效，因为存在一个明显的参考基选择，而对于任意向量空间，情况可能并非如此。

设 $V$ 是维数为 $m$ 的向量空间，并设 $\mathcal{H}$ 和 $\mathcal{F}$ 是 $V$ 的基。由定理 2.4.12 可知 $\det([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}})\neq 0$。我们称 $\mathcal{H}$ 和 $\mathcal{F}$ 是**一致的**，并记作 $\mathcal{H}\sim\mathcal{F}$，如果

\[\det\left([\operatorname{id}_V]_{\mathcal{H}}^{\mathcal{F}}\right)>0.\]

利用定理 2.2.2(c) 和定理 2.2.5(b)，可以容易地证明 $\sim$ 是 $V$ 的基集合上的一个等价关系，并且恰好有两个等价类。每个等价类都称为 **$V$ 的一个定向**。含给定基 $\mathcal{H}$ 的等价类记作 $[\mathcal{H}]$。设 $\mathcal{H}=(h_1,h_2,\ldots,h_m)$，并设

\[-\mathcal{H}=(-h_1,h_2,\ldots,h_m).\]

显然，$-\mathcal{H}$ 是 $V$ 的一个基，且 $\det([\operatorname{id}_V]_{\mathcal{H}}^{-\mathcal{H}})=-1$，因此 $\mathcal{H}$ 与 $-\mathcal{H}$ 不一致。我们称定向 $[-\mathcal{H}]$ 为 $[\mathcal{H}]$ 的**相反定向**。于是，对于 $V$ 的任意基 $\mathcal{H}$，$V$ 的两个定向为 $[\mathcal{H}]$ 和 $[-\mathcal{H}]$。例如，$\mathbb{R}^m$ 的两个定向是 $[\mathcal{E}]$ 和 $[-\mathcal{E}]$，其中 $\mathcal{E}$ 是 $\mathbb{R}^m$ 的标准基。我们将 $[\mathcal{E}]$ 称为 $\mathbb{R}^m$ 的标准定向。按照这一术语，$\mathcal{E}$ 是处于标准定向中的一个基，这与第 8.1 节中称 $\mathcal{E}$ 具有标准定向略有不同。推广采用这种语言是方便的：对于任何向量空间，处于一个定向中的基称为具有该定向。

一旦选定了 $V$ 的一个定向 $\mathcal{O}$，二元组 $(V,\mathcal{O})$ 就称为一个**定向向量空间**，并称 $V$ 具有定向 $\mathcal{O}$。另一个定向记作 $-\mathcal{O}$，称为 **$\mathcal{O}$ 的相反定向**。如果 $V$ 的一个基在 $\mathcal{O}$ 中，则称它是**正定向的（关于 $\mathcal{O}$）**。我们通常会认为这样说更方便，例如“$\mathcal{H}$ 是 $V$ 中关于 $\mathcal{O}$ 正定向的基”，而不是更简洁的“$\mathcal{H}$ 在 $\mathcal{O}$ 中”。尽管惯例或具体情况往往会影响赋予给定向量空间的定向，但必须强调的是，归根结底这是一个选择问题。$\mathcal{O}$ 或 $-\mathcal{O}$ 作为 $V$ 的定向，并没有内在属性使其中之一比另一个更可取。话虽如此，对于 $\mathbb{R}^m$ 的情况，我们采用以下约定。

