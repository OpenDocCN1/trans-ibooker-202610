

### 类型 1：仅属于一个特征的术语

如果所有术语 *t₁, t₂, …, tₙ* 映射到同一个特征 *f*，则 *f* 将作为结果返回，并附带每个术语的累积 LDA 得分。对于所有 *i*，1 < *i* < *n*，存在 *f* 使得

```
f ← ontoMap(tᵢ), where t ∈ T, and f ∈ F
fScore(f) = Σ LDA score(tᵢ), tᵢ ∈ T and i = 1, … n
cum_fScore(f) = LDA score(f) + fScore(f)
```

例如，“treatment is good” 被认为是类型 1 查询。这里提取出单词 *treatment*。它属于特征（或主题）“Medicare”，并作为特征返回。

### 类型 2：术语属于多个特征，每个术语仅属于一个特征

如果术语 { *tᵢ* } 映射到特征 *fₐ* ∈ F，且术语 { *tⱼ* } 映射到特征 *fᵦ* ∈ F，则返回 *fScore* 较高的 *f*。在这种情况下，为每个特征 *f* 计算累积 *fScore*，并选择得分较高的特征。特征 *f* 的累积 *fScore* 由对应于该特征 *f* 的术语的 LDA 得分之和决定。

```
fₐ ← ontoMap(tᵢ), where t ∈ T, and f ∈ F
fᵦ ← ontoMap(tⱼ), where t ∈ T, and f ∈ F where tᵢ ≠ tⱼ, and fₐ ≠ fᵦ
cum_fScore(f) = Σ LDA score(t), t ∈ T and i = 1, … n
fScore(f) = max (cum_fScore(fₐ), cum_fScore(fᵦ))
```

例如，“the treatment is good but the rent is costly” 被认为是类型 2 查询。这里提取出术语 *treatment* 和 *rent*。术语 *treatment* 属于主题“Medicare”，而术语 *rent* 属于主题“Cost”。

### 类型 3：术语属于多个特征，一个术语属于多个特征

如果某个术语 *tᵢ* 映射到特征 *fₐ, fᵦ* ∈ F，则考虑最近的术语，例如 *tᵢ₋₁* 或 *tᵢ₊₁* 来确定文档中术语 *tᵢ* 的特征类别。在这种情况下，通过比较每个特征的 *fScore* 将术语关联到一个特征。为每个特征 *f* 计算累积 *fScore*，并将结果的强特征返回为结果。特征 *f* 的累积 *fScore* 由对应于该特征 *f* 的术语的 LDA 得分之和决定。

```
f_a ← ontoMap(t), where t ∈ T, and f ∈ F
f_b ← ontoMap(t), where t ∈ T, and f ∈ F where f_a ≠ f_b
cum_fScore(f) = Σ LDAscore(t), i ∈ i-1^st, i^th or i+1^st terms
fScore(f) = max(cum_fScore(f_a), cum_fScore(f_b))
```

例如，“the rooms are maintained neatly but the room rent is costly” 被认为是类型 3 查询。这里提取出术语 *rooms*、*room* 和 *rent*。术语 *room* 同时属于主题“基础设施”和“成本”，而术语 *rent* 属于主题“成本”。术语 *room* 需要固定到单一主题（基础设施或成本）。这是通过计算术语出现的特征（或主题）的累积得分来完成的。假设，LDAscore(room, Infrastructure) = 0.17，LDAscore(room, Cost) = 0.38，且 LDAscore(rent, Cost) = 0.26。那么 cum_fScore(f = “Infrastructure”) 为 0.17，而 cum_fScore(f = “Cost”) 为 0.64。由于 0.64 > 0.17，因此在此上下文中术语 *room* 被分配为特征“Cost”。

### 类型 4：本体中不存在的术语

如果给定的术语不存在，则为其计算新的 LDA 得分并使用 update-Ontology() 对新术语进行更新。如果 ontoMap(t) 为 *null*，其中 t ∈ T，则需要用新术语更新本体。对本体更新再次进行 CFS_LDA 建模。查询过程重复为前面描述的另外三种类型之一。

## 1.5.4 指标分析

准确率被用作正确识别出属于主题（或特征）的术语数量的性能度量。然而，该模型还通过诸如精确率、召回率和 F 测度等指标进行进一步评估。精确率定义了模型的精确程度，而召回率定义了模型的完整程度。它表示为混淆矩阵，其中包含关于实际结果和预测结果的信息，如表 1.1 所示。

精确率定义为返回的文档中正确识别文档的百分比；而召回率定义为正确识别文档中相关结果的百分比。实际上，以牺牲精确率为代价可以获得较高的召回率，反之亦然 [61]。然而，当需要单一指标来比较不同模型时，F 测度指标是合适的。它定义为精确率和召回率的调和平均数。

**表 1.1 混淆矩阵。**

| | 预测为阳性 | 预测为阴性 |
|---|---|---|
| 实际阳性 | TP | FN |
| 实际阴性 | FP | TN |

其中：

- TP：阳性示例的正确分类数量（真正例）
- FN：阳性示例的错误分类数量（假阴性）
- FP：阴性示例的错误分类数量（假阳性）
- TN：阴性示例的正确分类数量（真阴性）

基于混淆矩阵，正类的精确率、召回率和 F 测度定义为

```
precision = TP / (TP + FP)    (1.3)

recall = TP / (TP + FN)    (1.4)

F − measure = 2 · (p · r) / (p + r)    (1.5)

accuracy = (TP + TN) / (TP + TN + FP + FN)    (1.6)
```

本体语义索引（OnSI）模型通过诸如精确率、召回率和准确率等指标进行评估，如公式 1.3、1.4、1.5 和 1.6 所示。

## 1.6 数据集描述

类似医疗服务评论的文档集合（数据集 DS1）从不同社交媒体网站收集，共涉及 10 家不同医院，详细情况见表 1.2 和 1.3。

**表 1.2 医疗服务社交媒体评论（DS1）。**

| 数据来源 | 评论数量（正面） | 评论数量（负面） |
|---|---|---|
| Twitter | 1200 | 525 |
| Mouthshut.com | 425 | 110 |
| BestHospitalAdvisor.com | 200 | 85 |
| Google Reviews | 580 | 320 |
| 总评论数 | 2405 | 1040 |

**表 1.3 特征和医院的评论数量（DS1）。**

| 特征 | 评论数 |
|---|---|
| Cost | 663 |
| Medicare | 748 |
| Nursing | 776 |
| Infrastructure | 554 |
| Time | 704 |

| 医院 | 评论数量 | 医院 | 评论数量 |
|---|---|---|---|
| H1 | 596 | H6 | 308 |
| H2 | 411 | H7 | 313 |
| H3 | 399 | H8 | 297 |
| H4 | 227 | H9 | 281 |
| H5 | 252 | H10 | 361 |

表 1.3 显示，为特征“cost”收集了 663 条评论，为医院 H1 收集了 596 条评论。特征列表来自先前的工作、相关文章和社交媒体网站。例如，文献 [62] 明确指出了医疗服务所需的必要标准。文献 [63] 中明确提到了患者期望，例如成本、医院特征、基础设施设施、其他用户的推荐、治疗和护理以及医疗专业知识。特征还通过参考 BestHospitalAdvisor、Mouthshut.com、webometrics 等网站进行选择。对这些网站中患者或其护理人员提供的评论进行了分析，以了解这些不同医院医疗服务的各个特征。

## 1.7 结果与讨论

本体语义索引（OnSI）模型使用 CFS_LDA 模型中选择的特征为每个产品/服务评论文档构建领域本体。使用 Protégé 软件构建和查询本体模型。LDA 模型为该数据集的每个主题选择的 top five 术语如表 1.4 所示。每个主题根据列表中的第一个术语在上下文中手动标记。很难对归属于主题下的所有术语进行人工标注。

例如，术语“bill”是主题“medicare”下的 top 词之一。然而，它更与主题“cost”或“infrastructure”或“time”相关。同样，术语“appointment”在 top 5 或 top 10 术语列表中均未出现；然而，它更适用于主题“time”和“medicare”。为缓解此问题，CFS_LDA 模型使用相关性分析，参考列表中第一个术语（每个主题中具有最高术语-主题概率的术语），选择每个主题的代表性术语。如前例所述，术语“bill”与术语“doctor”无关，且与术语“cost”、“hospital”和“time”高度相关。这些术语的相关值如表 1.5 所示。例如，“room”的术语-主题概率“Φ_tw”为 0.0134，与“cost”的相关值“c”为 0.0222。如另一示例所述，术语“appointment”与术语“doctor”和“time”高度相关，并被归入主题“medicare”和“time”，如表 1.5 所示。另一个例子是，术语“disease”与“doctor”和“hospital”相关，而与术语“cost”、“time”和“staff”无关，如表 1.5 所示。

表 1.6 显示了 CFS_LDA 模型选择的特征术语列表。在预处理和 PoS 标注的名词中，68 个术语被选为“cost”主题，110 个为“medicare”，112 个为“staff”，101 个为“infrastructure”，73 个为“time”。

因此，特征提取过程模型 CFS_LDA 不仅选择具有高术语-主题概率值的术语，还选择与每个主题下最顶层术语高度相关的术语。

**表 1.4 LDA 模型按主题列出的前五项术语。**

| Topics/features of (DS1) | Feature terms by FS_LDA model (DS1) |
|---|---|
| Topic 1—Cost | cost, test, money, charge, day |
| Topic 2—Medicare | doctor, nurse, team, treatment, bill |
| Topic 3—Staff | staff, patient, child, problem, face |
| Topic 4—Infrastructure | hospital, people, room, experience, surgery |
| Topic 5—Time | time, operation, hour, service, check |

