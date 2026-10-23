

在工业设施中，此类后果评估或应急规划根据《清洁空气法》（CAA）第40篇《联邦法规汇编》第68部分的规定是必需的。

扩散模型因用于开发模型的数学方法而有所不同，但所有模型都需要输入数据，可能包括以下内容：

- 气象条件，例如风速和风向、大气湍流的量（用所谓的“稳定度等级”表征）、环境空气温度、可能存在的上层逆温层的底部高度、云量和太阳辐射
- 源项（排放或事故释放源项中有毒物质的浓度或数量）和材料的温度
- 排放或释放参数，例如源位置和高度、源类型（即火灾、池或排气烟囱）和出口速度、出口温度以及质量流量或释放速率
- 源位置和受体位置（例如附近的住宅、学校、商业区和医院）的地形高程
- 排放气体羽流路径上任何障碍物（例如建筑物或其他结构）的<|message|>位置、高度和宽度、地表粗糙度，或使用更通用的参数“乡村”或“城市”地形

许多现代先进的扩散建模程序包括用于输入气象和其他数据的预处理器模块，许多还包括用于绘制输出数据和/或在地图上绘制空气污染物影响区域的后处理器模块。受影响区域的图还可能包括显示从最小到高浓度的区域的等值线，从而定义健康风险最高的区域。等值线图有助于确定公众和应急人员的保护措施。

大气扩散模型也称为大气扩散模型、空气扩散模型、空气质量模型和空气污染扩散模型。

## 4.15.1 大气层

讨论地球大气层的层是理解空气污染物在大气中扩散位置所必需的。最靠近地球表面的层称为对流层。它从海平面延伸到约18 km的高度，包含整体大气质量的约80%。平流层是下一层，从18 km延伸到约50 km。第三层是中间层，从50 km延伸到约80 km。

80 km以上还有其他层，但对于大气扩散建模而言并不重要。在中尺度上，图2.2显示了大气中随高度变化的温度而变化的四个区域，因此密度也会变化。

对流层的最低部分称为大气边界层（ABL）或行星边界层，从地球表面延伸到约1.5–2.0 km的高度。大气边界层的空气温度随高度增加而降低，直到达到所谓的逆温层（温度随高度增加而增加）为止，该逆温层限制了大气边界层。对流层的上部（即逆温层以上）称为自由对流层，延伸至对流层的18 km高度。

ABL对于排放、输送和扩散空气污染物而言是最重要的。ABL中地球表面与逆温层底部之间的部分称为混合层。几乎所有排放到环境大气中的空气污染物都在混合层内输送和扩散。部分排放物穿透逆温层并进入ABL上方的自由对流层。

总之，从地面向上，地球大气层的层为由逆温层封顶的混合层组成的ABL、自由对流层、平流层、中间层及其他。许多大气扩散模型被称为边界层模型，因为它们主要模拟ABL内的空气污染物扩散。为了避免混淆，被称为中尺度模型 的模型具有水平延伸至数百公里的扩散建模能力。这并不意味着它们模拟中间层的扩散。

### 示例 4.11 一台500-MW燃煤电厂正在燃烧热值为12 000 Btu/lb的煤。煤中含有2.0%的硫和150 ppb的水银。电厂的热效率为38%，空气/燃料比为14 : 1（质量基准）。计算 (a) 煤燃烧速率（lb/h），(b) 烟气流量（scfm），(c) 为满足1.4 lb/MWh（按总发电量计）的标准所需的SO₂去除效率，以及 (d) 为满足0.015 lb/GWh（按总能量输出计）的汞标准所需的整体汞捕获百分比。

已知：热值 = 12 000 Btu/lb
煤含量：2.0% S，150 ppb Hg
ηthermal = 38%
A/F = 14 : 1（质量基准）

## 4.16 扩散模型：室内浓度

Solution

- a) 煤燃烧速率
- 能量输入：$E_0 = \frac{500\mathrm{MW}}{0.38} = 1316\mathrm{MW}$
- $E_0 = \left(\frac{3.412 \times 10^6 \mathrm{Btu}}{\mathrm{MW}} \frac{1}{\mathrm{h}}\right)(1316 \mathrm{MW}) = 4.49 \times 10^9 \frac{\mathrm{Btu}}{\mathrm{h}}$
- 煤输入 = $\frac{4.49 \times 10^9 \frac{\mathrm{Btu}}{\mathrm{h}}}{12000 \frac{\mathrm{Btu}}{\mathrm{lb}}} = 374123 \frac{\mathrm{lb}}{\mathrm{h}}$

- b) 烟气流量：
- A/F = 14:1，因此烟气质量流量为煤进料速率的15倍
- $\mathcal{Q}_{\mathrm{FG}} = \left(374123\frac{\mathrm{lb}}{\mathrm{h}}\right) \times (14+1) = 5\,611\,845 \frac{\mathrm{lb}}{\mathrm{h}}$
- $\mathcal{Q}_{\mathrm{FG}} = 5\,611\,845 \frac{\mathrm{lb}}{\mathrm{h}} \times \left(\frac{1\mathrm{ft}^3}{0.075\mathrm{lb}}\right) \times \left(\frac{1\mathrm{h}}{60\mathrm{min}}\right)$
- = 1 247 000 scfm

- c) $\mathrm{SO}_2$ 去除效率
- 燃料中的硫：$\left(374123\frac{\mathrm{lb}}{\mathrm{h}}\right) \times 0.02 = 7482 \mathrm{lb} \frac{\mathrm{S}}{\mathrm{h}}$
- $\mathrm{S}+\mathrm{O}_2 \rightarrow \mathrm{SO}_2$
- $\left(\frac{7\,482\mathrm{lb}\mathrm{S}}{\mathrm{h}}\right) \times \left(\frac{1\mathrm{bmol}\mathrm{S}}{32\mathrm{lb}}\right) \times \left(\frac{1\mathrm{bmol}\mathrm{SO}_2}{1\mathrm{bmol}\mathrm{S}}\right) \times \left(\frac{64\mathrm{lb}}{1\mathrm{bmol}\mathrm{SO}_2}\right)$
- $= \frac{14\,965\mathrm{lb}\mathrm{SO}_2}{\mathrm{h}}$
- $\frac{14\,965\mathrm{lb}\mathrm{SO}_2}{\mathrm{h}} \frac{1}{500\mathrm{MW}} = \frac{\mathrm{SO}_2}{\mathrm{MWh}}$
- $\frac{\left[(29.9-1.4)\mathrm{lb}\mathrm{SO}_2}{\mathrm{MWh}}\right] \times 100}$
- $\frac{(29.9\mathrm{lb}\mathrm{SO}_2}{\mathrm{MWh}})$
- = 95.3% 硫去除效率

- d) 整体汞捕获百分比
- $m_{\mathrm{Hg}} = \frac{\left(374123\mathrm{lb}\mathrm{coal}}{\mathrm{h}}\right) \times (150\mathrm{ppb}\mathrm{Hg})}{10^9} = \frac{0.0561\mathrm{lb}\mathrm{Hg}}{\mathrm{h}}$

总能量输出：

$\left(\frac{500\mathrm{MW}}{\mathrm{h}}\right) \times \left(\frac{1\mathrm{GWh}}{1000\mathrm{MWh}}\right) = \frac{0.50\mathrm{GWh}}{\mathrm{h}}$

未控制的汞排放：

$\begin{array}{c}\frac{0.0561\mathrm{lb}\mathrm{Hg}}{\mathrm{h}}\\\\=\frac{0.0561\mathrm{lb}\mathrm{Hg}}{0.500\mathrm{GWh}}\end{array}=\frac{0.1122\mathrm{lb}}{\mathrm{GWh}}$

汞捕获百分比：

$\left[\frac{0.1122\mathrm{lb}\quad 0.015\mathrm{lb}}{\mathrm{GWh}\quad\mathrm{GWh}}\right]$ $\frac{0.1122\mathrm{lb}}{\mathrm{GWh}}$ $\times 100 = 86.66\%$

### 4.16 扩散模型：室内浓度

工作场所空气中污染物的扩散导致污染物从高浓度区域向低浓度区域的净迁移。污染物的扩散得到通风系统驱动的平流传质的帮助。这两种影响的结合导致污染物从其源头向周围房间迁移。

质量平衡模型假定工作区域内化学污染物的浓度是均匀的。扩散模型具有显著优势；它们描述了污染物浓度随距源距离的变化。浓度梯度由以下方程描述，当平流仅在 *x* 方向发生且扩散在各方向相等时：

```
u dC/dx = D/r^2 d/dr [r dc/dr]    (4.14)
```

- *u* 是 *x* 方向的空气速度（长度/时间）
- *C* 是空气污染物的浓度（质量/长度）
- *D* 是扩散系数（长度$^2$/时间）
- *x* 是距源的下风向距离（长度）
- *r* 是从源到采样点的距离（长度）

