
* [“Caching in Snowflake Data Warehouse” Snowflake Community page](https://oreil.ly/opMFi)
* [“Data Warehouse: The Choice of Inmon vs. Kimball”](https://oreil.ly/pjuuz) by Ian Abramson
* *The Data Warehouse Toolkit* by Ralph Kimball and Margy Ross (Wiley)
* [“Data Vault—An Overview”](https://oreil.ly/Vxsm6) by John Ryan
* [“Data Vault 2.0 Modeling Basics”](https://oreil.ly/DLvaI) by Kent Graziano
* [“A Detailed Guide on SQL Query Optimization” tutorial](https://oreil.ly/WNate) by Megha
* [“Difference Between Kimball and Inmon”](https://oreil.ly/i8Eki) by manmeetjuneja5
* [“Eventual vs. Strong Consistency in Distributed Databases”](https://oreil.ly/IU3H1) by Saurabh.v
* [“The Evolution of the Corporate Information Factory”](https://oreil.ly/j0pRS) by Bill Inmon
* Gavroshe USA’s [“DW 2.0” web page](https://oreil.ly/y1lgO)
* Google Cloud’s [“Using Cached Query Results” documentation](https://oreil.ly/lGNHw)
* Holistics’ [“Cannot Combine Fields Due to Fan-Out Issues?” FAQ page](https://oreil.ly/r5fjk)
* [“How a SQL Database Engine Works,”](https://oreil.ly/V0WkU) by Dennis Pham
* [“How Should Organizations Structure Their Data?”](https://oreil.ly/00d2b) by Michael Berk
* [“Inmon or Kimball: Which Approach Is Suitable for Your Data Warehouse?”](https://oreil.ly/ghHPL) by Sansu George
* [“Introduction to Data Vault Modeling” document,](https://oreil.ly/3rrU0) compiled by Kent Graziano and Dan Linstedt
* [“Introduction to Data Warehousing”](https://oreil.ly/RpmFV), [“Introduction to Dimensional Modelling for Data Warehousing”](https://oreil.ly/N1uUg), and [“Introduction to Data Vault for Data Warehousing”](https://oreil.ly/aPDUx) by Simon Kitching
* Kimball Group’s [“Four-Step Dimensional Design Process”](https://oreil.ly/jj2wI), [“Conformed Dimensions”](https://oreil.ly/A9s6x), and [“Dimensional Modeling Techniques”](https://oreil.ly/EPzNZ) web pages
* [“Kimball vs. Inmon vs. Vault” Reddit thread](https://oreil.ly/9Kzbq)
* [“Modeling of Real-Time Streaming Data?” Stack Exchange thread](https://oreil.ly/wC9oD)
* [“The New ‘Unified Star Schema’ Paradigm in Analytics Data Modeling Review”](https://oreil.ly/jWFHk) by Andriy Zabavskyy
* Oracle’s [“Slowly Changing Dimensions” tutorial](https://oreil.ly/liRfT)
* ScienceDirect’s [“Corporate Information Factory” web page](https://oreil.ly/u2fNq)
* [“A Simple Explanation of Symmetric Aggregates or ‘Why on Earth Does My SQL Look Like That?’”](https://oreil.ly/7CD96) by Lloyd Tabb
* [“Streaming Event Modeling”](https://oreil.ly/KQwMQ) by Paul Stanton
* [“Types of Data Warehousing Architecture”](https://oreil.ly/gHEJX) by Amritha Fernando
* [US patent for “Method and Apparatus for Functional Integration of Metadata”](https://oreil.ly/C3URp)
* Zentut’s [“Bill Inmon Data Warehouse” web page](https://oreil.ly/FvZ6K)

^([1](#ch08.html#ch01fn62-marker)) 例如，参见 Emin Gün Sirer，“NoSQL Meets Bitcoin and Brings Down Two Exchanges: The Story of Flexcoin and Poloniex,” *Hacking, Distributed*, 2014 年 4 月 6 日，[*https://oreil.ly/RM3QX*](https://oreil.ly/RM3QX)。

^([2](#ch08.html#ch01fn63-marker)) 有些 [Redshift configurations](https://oreil.ly/WgLcV) 依赖对象存储。

^([3](#ch08.html#ch01fn64-marker)) 作者知晓一起事件：一家大型连锁杂货店的新分析师在生产数据库上运行 `SELECT *`，导致关键库存数据库瘫痪了三天。

^([4](#ch08.html#ch01fn65-marker)) [图 8-11](#ch08.html#an_architecture_to_join_streams_buffers) 及其描述的示例主要基于 Tathagata Das 和 Joseph Torres 的 [“Introducing Stream—Stream Joins in Apache Spark 2.3”](https://oreil.ly/LG4EK)（Databricks *Engineering Blog*，2018 年 3 月 13 日）。

^([5](#ch08.html#ch01fn66-marker)) 关于 DRY 原则的更多细节，请参阅 David Thomas 和 Andrew Hunt 合著的 *The Pragmatic Programmer*（Addison-Wesley Professional，2019）。

^([6](#ch08.html#ch01fn67-marker)) E. F. Codd, “Further Normalization of the Data Base Relational Model,” IBM Research Laboratory (1971), [*https://oreil.ly/Muajm*](https://oreil.ly/Muajm)。

^([7](#ch08.html#ch01fn68-marker)) H. W. Inmon, *Building the Data Warehouse* (Hoboken: Wiley, 2005)。

^([8](#ch08.html#ch01fn69-marker)) Inmon, *Building the Data Warehouse*。

^([9](#ch08.html#ch01fn70-marker)) Inmon, *Building the Data Warehouse*。

^([10](#ch08.html#ch01fn71-marker)) 虽然维度和事实常与 Kimball 联系在一起，但它们最早用于 1960 年代的 General Mills 和 Dartmouth University，并在 Nielsen 和 IRI 等公司得到早期采用。

^([11](#ch08.html#ch01fn72-marker)) Data Vault 有 1.0 和 2.0 两个版本。本节重点介绍 Data Vault 2.0，但为简洁起见，我们将其称为 *Data Vault*。

^([12](#ch08.html#idm45075122362304-marker)) Kent Graziano, “Data Vault 2.0 Modeling Basics,” Vertabelo, October 20, 2015, [*https://oreil.ly/iuW1U*](https://oreil.ly/iuW1U)。

^([13](#ch08.html#ch01fn73-marker)) Bill Inmon, “Avoiding the Horrible Task of Integrating Data,” LinkedIn Pulse, March 24, 2022, [*https://oreil.ly/yLb71*](https://oreil.ly/yLb71)。

^([14](#ch08.html#ch01fn74-marker)) Alex Woodie, “Lakehouses Prevent Data Swamps, Bill Inmon Says,” Datanami, June 1, 2021, [*https://oreil.ly/XMwWc*](https://oreil.ly/XMwWc)。

^([15](#ch08.html#ch01fn75-marker)) 我们提醒您负责任地使用 UDF。SQL UDF 通常性能相当不错。我们见过 JavaScript UDF 将查询时间从几分钟增加到数小时。

^([16](#ch08.html#ch01fn76-marker)) Michael Blaha, “Be Careful with Derived Data,” Dataversity, December 5, 2016, [*https://oreil.ly/garoL*](https://oreil.ly/garoL)。

^([17](#ch08.html#ch01fn77-marker)) Benn Stancil, “The Missing Piece of the Modern Data Stack,” *benn.substack*, April 22, 2021, [*https://oreil.ly/GYf3Z*](https://oreil.ly/GYf3Z)。

^([18](#ch08.html#ch01fn78-marker)) “What Is the Difference Between Apache Spark and Hadoop MapReduce?,” Knowledge Powerhouse YouTube video, May 20, 2017, [*https://oreil.ly/WN0eX*](https://oreil.ly/WN0eX)。

^([19](#ch08.html#ch01fn79-marker)) 关于流式 DAG 概念的详细应用，请参阅 Simba Khadder 的 “Why We Moved from Apache Kafka to Apache Pulsar”，StreamNative blog, April 21, 2020, [*https://oreil.ly/Rxfko*](https://oreil.ly/Rxfko)。

# 第 9 章 为分析、机器学习和反向 ETL 提供数据服务

恭喜！您已到达数据工程生命周期的最后阶段——为下游用例提供数据服务（参见 [图 9-1](#ch09.html#serving_delivers_data_for_use_cases)）。在本章中，您将了解作为数据工程师将遇到的三种主要用例的各种数据服务方式。首先，您将为分析和 BI 提供数据服务。您将为统计分析、报告和仪表板准备数据。这是数据服务最传统的领域。可以说，它早于 IT 和数据库，但对于利益相关者深入了解业务、组织和财务流程而言，它依然至关重要。

![](assets/fode_0901.png)

其次，您将为 ML 应用提供数据服务。没有高质量、经过适当准备的数据，ML 就无从谈起。数据工程师与数据科学家和 ML 工程师合作，获取、转换并交付模型训练所需的数据。

第三，您将通过反向 ETL 提供数据服务。*反向 ETL* 是将数据送回数据源的过程。例如，我们可能从广告技术平台获取数据，对该数据运行统计流程以确定每次点击成本（CPC）出价，然后将该数据反馈回广告技术平台。反向 ETL 与 BI 和 ML 高度交织。

在深入探讨这三种主要数据服务方式之前，让我们先看一些通用考量因素。

## 为数据服务的通用考量因素
