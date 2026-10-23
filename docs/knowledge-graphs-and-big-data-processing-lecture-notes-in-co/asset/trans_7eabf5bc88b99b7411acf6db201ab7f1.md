
应被纳入考虑，因为该位置决定了气候条件的季节性变化将有多严重。至于静态数据，总楼面面积或供暖体积也被认为与总消耗量密切相关，但无法用于以高时间分辨率动态估算需求。这时，大量直接从家庭收集的物联网传感器数据对于提高预测模型的精度可以发挥很大的作用。也就是说，室内环境温度与室外气象条件相结合，再加上实时的占用数据，可以提供对消费曲线的精确短期估算。此外，如果将过去的行为纳入考虑（以先前需求曲线的形式，既包括过去较长时间段的平均值，也包括前几天的较新数据），并结合当日指标（即是否为工作日或周末/节假日），就可以生成相对精确的逐小时甚至亚小时级的曲线。

以智能插头和智能线缆形式存在的智能测量设备能够报告家庭中每个电器的实时消耗量，或者用第3.1小节中描述的NILM算法替代它们（在这种替代方案中，因泛化不足导致的性能不佳不是问题），这为在单个电器层面预测需求提供了可能性。这种方法在当代研究文章中很少被描述，只有像[28,312]和[226]等少数论文探讨了这一主题。另一方面，需求预测问题最常从聚合的角度来处理，即通过预测社区、城市或州级消费来实现，而数据的可用性通常是最终决定将估算何种需求类型的驱动因素。图4、图5和图6中的时间序列展示了来自单个电器、一个家庭的所有电器以及多个聚合家庭的需求信号的不同动态。由于这些应用通常需要不同的预测精度水平，用于这些图示的原始数据按照适当的使用场景采用了不同的采样间隔（15秒、60秒和15分钟）进行平均。

![](img/c21ce6788ba392bc08a8057d8b20603e_179_0.png)

图4. 采用15秒平均值的典型洗衣机需求曲线（显示在4小时的时间跨度内似乎有两次启动）

![](img/c21ce6788ba392bc08a8057d8b20603e_180_0.png)

图5. 采用60秒平均值的家庭总需求曲线（显示在完整的24小时期间内有多次电器启动）

![](img/c21ce6788ba392bc08a8057d8b20603e_180_1.png)

图6. 采用15分钟平均值的聚合家庭需求曲线（显示采用分时电价的两天消费情况）

## 4.2 产量预测器

前面已经提到，能源规划对于电网稳定性至关重要，并且它在很大程度上依赖于可再生能源（RES）产量的预测。因此，本小节将涵盖用于产量预测的不同方法，以及它们与大数据领域的关系。

RES技术的产量在很大程度上受天气条件的影响。例如，光伏（PV）发电量与太阳辐射之间存在非常高的依赖性，这与风力涡轮机与风速和风向之间的关系类似。表1给出了所选天气服务及其主要特征。也就是说，根据实际应用的不同，产量预测器可以具有不同的时间分辨率和预测范围，这决定了所需的气象预报参数。因此，从上述表格可以看出，Darksky可以提供以分钟为单位的估算，而其预测范围作为一种折衷，仅有7天。此外，根据所采用方法的不同，历史天气数据对于训练过程可能是必要的，因为目前最流行的方法

## 4.3 价格预测

预测算法在能源领域的另一个重要应用是价格预测。随着全球能源行业日益放松管制，能源交易中的可变定价正变得越来越突出，一些人设想在不远的将来，批发市场甚至零售市场的能源成本将每15分钟变化一次，而如今的标准通常最多是按小时变化。对批发市场价格进行准确预测为大型能源交易商提供了关键信息，因为它能够像股价预测一样提供对未来趋势的洞察，并允许进行合理的投资规划。

批发价格的变动极大地影响了零售价格，而零售价格反过来又对最终用户预期需求曲线的形状产生关键影响。从固定定价转向首次分时电价，再到后来的按小时可变定价，使得能源零售商能够通过本质上属于隐式需求响应（DR）的机制对负荷水平进行精细控制，其中负荷增加或减少事件由当前价格决定。能源价格还受到可再生能源可用性的影响。例如，光伏渗透率高的系统在正午的发电高峰期往往价格较低，以试图激励用户在系统出现盈余时消耗更多能源。这样，需求预测、产量预测和价格预测相互关联，从而形成一个供需平衡的均衡系统。

## 5 结论

本章的简要概述提供了对大数据工具和分析技术在能源领域的一些潜在应用的洞察。随着减缓气候变化的重要性与日俱增，致力于提高能源效率和负责任能源使用的解决方案数量只会不断增加。因此，该领域为新颖的研究方法提供了一个有趣且具有挑战性的领域。

**开放获取** 本章依据知识共享署名4.0国际许可协议（http://creativecommons.org/licenses/by/4.0/）的条款获得许可，该协议允许以任何媒介或格式进行使用、共享、改编、分发和复制，前提是您适当注明原作者和来源，提供知识共享许可协议的链接，并注明是否进行了修改。

本章中的图像或其他第三方材料包含在本章的知识共享许可协议范围内，除非在材料的署名行中另有说明。如果材料未包含在本章的知识共享许可协议中，并且您的预期使用不受法律法规允许或超出允许的使用范围，您需要直接获得版权所有者的许可。

![](img/c21ce6788ba392bc08a8057d8b20603e_183_0.png)

## 参考文献

- 1. OZON Open Source Projects: A distributed, reliable key-value store for the most critical data of a distributed system (2014)
- 2. CGI Inc.: Internet of things (2015). https://www.cgi-group.co.uk/sites/default/files/files.uk/brochures/uk_iot_overview_brochure.nov.15.pdf
- 3. Abadi, M.J.S., Zamanifar, K.: Producing complete modules in ontology partitioning. In: 2011 International Conference on Semantic Technology and Information Retrieval, pp. 137–143. IEEE (2011)
- 4. Abdelaziz, L., Mansour, E., Ouzzani, M., Abounaga, A., Kalnis, P.: Lusail: a system for querying linked data at scale. Proc. VLDB Endowment **11**(4), 485–498 (2017)
- 5. Acosta, M., Hartig, O., Sequeda, J.F.: Federated RDF query processing. In: Sakr, S., Zomaya, A.Y. (eds.) Encyclopedia of Big Data Technologies. Springer, Cham (2019). https://doi.org/10.1007/978-3-319-77525-8
- 6. Acosta, M., Vidal, M., Lampo, T., Castillo, J., Ruckhaus E.: ANAPSID: an adaptive query processing engine for SPARQL endpoints. In: The Semantic Web - ISWC 2011–10th International Semantic Web Conference, Bonn, Germany, 23–27 October 2011, Proceedings Part I, pp. 18–34 (2011)
- 7. Agarwal, B.L.: Basic Statistics, 6th edn. New Age International, New Delhi (2015)
- 8. Al Hasan, M., Zaki, M.J.: A survey of link prediction in social networks. In: Aggarwal, C. (ed.) Social Network Data Analytics, pp. 243–275. Springer, Boston (2011). https://doi.org/10.1007/978-1-4419-8462-3_9
- 9. Al-Musaylh, M.S., Deo, R.C., Adamowski, J.F., Li, Y.: Short-term electricity demand forecasting with MARS, SVR and ARIMA models using aggregated demand data in queensland, australia. Adv. Eng. Inform. **35**, 1–16 (2016)
- 10. Alexander, K., Cyganiak, R., Hausenblas, M., Zhao, J.: Describing linked datasets. In: Proceedings of the WWW2009 Workshop on Linked Data on the Web, LDOW 2009, Madrid, Spain, 20 April 2009 (2009)
- 11. Alharthi, H.: Healthcare predictive analytics: an overview with a focus on Saudi Arabia. J. Infection Public Health **11**(6), 749–756 (2018)
- 12. Ali, M., Iqbal, M.J., Sharif, M.: Relationship between extreme temperature and electricity demand in Pakistan. Int. J. Energy Environ. Eng. **4**(1), 36 (2013)
- 13. Allen, J.F., Frisch, A.M.: What's in a semantic network? In: Proceedings of the 20th Annual Meeting on Association for Computational Linguistics, pp. 19–27. Association for Computational Linguistics (1982)
- 14. Aluc, G., Ozu, M.T., Daudjee, K., Hartig, O.: Chameleon-DB: a workload-aware robust RDF data management system. University of Waterloo, Technical report CS-2013-10 (2013)
- 15. Amin, A., Al-Obiedat, F., Shah, B., Adnan, A., Loo, J., Anwar, S.: Customer churn prediction in telecommunication industry using data certainty. J. Bus. Res. **94**, 290–301 (2019)
- 16. Ancin, D., Almirall, G.: 5G to account for 15 percentage of global mobile industry by 2025 as 5G network launches accelerate (2019)
- 17. Angeli, G., Manning, C.: Philosophers are mortal: inferring the truth of unseen facts. In: Proceedings of the Seventeenth Conference on Computational Natural Language Learning, pp. 133–142 (2013)
