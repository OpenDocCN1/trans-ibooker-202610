
若使用规则 F15（分别，F16）推导出 Env ⊢ e ↝ v，则 e = fun x ↦ e_f 且 v = ⟨x, e_f, Env⟩（分别，e = rec f x = e_f 且 v = ⟨f, x, e_f, Env⟩），且由于 F15（分别，F16）是对函数（分别，递归函数）求值的唯一适用规则，则 v = v'。

若使用规则 F17（分别，F18）推导出 Env ⊢ e ↝ v，则 e = (e₁ e₂) 且：

Env ⊢ e₁ ↝ ⟨x, e_f, Env_f⟩（分别，Env ⊢ e₁ ↝ ⟨f, x, e_f, Env_f⟩）

Env ⊢ e₂ ↝ v₂

(x, v₂) ⊕ Env_f ⊢ e_f ↝ v（分别，(f, ⟨f, x, e_f, Env_f⟩) ⊕ (x, v₂) ⊕ Env_f ⊢ e_f ↝ v）

根据归纳假设，若：

Env ⊢ e₁ ↝ v'₁ 则 v'₁ = ⟨x, e_f, Env_f⟩（分别 v'₁ = ⟨f, x, e_f, Env_f⟩）

且若：

Env ⊢ e₂ ↝ v'₂ 则 v'₂ = v₂

因此：

(x, v'₂) ⊕ Env_f = (x, v₂) ⊕ Env_f

（分别 (f, v'₁) ⊕ (x, v'₂) ⊕ Env_f = (f, ⟨f, x, e_f, Env_f⟩) ⊕ (x, v₂) ⊕ Env_f）

唯一适用规则是 F17（分别，F18），且根据归纳假设，由于：

(x, v₂) ⊕ Env_f ⊢ e_f ↝ v

（分别，(f, ⟨f, x, e_f, Env_f⟩) ⊕ (x, v₂) ⊕ Env_f ⊢ e_f ↝ v）

我们得到 v = v'。

若使用规则 F19 推导出 Env ⊢ e ↝ v，则 e = (e₁ e₂)，Env ⊢ e₁ ↝ v₁，Env ⊢ e₂ ↝ v₂，v₁ ∈ IF，v₂ ∈ IE 且 v = v₂。根据归纳假设，若 Env ⊢ e₁ ↝ v'₁，则 v'₁ = v₁ ∈ IF；且若 Env ⊢ e₂ ↝ v'₂，则 v'₂ = v₂ ∈ IE。规则 F19 是对 e 求值的唯一适用规则，我们得到 v' = v'₂ = v₂ = v。

若使用规则 F20 推导出 Env ⊢ e ↝ v，则 e = (e₁ e₂)，Env ⊢ e₁ ↝ v₁，v₁ ∉ IF 且 v = Err_T。根据归纳假设，若 Env ⊢ e₁ ↝ v'₁，则 v'₁ = v₁ ∉ IF。规则 F20 是对 e 求值的唯一适用规则，我们得到 v' = v。

下述命题建立了由求值函数 [[\_]] 定义的语义与在方框 3.2 和方框 3.3 中定义的按值调用运算语义之间的等价性。

## 命题 3.2. 等价性:

∀e ∈ **Exp₂** ∀Env ∈ **E** ∀v ∈ V [[e]]_Env = v ⇔ Env ⊢ e ↝ v

蕴含 ⇒ 可使用 LFP 理论证明，但这超出了本书的范围。蕴含 ⇐ 的证明是采用与命题 3.1 相同的技术，通过对 Env ⊢ e ↝ v 的求值树进行归纳得到的。对于每条规则：

```
(Fᵢ) Env₁ ⊢ e₁ ↝ v₁ ... Envₙ ⊢ eₙ ↝ vₙ
--------------------------------------
Env ⊢ e ↝ v
```

在定义 **Exp₂** 中表达式的大步运算语义的推理系统 S_**Exp₂** 中，我们必须证明 [[e]]_Env = v，在归纳假设下，即 [[e₁]]_Env₁ = v₁，等等，[[eₙ]]_Envₙ = vₙ。此证明是直接的，因为运算语义的求值规则是按照与定义求值函数完全相同的选择来定义的。

## 3.5. 语义属性

编程语言中用于证明表达式语义属性的常用技术如下所示。

### 3.5.1. 等价表达式

### 定义 3.3. 等价表达式：

两个表达式等价，当且仅当，在每个环境中，它们的求值返回相同的值：

∀e₁, e₂ ∈ **Exp₂** e₁ ≡ e₂ ⇔ (∀Env ∈ **E** [[e₁]]_Env = [[e₂]]_Env) [3.8]

在本定义中，我们使用了求值函数 [[__]]_ 对表达式进行求值。用于求值大步运算语义的关系 ↝ 也可用于定义等价表达式的概念：

∀e₁, e₂ ∈ **Exp₂**
e₁ ≡ e₂ ⇔ (∀Env ∈ **E** ∀v ∈ V Env ⊢ e₁ ↝ v ⇔ Env ⊢ e₂ ↝ v) [3.9]

使用第二个定义，证明 e₁ ≡ e₂ 相当于证明如何从 Env ⊢ e₁ ↝ v 的树得到 Env ⊢ e₂ ↝ v 的求值树，反之亦然。根据命题 3.2，这两种方法是等价的。我们可以轻松证明 ≡ 定义了一个同余关系，即与 **Exp₂** 的内部组成规律兼容的等价关系。例如，若 e₁ ≡ e₂ 且 e'₁ ≡ e'₂，则 (e₁ + e'₁) ≡ (e₂ + e'₂)。

### 示例 3.6。

我们可以证明 let x = e1 in e2 ≡ ((fun x ↦ e2) e1)。使用定义 [3.8]，通过分情况证明，利用 e1 的求值来证得此等价性。

若 [[e1]]Env = v ∈ V，则 [[let x = e1 in e2]]Env = [[e2]](x,v)⊕Env，且由于 [[fun x ↦ e2]]Env = ⟨x, e2, Env⟩，我们得到 [[(fun x ↦ e2) e1]]Env = [[e2]](x,v)⊕Env，从而证明结束。

若 [[e1]]Env = v ∈ IE，也可以完成证明。在此情况下，有 [[let x = e1 in e2]]Env = v = [[(fun x ↦ e2) e1]]Env'。

该等价性可通过考虑定义 [3.9] 来建立。首先，取一棵树 Env ⊢ let x = e1 in e2 ↝ v。有两种可能性。若此树是通过应用规则 F13 得到的，它将采用以下形式：

![](img/c9a176746efdbd36ba74d71dfdc86e86_85_0.png)

且一棵 Env ⊢ ((fun x ↦ e2) e1) ↝ v 的树可以从树 ∇1 和 ∇2 容易构造：

![](img/c9a176746efdbd36ba74d71dfdc86e86_85_1.png)

其次，树 Env ⊢ let x = e1 in e2 ↝ v 是使用规则 F14 得到的：

![](img/c9a176746efdbd36ba74d71dfdc86e86_85_2.png)

再次，一棵 Env ⊢ ((fun x ↦ e2) e1) ↝ v 的树可以从 ∇ 容易构造：

![](img/c9a176746efdbd36ba74d71dfdc86e86_85_3.png)

反过来，一棵 Env ⊢ ((fun x ↦ e2) e1) ↝ v 的树可用于构造一棵 Env ⊢ let x = e1 in e2 ↝ v 的树，因为树 [3.10] 和 [3.12] 分别可从树 [3.11] 和 [3.13] 得到。

### 3.5.2. 等价环境

如我们在 3.1.4 节中非正式所述，在环境 ENV 中对表达式 e 求值的结果仅取决于 ENV 与 e 的自由变量关联的值。换句话说，e 的求值结果在所有将相同值关联到 e 的自由变量的环境中都是相同的。给出此属性的正式定义需要引入变量集上等价环境的概念，如下所示：

### 定义 3.4. 等价环境：

令 \( F \subseteq \mathbf{X} \) 为变量集，ENV₁ 和 ENV₂ 为两个环境：

ENV\(_1 \equiv_F\) ENV\(_2 \Leftrightarrow \forall x \in F \, \) ENV\(_1(x) = \) ENV\(_2(x)\)

显然，给定 \(\mathbf{X}\) 的两个子集 \( F_1 \) 和 \( F_2 \)，若 \( F_1 \subseteq F_2 \) 且 ENV₁ ≡_{F₂} ENV₂，则 ENV₁ ≡_{F₁} ENV₂。现在可以正式地表达，对于任何表达式 e，[[e]]_Env 的值仅取决于 ENV 中绑定到 e 的自由变量（\(\mathcal{F}(e)\)）的值。

### 命题 3.3。

对于任何表达式 \( e \in \mathbf{Exp}_2 \):

\(\forall \text{ENV}_1, \text{ENV}_2 \in \mathbf{E}\)
ENV\(_1 \equiv_{\mathcal{F}(e)}\) ENV\(_2\)
\(\Rightarrow\)
\(\left( \begin{aligned} [[e]]_{\text{ENV}_1} = [[e]]_{\text{ENV}_2} \\ \text{or} [[e]]_{\text{ENV}_1} = \langle x, e', \text{ENV}'_1 \rangle \text{ and } [[e]]_{\text{ENV}_2} = \langle x, e', \text{ENV}'_2 \rangle \\ \text{and } \text{ENV}'_1 \equiv_{\mathcal{F}(e)\backslash \{x\}} \text{ENV}'_2 \\ \text{or} [[e]]_{\text{ENV}_1} = \langle f, x, e', \text{ENV}'_1 \rangle \text{ and } [[e]]_{\text{ENV}_2} = \langle f, x, e', \text{ENV}'_2 \rangle \\ \text{and } \text{ENV}'_1 \equiv_{\mathcal{F}(e)\backslash \{x\}} \text{ENV}'_2 \end{aligned} \right)\)

此命题的证明冗长而复杂。练习 3.5 为不含函数表达式的语言建立了此属性。在此情况下，证明通过对表达式的句法结构进行归纳得到。在存在函数值的情况下，此归纳不再充分，证明需要使用不动点理论的定理。

## 3.6. 习题

### 练习 3.1

给定环境 Env，计算：

\( [[\text{fun } x \mapsto \text{let } y = 3 \text{ in } x + y]]_{\text{Env}} \text{ and } [[\text{let } y = 3 \text{ in } \text{fun } x \mapsto x + y]]_{\text{Env}} \)
