

毒性单位慢性（TUc）是导致慢性暴露期结束时对试验生物无不可接受影响的废水稀释倍数的倒数：

```
TUc = 100 / NOEC  (4.2)
```

其中，与前文相同，NOEC 为无观察到效应浓度。

过去，急性与慢性比值（ACR）由方程 ACR = LC50/NOEC 确定。慢性数据是通过从急性数据外推确定的。急性与慢性比值在不同物种和不同有毒物质之间变化极大。采用全废水方法可防止实施过于严格或过于宽松的要求。

## 4.6.5 毒性试验结果的应用

水质标准通过包含毒性物质允许的量级（数量）、持续时间（河流水体中浓度平均的时间段）以及频率（在不产生不可接受的受纳水体生态群落影响的情况下标准可被超出的次数）来确保指定用途的保护。现代水质标准旨在通过使用标准最大浓度（CMC）防止短期（急性）效应，并通过使用标准持续浓度（CCC）防止长期（慢性）效应。这些标准通常在混合后适用。典型水质标准包含浓度限值、平均期和重现频率。CMC 通常为四日平均浓度，平均而言不得超过每三年一次，CCC 通常为一小时平均浓度，平均而言不得超过每三年一次。

## 4.6.6 对急性毒性的保护

为防止急性毒性，CMC 必须不超过 0.3 TUa，按所进行试验中最敏感结果测定。

```
CMC = TUa / CID ≤ 0.3 TUa  (4.3)
```

其中 CID 为临界初始稀释度。在海洋排放中，CID 定义为在排放附近区域在“最不利”环境条件下实现的稀释度，其中废水羽流的混合和稀释由排放的初始动量和浮力决定。在河流排放中，通常将混合区边界处实现的稀释度作为 CID。根据大量 96 小时废水毒性试验的结果发现，0.3 的系数可解释 91% 的观察到的 LC50 与 LC1 比值（LC1 等于稀释水中导致试验种群 1% 死亡的废水浓度）。因此，为急性保护，CMC 不应超过基于所试验最敏感物种的 0.3 TUa。急性标准因此等于近似 LC1 的 CMC。

## 4.6.7 对慢性毒性的保护

为防止在初始混合区或排放影响区之外发生慢性效应，CCC 必须不超过 1.0 TUc，基于至少三种物种试验中最敏感者获得的结果。

```
CCC = TUc / CID ≤ 1.0 Tuc  (4.4)
```

符合性基于将毒性标准（TUa 和 TUc 均以毒性单位表示）在临界初始稀释后的废水中进行比较，以确定是否满足 EPA 推荐的标准。这些标准的应用在示例 4.3 中说明。

## 示例 4.3 毒性试验结果的应用

向海洋受纳水体排放的经处理废水实现了 225:1 的临界初始稀释度。使用三种海洋物种对 WWTP 废水进行了毒性试验。以下急性与慢性毒性试验结果表明，根据 LC50，C. parvula 表现出最敏感物种急性终点（2.59% 废水），并且根据 NOEC，也表现出最敏感物种慢性终点（1.0%）。使用给定的毒性数据，确定 CMC 和 CCC 标准的符合性。

| Results of acute toxicity tests |
| Species | Exposure (hours) | Control survival (%) | Percent effluent |  |
|  |  |  | LC50 | NOAEL |
| Mysidopsis bahia | 96 | 100 | 18.66 | 10.0 |
| Cyprinodon variegates | 96 | 100 | >100 | 50.0 |
| Champia parvula | 48/168 | 100 | 2.59 | 12.25 |

**Results of chronic toxicity tests**

| Species | Exposure (days) | Control survival (%) | Percent effluent: NOEC | Percent effluent: LOEC |
|---|---:|---:|---:|---:|
| *Mysidopsis bahia* | 7 | 82 | 6.0 | 10.0 |
| *Cyprinodon variegates* | 7 | 98.8 | 15.0 | >15.0 |
| *Champia parvula* | 7 | 100 | 1.0 | 2.25 |

## Solution

- 1. Check compliance with the CMC criterion.
  - a) Based on data for the most sensitive species tested, we use Eq. (4.1) to find the number of acute toxic units:
  
  \(\mathrm{TU_a} = \frac{100}{\mathrm{LC}_{50}} = \frac{100}{2.59} = 38.6\) units
  
  - b) For acute protection, the criterion maximum concentration must not exceed 0.3 TUₐ, as shown in Eq. (4.3). Following an initial dilution of 225, the CMC is:
  
  \(\mathrm{CMC} = \frac{\mathrm{TU_a}}{\mathrm{CID}} \leq 0.3\mathrm{TU_a}\)
  
  \(\frac{38.6}{225} \leq 0.3(38.6)\)
  
  \(0.17 \leq 11.58\)
  
  - c) The CMC (0.17) is considerably less than the value of 0.3 TUₐ (11.58) required for compliance with the CMC criterion.
  
- 2. Check compliance with the CCC criterion.
  - a) Based on data for the most sensitive species tested, we use Eq. (4.2) to find the number of chronic toxic units:
  
  \(\mathrm{TU_c} = \frac{100}{\mathrm{NOEC}} = \frac{100}{1.0} = 100\) units
  
  - b) For chronic protection, the criterion continuous concentration must not exceed 1.0 toxic unit as shown in Eq. (4.4). Following an initial dilution of 225, the CCC is:
  
  \(\mathrm{CCC} = \frac{\mathrm{TU_c}}{\mathrm{CID}} \leq 1.0\mathrm{TU_c}\)
  
  \(\frac{100}{225} \leq 1.0(100)\)
  
  \(0.44 \leq 100\)

The CCC (0.44) is considerably less than the value of 1.0TU₍c₎ (100) required for compliance with the CCC criterion.

采用全废水毒性测试具有诸多优势。在此方法中，有毒物质的生物可利用性得以测定，任何协同相互作用的影响也被考虑在内。由于确定了废水排放物所有组分的总体毒性，因此可以通过限制单一参数——废水毒性来限制毒性效应。由于当代受纳水体管理策略基于特定场地的水质标准，毒性测试便于将废水毒性与旨在保护代表性敏感物种并允许建立保护水生环境的排放限值的特定场地水质标准进行比较。

## 4.7 流量测量

工业用户许可中的污染物限值通常以向工业设施或 POTW 的质量负荷表示（例如，常规污染物、有机物、金属、微生物等的分类标准）。为确定质量负荷，需要获取并记录准确的流量数据。本节简要描述用于测量流量的设备类型和程序。在无法使用以下各节所述的流量测量设备的情况下，设施可能需要依赖设施用水消耗记录。然而，当需要确定质量负荷以评估符合性时，设施应能够在采样期间直接评估设施的流量。

### 4.7.1 开渠流

开渠流是指在未充满液体的管道中发生的流动，是预处理计划监管的工业用户排放点最常见的流动类型。未充满且无压力的半满管道也归类为开渠。开渠流使用初级和次级设备进行测量（如下面所述）。

#### 4.7.1.1 Primary Devices

初级设备是校准过的液压结构，安装在渠道中，以便通过在特定点相对于初级设备的液位深度测量来获得流量测量。堰和量槽是初级设备的例子。

