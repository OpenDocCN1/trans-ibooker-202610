
物联网的一个重要服务模式类似于反向 ETL（[图 3-17](#ch03.html#iot_serving_pattern_for_downstream_use)），尽管我们倾向于不在物联网语境中使用该术语。设想这样一个场景：来自制造设备上传感器的数据被收集并分析。这些测量结果经过处理以寻找优化方案，使设备能更高效地运行。数据被发送回去以重新配置设备并对其进行优化。

![](assets/fode_0317.png)

### 仅触及物联网表面

物联网场景极其复杂，物联网架构和系统对于可能将职业生涯花在处理业务数据上的数据工程师来说也比较陌生。我们希望这一介绍能鼓励感兴趣的数据工程师深入了解这个迷人且快速发展的专业领域。

## 数据网格

*数据网格*是针对蔓延的单体数据平台（如中心化数据湖和数据仓库）以及“数据的巨大鸿沟”（即数据领域被划分为运营数据和分析数据）的一种近期回应。^([24](#ch03.html#ch01fn30)) 数据网格试图颠覆中心化数据架构的挑战，采用领域驱动设计的概念（常用于软件架构）并将其应用于数据架构。由于数据网格近期备受关注，你应该了解它。

数据网格的很大一部分是去中心化，正如 Zhamak Dehghani 在其关于该主题的开创性文章中所指出的：^([25](#ch03.html#ch01fn31))

> 为了去中心化单体数据平台，我们需要改变对数据、其位置和所有权的看法。不再将数据从域流向中心拥有的数据湖或平台，而是需要由域以易于消费的方式托管和服务其领域数据集。

Dehghani 后来确定了数据网格的四个关键组成部分：^([26](#ch03.html#ch01fn32))

* 面向领域的去中心化数据所有权和架构
* 数据即产品
* 自助式数据基础设施即平台
* 联邦计算治理

[图 3-18](#ch03.html#simplified_example_of_a_data_mesh_archi) 展示了数据网格架构的简化版本。你可以在 Dehghani 的著作 [*Data Mesh*](https://oreil.ly/mK8cJ)（O’Reilly 出版）中了解更多关于数据网格的信息。

![](assets/fode_0318.png)

## 其他数据架构示例

数据架构还有无数其他变体，例如数据织构、数据枢纽、[可扩展架构](https://oreil.ly/MB1Ap)、[元数据优先架构](https://oreil.ly/YkA9e)、事件驱动架构、实时数据栈（[第 11 章](#ch11.html#the_future_of_data_engineering)）等。随着实践的巩固成熟和工具的简化改进，新架构将持续涌现。我们重点介绍了少数最关键的数据架构模式，它们要么极其成熟，要么快速演进，要么两者兼具。

作为数据工程师，要关注新架构如何帮助你的组织。通过培养对数据工程生态系统发展的高层认知，保持对新进展的了解。保持开放心态，不要对某一方法产生情感依赖。一旦识别出潜在价值，深入学习并做出具体决策。如果做得正确，对数据架构的微调——或大规模改造——都能对业务产生积极影响。

## 谁参与数据架构设计？

数据架构不是在真空中设计的。大公司可能仍会雇佣数据架构师，但这些架构师需要紧跟技术和数据的现状。象牙塔式数据架构的时代已经一去不复返。过去，架构在很大程度上与工程正交。我们预计随着数据工程乃至整个工程领域的快速演进，变得更加敏捷，工程与架构之间的界限将消失，这种区别将不复存在。

理想情况下，数据工程师将与专职数据架构师共事。然而，如果公司规模较小或数据成熟度较低，数据工程师可能需要身兼数职，同时担任架构师。因为数据架构是数据工程生命周期的潜流，数据工程师应了解“良好”的架构以及各种类型的数据架构。

在设计架构时，你将与业务利益相关者一起评估权衡。采用云数据仓库与数据湖相比有哪些固有权衡？各种云平台有哪些权衡？何时统一批/流框架（Beam、Flink）会是合适的选择？在抽象层面研究这些选择将帮助你做出具体、有价值的决策。

## 总结

你已了解数据架构如何融入数据工程生命周期，什么构成了“良好”的数据架构，并看到了几个数据架构示例。因为架构是成功的关键基础，我们鼓励你投入时间深入研究它，并理解任何架构中固有的权衡。你将做好准备，规划出符合组织独特需求的架构。

接下来，让我们看看一些在数据架构和整个数据工程生命周期中选择合适技术的方法。

## 附加资源

* [“贫血领域模型”](https://oreil.ly/Bx8fF) by Martin Fowler
* [“大数据架构”](https://oreil.ly/z7ZQY) Azure 文档
* [“限界上下文”](https://oreil.ly/Hx3dv) by Martin Fowler
* [“大数据两种处理架构简介——Lambda 和 Kappa”](https://oreil.ly/CcmZi) by Iman Samizadeh
* [“现代数据平台的构建模块”](https://oreil.ly/ECuIW) by Prukalpa
* [“明智选择开源”](https://oreil.ly/79pNh) by Benoit Dageville 等
* [“选择适合全球数据分发的架构”](https://oreil.ly/mGkrg) Google Cloud Architecture 网页
* [“面向列的 DBMS 维基百科页面”](https://oreil.ly/pG4DJ)
* [“数据处理框架对比”](https://oreil.ly/XSM7H) by Ludovik Santos
* [“云的成本，万亿美元悖论”](https://oreil.ly/8wBqr) by Sarah Wang and Martin Casado
* [“数据湖怪兽的诅咒”](https://oreil.ly/UdFHa) by Kiran Prakash and Lucy Chambers
* [*数据架构：数据科学家入门*](https://oreil.ly/dbiK0) by W. H. Inmon 等（Academic Press）
* [“数据架构：复杂与繁杂”](https://oreil.ly/akjNd) by Dave Wells
* [“数据即产品与数据即服务”](https://oreil.ly/6svBK) by Justin Gage
* [“数据二分法：重新思考我们对待数据和服务的方式”](https://oreil.ly/Bk4dV) by Ben Stopford
* [“数据织构架构是现代化数据管理和集成的关键”](https://oreil.ly/qQf3z) by Ashutosh Gupta
* [“数据织构定义”](https://oreil.ly/ECpAG) by James Serra
* [“数据团队平台”](https://oreil.ly/SkDj0) by GitLab Data
* [“数据仓库架构：概览”](https://oreil.ly/pzGKb) by Roelant Vos
* [“Javatpoint 上的数据仓库架构教程”](https://oreil.ly/XgwiO)
* [“定义架构 ISO/IEC/IEEE 42010 网页”](https://oreil.ly/CJxom)
* [“现代面向列数据库系统的设计与实现”](https://oreil.ly/Y93uf) by Daniel Abadi 等
* [“我在微服务世界见过的灾难”](https://oreil.ly/b1TWh) by Joao Alves
* [“领域驱动设计”](https://oreil.ly/nyMrw) by Martin Fowler
* [“消除管道债务：引入 Great Expectations”](https://oreil.ly/EgVav) by the Great Expectations project
* [*EABOK* 草案](https://oreil.ly/28yWO), edited by Paula Hagan
* [EABOK 网站](https://eabok.org)
