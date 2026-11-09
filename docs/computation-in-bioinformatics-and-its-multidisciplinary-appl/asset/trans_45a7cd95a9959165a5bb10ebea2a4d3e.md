
| `iGEMDOCK` || 用于识别先导化合物和理解配体结合机制的友好型对接服务器 || [http://gemdock.life.nctu.edu.tw/dock/igemdock.php](http://gemdock.life.nctu.edu.tw/dock/igemdock.php) |
| `PatchDock` || 基于算法的蛋白质、DNA、肽和药物对接研究 || [https://bioinfo3d.cs.tau.ac.il/PatchDock/php.php](http://bioinfo3d.cs.tau.ac.il/PatchDock/php.php) |
| `SwissDock` || 用于预测分子-配体相互作用的基于网络的对接服务器 || [http://www.swissdock.ch/](http://www.swissdock.ch/) |
| `ZDOCK` || 用于蛋白质-蛋白质研究的交互式对接预测工具 || [http://zdock.umassmed.edu/](http://zdock.umassmed.edu/) |

![图 3.4 蛋白质-配体相互作用可视化的示意图，使用两种不同的软件 PyMOL 和 BIOVIO-DVS。](images/fig3-4.jpg)

[**图 3.4**](#c03.xhtml#ifig3-4) 蛋白质-配体相互作用可视化的示意图，使用两种不同的软件 `PyMOL` 和 `BIOVIO-DVS`。

### 3.3.2 可能的理化性质和类药性验证工具

多种强效药物候选物因缺乏药物适用性特征而在临床试验中被淘汰，例如低生物利用度、高毒性、药代动力学不良或药物-药物相互作用。因此，所有平衡形式被认为是药物能力或类药性特征 \[44\]。`CADD` 一直专注于基于理想类药性特征和对靶蛋白的强结合亲和力，对潜在先导化学实体进行搜寻和筛选。在 1990–2010 年间，早期阶段的药物能力预测是不可能的，且 FDA 药物批准率也显著下降 \[45, 46\]。通过计算工具预测可能的药物特征，可减少耗时且昂贵的 *体外* 和 *体内* 实验，以排除不需要的药物候选物。

溶解度、稳定性、药代动力学和 `ADME/T` 特征是药物能力决定因素 \[47, 48\]。Lipinski 五规则 (`RO5`) 或 Pfizer 规则在分析中考虑分子量 (`MW`)、氢键受体数 (`H-BA`)、氢键供体数 (`H-BD`) 和辛醇/水分配系数 (`ClogP`) 值。`RO5` 参数在统计和实验上也直接影响药代动力学特征和 `ADME/T` 性质 \[44\]。根据 `RO5`，预期化学物质的 `MW` 必须 ≤ 500 g/mol，`H-BD` ≤ 5，`H-BA` ≤ 10，且 `ClogP` ≤ 5。如果化合物违反两条或以上规则，则可能对人体无口服活性。因此，`RO5` 标准化是正在进行的计算药物开发平台中的常规分析。

如下所述，大多数上市药物已通过/遵守 `RO5` 原则，因为它是在合成前在计算上验证所设计化学物质类药性的预定义流程，并受制于药理学验证。因此，预测新化学实体的类药性值是现有药物在世界药物索引文件中放置的统计输出。今天，`Molspiration` ([https://www.molinspiration.com/](http://www.molinspiration.com/))、`MolSoft` ([https://www.molsoft.com/](http://www.molsoft.com/)) 和 `SwissADME` ([http://www.swissadme.ch/](http://www.swissadme.ch/)) 是广泛使用的用户友好工具，可通过上传新设计的化学结构或使用简化分子输入行条目系统 (`SMILE`) 记号进行类药性预测，根据工具 ([表 3.2](#c03.xhtml#tab3-2))。

[**表 3.2**](#c03.xhtml#itab3-2) 列出一些广泛且免费使用的理化性质、毒性和 ADMET-药代动力学特征预测工具。

| **工具名称** || **简要描述** || **网络信息** |
| --- | --- | --- |
| `admetSAR` || 通过结构相似性搜索 `QSAR` 模型进行 `ADME/T` 轮廓预测 || [http://lmmd.ecust.edu.cn/admetsar2](http://lmmd.ecust.edu.cn/admetsar2) |
| `Lazar` || 用于预测所需化学物质毒性、致死剂量和推荐剂量的基于 `QSRA` 的框架 || [http://lazar.in-silico.de](http://lazar.in-silico.de) |
| `Molinspiration` || 用于化学实体的理化性质、生物活性预测的基于片段的虚拟筛选平台 || [https://www.molinspiration.com/](http://www.molinspiration.com/) |
| `MolSoft` || 用于化学物质的高速度分子性质和类药性评分计算工具 || [https://www.molsoft.com/](http://www.molsoft.com/) |
| `OECD Toolbox` || 用于预测多个终点以及参考研究的实验数据 || [http://toolbox.oasis-lmc.org](http://toolbox.oasis-lmc.org) |
| `OSIRIS-property explore` || 对化学物质溶解度、毒性理化性质的预测 || [https://www.organicchemistry.org/prog/peo/](http://www.organicchemistry.org/prog/peo/) |
| `PreADMET` || 它是基于训练集数据的类药性、`ADME/T` 和毒性预测工具 || [https://preadmet.bmdrc.kr/](http://preadmet.bmdrc.kr/) |
| `ProTox-II` || 欢迎使用 ProTox-II，用于预测小分子毒性的虚拟实验室 || [http://tox.charite.de/protox\_II/](http://tox.charite.de/protox_II/) |
| `ToxiM` || 用于预测分子溶解度、毒性特征的机器学习方法 || [http://metagenomics.iiserb.ac.in/ToxiM/index.html](http://metagenomics.iiserb.ac.in/ToxiM/index.html) |
| `Swiss-ADME` || 对任何化学实体的 ADMET-药代动力学、生物利用度和类药性性质的预测 || [http://www.swissadme.ch/](http://www.swissadme.ch/) |

### 3.3.3 可能的毒性和 ADME/T 特征预测工具

与毒性相关的不良反应继续挑战任何新引入的药物候选物通过临床试验评估阶段成为上市药物 \[45, 46\]。此外，制药公司基于业务概况，其在临床试验中引入药物的成功与否直接成正比。因此，在临床前阶段更早识别安全问题是一个巨大优势，其中 *计算机模拟* 毒性预测发挥关键作用 \[33, 34\]。使用先进的高通量算法、人工智能技术、多组学临床数据集可提高计算毒性预测的质量和效能，这对制药行业在临床前阶段剔除不需要的有毒化学物质更有益。

若干训练集数据库，名称为 `ACuteTox` ([http://www.acutetox.eu/](http://www.acutetox.eu/))、`Open PHACTS` ([https://www.openphactsfoundation.org/](http://www.openphactsfoundation.org/))、`OpenTox` ([https://www.opentox.net/](http://www.opentox.net/))、`ToxBank` ([http://www.toxbank.net/](http://www.toxbank.net/))、`RepDose` ([https://repdose.item.fraunhofer.de/](http://repdose.item.fraunhofer.de/))、`RISCTOX:` ([http://risctox.istas.net/en/](http://risctox.istas.net/en/)) 和 `TOXNET` ([https://toxnet.nlm.nih.gov/](http://toxnet.nlm.nih.gov/))，汇集了大量临床事实并可免费用于任何研究指导。上述参考数据库的主要目标是提高毒性预测的可靠性，减少实验毒理学测试，并为实验研究选择更安全的化学物质用于药物开发 \[23, 30, 49\]。若干工具已在公共领域可用，可基于所提供的训练数据预测可能的毒性 ([表 3.2](#c03.xhtml#tab3-2))。
