

![](img/c21ce6788ba392bc08a8057d8b20603e_136_0.png)

Fig. 4. Entity Summarization. Summarizing a single entity as envisioned by LinkSum [424]

## 4 COMET: A Context-Aware Matching Technique

为了解决上下文匹配 RDF 实体的问题，提出了 COMET —— 一种上下文感知的 RDF 分子匹配技术。该技术基于 Collarana 等人 [84] 提出的语义数据集成技术，其工作涉及使用语义相似度度量和融合策略来匹配和合并语义相似的 RDF 分子。本工作使用了 RDF 分子的概念，但提出了一种新的方法，即在匹配实体时考虑系统的上下文。COMET 是一个实体匹配框架，旨在创建、识别和匹配上下文等价的 RDF 实体。基于 Collarana 等人 [84] 提出的数据集成技术中的实体匹配组件，我们提出了 COMET，一种基于上下文合并等价 RDF 实体的实体匹配方法。因此，提供了上下文匹配实体的解决方案（图 5）。

### 4.1 Problem Definition

RDF Molecule [84] – 若 Φ(G) 是一个给定的 RDF 图，我们定义 RDF 分子 M 为 Φ(G) 的一个子图，使得，

```
M = {t₁, ..., tₙ}
∀ i, j ∈ {1, ..., n} (subject(tᵢ) = subject(tⱼ))
```

其中 t₁, t₂, ..., tₙ 表示 M 中的三元组。换句话说，RDF 分子 M 由具有相同主体的三元组组成。也就是说，它可以表示为一个元组 M = (R, T)，其中 R 表示分子的 URI，

![](img/c21ce6788ba392bc08a8057d8b20603e_137_0.png)

**Fig. 5. Problem Definition.** The left side shows two RDF Graphs the system Context. The right side shows the application of homomorphism $\theta_C$ on the RDF graphs, resulting in the formation of Contextualized RDF Graph $F_C$.

主体，T 表示属性和值对的集合 $p=(prop,val)$，使得三元组 $(R, prop, val)$ 属于 M。例如，*Arnold Schwarzenegger* 的 RDF 分子为 $(dbr:Arnold-Schwarzenegger, \{(dbo:occupation, Politician), (dbp:title, Governor)\})$。用 RDF 分子描述的 RDF 图 Φ(G) 定义如下：

```
$$\Phi(G)=\{M=(R,T)\mid t=(R,prop,val)\in G\land(prop,val)\in T\}$$
```

*Context* – 我们定义上下文 $C$ 为任何表示系统标准的布尔表达式。两个实体，例如 RDF 分子 $M_1$ 和 $M_2$，相对于给定的上下文可以相似或不相似。也就是说，$C$ 是一个布尔函数，以两个分子 $M_1$ 和 $M_2$ 作为输入，如果它们根据系统上下文相似则返回 true，否则返回 false。下面是上下文 $C$ 的一个示例，模仿图 1 中呈现的示例，其中两个分子如果具有相同的职业则相似。如果 $P=(p,v)$ 是表示分子职业属性的谓词，则上下文。

```
$$C(M_1,M_2)=\begin{cases}\text{true},&\text{if }P\in M_1\land P\in M_2.\\\text{false},&\text{otherwise.}\end{cases}$$
```

根据集成场景的要求，此上下文可以是任何布尔表达式。

*Semantic Similarity Function* – 设 $M_1$ 和 $M_2$ 为任意两个 RDF 分子。那么 *semantic similarity function* $Sim_f$ 是一个函数，用于衡量这两个分子之间的 *semantic similarity* 并返回 [0, 1] 之间的值。0 的值表示两个分子完全不相似，1 表示分子相同。GADES [371] 中定义了此类相似度函数。

Contextually Equivalent RDF Molecule - 设 Φ(G) 和 Φ(D) 为两个 RDF 分子集合。设 $M_G$ 和 $M_D$ 分别来自 Φ(G) 和 Φ(D) 的两个 RDF 分子。那么，$M_G$ 和 $M_D$ 被定义为上下文等价当且仅当

- 1. 它们处于同一上下文中。也就是说，$C(M_1, M_2) = \textbf{true}$
- 2. 它们具有最高的相似度值，即，
- $Sim_f(M_G, M_D) = max(\forall m \in \Phi(D) Sim_f(M_G, m))$

设 $F_c$ 为来自 Φ(G) 和 Φ(D) 的 *contextually integrated* RDF 分子的理想集合。设 $\theta_c$ 为一个同态，使得 $\theta_c: \Phi(G) \cup \Phi(D) \rightarrow F_c$。那么存在一个来自 $F_c$ 的 RDF 分子 $M_F$ 使得 $\theta(M_D) = \theta(M_G) = M_F$。从动机示例来看，这意味着政治家 *Arnold Schwarzenegger* 的分子与 *Donald Trump* 的分子是 *contextually equivalent*，因为它们相似 *并且* 满足具有相同职业的上下文条件。

在本工作中，我们解决了显式建模上下文然后匹配来自 RDF 图的 RDF 分子的难题，这些分子在相似性方面都很高并且在该上下文方面等价。该问题定义如下：给定 RDF 图 Φ(G) 和 Φ(D)，设 $M_G$ 和 $M_D$ 为两个 RDF 分子，使得 $M_G \in \Phi(G)$ 且 $M_D \in \Phi(D)$。系统提供了一个上下文参数 $C$，它是一个布尔函数，用于评估两个分子是否处于同一上下文中。它还提供了一个相似度函数 $Sim_f$，用于评估 $M_G$ 和 $M_D$ 之间的语义相似度。

创建上下文图 Φ_C 的问题在于构建一个同态 $\theta_c: \Phi(G) \cup \Phi(D) \rightarrow F_c$，使得对于属于 Φ_C 的每对 RDF 分子，都不存在根据系统上下文 $C$ *contextually equivalent* 的分子。如果 $M_G$ 和 $M_D$ 是属于 $F_c$ 的上下文等价分子，则 $\theta_c(M_G) = \theta_c(M_G)$，否则 $\theta_c(M_G) \neq \theta_c(M_D)$。

该问题的示例在图 X 中说明，该图描绘了一个使用两个 RDF 图和单个上下文参数 $C$ 的用例。相对于 $C$，来自 Φ(G) 的 RDF 分子 *Arnold.S* 与来自 Φ(D) 的 *Donald.T* 处于同一上下文中，但不与来自 Φ(G) 的分子 *Arnold.S* 处于同一上下文中。因此，该问题是要识别一个同态 $\theta_c$，该同态根据系统上下文评估 RDF 分子并以一种方式映射这些 RDF 分子，以便它们可以集成到上下文图中。

### 4.2 The COMET Architecture

我们提出了 COMET，一种根据给定上下文匹配上下文等价 RDF 图的方法，从而为 *contextually matching* RDF 图的问题提供了解决方案。图 6 描述了 COMET 架构的主要组件。COMET 采用双重方法以上下文感知的方式解决 RDF 图中的实体匹配问题：首先，COMET 计算 RDF 实体之间的相似度度量，并利用形式概念分析算法来映射上下文等价的 RDF 实体。最后，COMET 结合第一步的结果并执行 1-1 完美匹配算法，根据组合得分匹配 RDF 实体，最终将匹配综合为上下文 RDF 图。

![](img/c21ce6788ba392bc08a8057d8b20603e_139_0.png)

Fig. 6. The COMET Architecture. COMET receives two RDF datasets, e.g., G and D; a similarity function Sim_f; and a context C. The output is a set of contextually matching RDF entities.

### 4.3 Identifying Contextually Equivalent Entities

Building a Bipartite Graph. COMET 管道接收两个 RDF 图 Φ(G)、Φ(D) 作为输入，以及上下文参数 C 和相似度函数 Sim_f。COMET 首先在集合 φ(G) 和 φ(D) 之间构建一个二部图。数据集分区器采用相似度函数 Sim_f 和本体 O 来计算 φ(G) 和 φ(D) 中 RDF 分子之间的相似度，并将相似度分数分配为二部图中的顶点权重。COMET 允许任意用户提供的相似度函数，利用不同的算法来估计 RDF 分子之间的相似度。因此，COMET 支持多种相似度函数，包括简单的字符串相似度。然而，如 [84] 所示，语义相似度度量是推荐的（在本工作的实现中我们特别使用 GADES [371]），因为它们通过考虑 RDF 图中编码的语义而取得更好的结果。

在 RDF 分子相似度比较之后，相似度函数的结果会针对阈值 γ 进行测试，以确定实体相似度（相似度阈值的最低可接受分数）。因此，权重低于 γ 的边会被从二部图中丢弃。等于 0.0 的阈值不对相似度值施加任何限制；因此二部图包含所有边。较高的阈值，例如 0.8，限制相似度值，导致二部图仅包含少数几条边。

