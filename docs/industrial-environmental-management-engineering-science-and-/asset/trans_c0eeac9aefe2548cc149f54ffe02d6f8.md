

用户指定的表面特征（Z₀, B₀, r）。对于稳定情况，机械混合高度（Zₘ）是基于摩擦速度 u* 计算的。还包括通过 Zₘ 因子（乘以由 u* 计算的初始值）的循环，以考虑使用精炼 AERMET 数据时 Zₘ 的平滑。太阳角度大于零但小于临界太阳角度（ACRIT）的稳定过渡情况也包含在矩阵中。

AERSCREEN 是 AERMOD 建模系统的交互式包装器，根据用户对屏幕上打印的提示提供的数值计算输入值。

在运行 AERSCREEN 之前，用户应查阅并熟悉以下内容：

- AERMOD 用户指南（USEPA 2016a）
- AERMAP 用户指南（USEPA 2016b）
- AERMET 用户指南（用于表面特征表）（USEPA 2016c）
- BPIPPRM 用户指南（USEPA 2004）– 与建筑物对扩散的影响相关
- AERSURFACE 用户指南（USEPA 2008）
- AERMOD 实施指南（USEPA 2016e）

尽管接下来的两个参考资料专门针对美国，但其中包含的信息可能对扩散建模总体而言很重要。

空气质量模型指南（附录 W）（USEPA 2016d）

估算固定源空气质量影响的筛选程序（USEPA 1992）

下面提供了 AERSCREEN（版本 16216）代码的链接，包括最新的 MAKEMET 代码（版本 16216），以及文档和测试用例。MAKEMET 程序与 AERSCREEN 接口，根据用户输入生成基于站点的筛选气象条件矩阵，用于输入 AERMOD。

AERSCREEN 代码和可执行文件（v16216） (ZIP)(759 K)
MAKEMET 代码和可执行文件（v16216） (ZIP)(355 K)

请注意，在使用 AERSCREEN 之前，用户必须拥有最新的 AERMOD 可执行文件（18081）以及 AERMAP 地形预处理器和 BPIPPRM 可执行文件。这些程序可以从 AERMOD 建模系统部分下载。

## D.5.4 AERSCREEN 示例

图 D.4 是针对用于生产甲烷发电的厌氧消化器排放物分析而产生的。分析表明，甲烷中还含有硫化合物。它提供了烟囱高度对地面水平

改变烟囱高度时的筛选浓度
（10 吨/年 SO2 排放）

![](img/009e9fca58d02c8d25e9fb0817ce46c9_523_0.png)

图 D.4 随高度变化的 SO₂ 浓度。

浓度的效应示例。它还表明，一个非常小的、10 TPY 的 SO 2 源，如果烟囱很短，可能在距离烟囱数千米处超过美国标准。空气质量影响分析（AQIA）从每个污染物的初步建模开始，以确定申请人是否可以免除详细分析和建设前监测。如果给定污染物的预测环境浓度增幅低于每个平均时段的 PSD 显著影响水平（SILs），则无需对该污染物的环境影响进行进一步分析。对于具有平均时段且影响大于 SIL 的污染物，使用完整的环境影响分析（考虑其他消耗增量的源）来证明符合 PSD 增量和

NAAQS（国家环境空气质量标准）。许多 SO 2 源排放数千吨，但烟囱高度可达 300 m。AERSCREEN 可以快速提供最大地面水平影响和对环境浓度有显著贡献的距离的估计。请注意，浓度轴为对数。

图 D.5 显示了土地利用，例如水域、森林、草地，对相同 10 TPY 源的地面水平浓度的影响。它也是经验法则的一个很好的演示，即最大地面水平浓度出现在距离源大约十倍烟囱高度处，例如对于这个 10 m 烟囱高度，最大地面水平浓度出现在源下风向约 100 m 处。

![](img/009e9fca58d02c8d25e9fb0817ce46c9_524_0.png)

图 D.5 堆栈排放和土地利用的影响。

## 参考文献

- **Max/Min Temperatures**
- http://www.wrcc.dri.edu/summary/Climsmwa.html

- **Population**
- http://www.ofm.wa.gov/pop/april1/poprends.pdf

- **AERSCREEN User's Guide**
- http://www.3pa.gov/tcr/scram/dispersion_screening.htm
- http://lar.wsu.edu/nw-airquest/lookup.html

- **Background Concentration**
- http://lar.wsu.edu/nw-airquest/lookup.html

- **National Land Cover Database**
- http://www.mrlc.gov/viewerjs

- **National Elevation Database**
- http://viewer.nationalmap.gov/basic
- Also includes NLCD access

- **EPA** Support Center for Regulatory Atmospheric Modeling (SCRAM)
- https://www.epa.gov/scram/air-quality-dispersion-modelingscreening-models

USEPA (1992). Screening Procedures for Estimating the Air Quality Impact of Stationary Sources. Research Triangle Park, NC: USEPA.

USEPA (2004). User's Guide to the Building Profile Input Program, EPA-454/R-93-038. Research Triangle Park, NC: USEPA.

USEPA (2008). AERSURFACE User's Guide, EPA-454/B-08-001. Research Triangle Park, NC: USEPA.

USEPA (2016a). User's Guide for the AMS/EPA Regulatory Model – AERMOD, EPA-454/B-16-011. Research Triangle Park, NC: USEPA.

USEPA (2016b). User's Guide for the AERMOD Terrain Preprocessor (AERMAP), EPA454/B-16-012. Research Triangle Park, NC: USEPA.

USEPA (2016c). User's Guide for the AERMOD Meteorological Preprocessor (AERMET), EPA-454/B-16-010. Research Triangle Park, NC: USEPA.

USEPA (2016d). Guideline on Air Quality Models (Appendix W of 40 CFR Part 51). Research Triangle Park, NC: USEPA.

USEPA (2016e). AERMOD Implementation Guide, EPA-454/B-16013. Research Triangle Park, NC: USEPA.

USEPA (2016f). AERSCREEN User's Guide, EPA-454/B-16-004. Research Triangle Park, NC: USEPA.

## 参考文献

Crowl, D.A. and Louvar, J.F. (2011). *Chemical Process Safety*, 3e. Upper Saddle River, NJ: Prentice Hall.

## 附录 G

## 资源回收

华盛顿州斯波坎市废物能源设施，美国

## G.1 设施描述

位于华盛顿州斯波坎的资源回收和废物能源设施（WTE）可处理 800 T 市政固体废物（MSW）每日：有两个锅炉，每个额定 400 T/日。该设施每年使用约 255 000 T 垃圾。有 1 台蒸汽轮机发电机，额定 13.8 kV，30 MVA，25.8 MW，功率因数为 0.86。该设施的发电合同按季节变化，从 $38 到 $48 MWh（毛 ~1 500 000 MW/年）（C. Averyt，个人交流）。MSW 通常由卡车直接倾倒到垃圾坑中。 overhead 垃圾吊车然后将垃圾从坑中移至每个锅炉的进料漏斗。MSW 在坑中由吊车操作员混合和分拣。灰渣残留物以约 6000 T/月的速率产生。平均垃圾衍生燃料组成见表 G.1。资源回收的示意图如图 G.1 所示。

管道。斯波坎市 WTE 设施配备了两台相同的锅炉，每台锅炉都有单独的喷雾干燥吸收器（SDA）和单独的布袋过滤器（FF）。石灰制备、稀释水、灰渣去除和雾化空气系统对两台锅炉是共用的。空气污染控制系统中的主要流动路径始于锅炉省煤器处的烟气排放。烟气离开锅炉，含有多种污染物，主要是二氧化硫（SO₂）、液压酸（HCl）和颗粒灰。这些受污染的气体通过管道流向 SDA 的入口。气体进入 SDA 顶部并向下流经一系列折流板分布网络。折流板用于将气体均匀分布在 SDA 横截面上，在那里它与雾化石灰浆接触。石灰浆吸收大部分酸性气体。清洁气体流向 SDA 容器底部，在那里转向并流出 SDA 容器侧面。

### G.1.1 技术

垃圾焚烧锅炉燃烧系统设计用于以受控速率将垃圾引入炉膛，并提供必要的过火和欠火燃烧空气，以实现垃圾和可燃气体的适当燃烧。它还设计用于将灰渣惰性残留物输送到灰渣处理系统进行处置，并从换热表面去除烟灰和灰尘颗粒。垃圾焚烧锅炉燃烧系统的燃烧空气处理部分包含必要的设备和控制装置，以提供和调节供给锅炉炉膛用于垃圾燃烧的空气量。它还包含必要的设备和控制装置，以从锅炉炉膛中去除燃烧气体。燃烧系统的空气供给侧由一次和二次鼓风机、蒸汽盘管空气加热器、容积控制挡板和相关管道组成。气体去除侧由诱导通风机和相关

为消除储存大量无水氨的需要，使用尿素水解器产生用于 NOₓ 控制的 NH₃ 蒸汽。该过程使用 40–50% 重量比的尿素溶液生产氨。尿素溶液由由变频驱动电机驱动的隔膜式计量泵送入水解器。尿素溶液以可变流速泵送以保持水解器中的恒定液位。烟气在离开 SDA 时含有大量粉尘（灰渣和石灰）颗粒。该气体进入 FF，在那里分配到各个模块（或隔间）。FF 设计为六个隔间。五个隔间对于 100% 负荷是必要的，而六个隔间可在维护时离线。脉冲喷射使用压缩空气脉冲来清洁袋子。清洁周期可以由压降信号或时间周期启动。压降信号是通常的方法。总共使用了四套 Aldora Technologies, Inc. 提供的连续监测系统（CEMS）来测量来自两台市政废物焚烧炉（MWC）

