
如果编排是集中式的，谁来负责？当公司拥有 DataOps 团队时，编排通常归属于此团队。通常，参与服务的团队是天然的选择，因为它对数据工程生命周期的所有阶段都有相当全面的了解。这可能是 DBA、分析工程师、数据工程师或 ML 工程师。ML 工程师协调复杂的模型训练流程，但可能不愿意（或不想）在本已繁重的职责清单上，再增加管理编排的运维复杂性。

# 软件工程

与几年前相比，数据服务变得更加简单。编写代码的需求已大幅简化。数据也变得更加“代码优先”，随着专注于简化数据服务的开源框架激增。存在许多向最终用户提供数据的方式，数据工程师的重点应放在了解这些系统如何工作以及数据如何交付上。

尽管数据服务变得简单，但如果涉及代码，数据工程师仍应了解主要服务接口的工作原理。例如，数据工程师可能需要将数据科学家在笔记本上本地运行的代码进行转换，并将其转化为报表或基础 ML 模型以供运行。

数据工程师大有用武之地的另一个领域是理解代码和查询针对存储系统运行时的性能影响。分析师可以通过多种编程方式生成 SQL，包括 LookML、dbt 中的 Jinja、各种对象关系映射 (ORM) 工具和指标层。当这些编程层编译为 SQL 时，该 SQL 的性能如何？数据工程师可以在 SQL 代码性能不如手写 SQL 时提出优化建议。

分析和 ML 基础设施即代码的兴起，意味着编写代码的角色正转向构建支持数据科学家和分析师的系统。数据工程师可能负责为其数据团队设置 CI/CD 流水线并构建流程。他们最好还能培训和支持其数据团队使用他们构建的 Data/MLOps 基础设施，以使这些数据团队尽可能实现自助服务。

对于嵌入式分析，数据工程师可能需要与应用开发人员合作，以确保查询能快速且具成本效益地返回。应用开发人员将控制用户交互的前端代码。数据工程师的职责是确保开发人员在请求时能收到正确的数据载荷。

# 结论

数据工程生命周期在服务阶段有一个合乎逻辑的终点。与所有生命周期一样，会发生反馈循环（[图 9-6](#ch09.html#buildcomma_learncomma_improve)）。你应该将服务阶段视为了解什么有效、什么可以改进的机会。倾听利益相关者的声音。如果他们提出问题——他们必然会提出——尽量不要介怀。相反，将其作为改进你所构建事物的机会。

![](assets/fode_0906.png)

优秀的数据工程师始终乐于接受新的反馈，并不断寻找改进技艺的方法。现在我们已经走完了数据工程生命周期的旅程，你已了解如何设计、架构、构建、维护和改进你的数据工程系统和产品。让我们把目光转向本书的 [第三部分](#part03.html#part3)，我们将介绍数据工程中一些我们被持续询问且坦白说值得更多关注的方面。

# 附加资源

*   [“Data as a Product vs. Data Products: What Are the Differences?”](https://oreil.ly/fRAA5) by Xavier Gumara Rigol
*   [“Data Jujitsu: The Art of Turning Data into Product”](https://oreil.ly/5TH6Q) by D. J. Patil
*   [*Data Mesh*](https://oreil.ly/VjniC) by Zhamak Dehghani (O’Reilly)
*   [“Data Mesh Principles and Logical Architecture”](https://oreil.ly/JqaW6) by Zhamak Dehghani
*   [“Designing Data Products”](https://oreil.ly/BKqu4) by Seth O’Regan
*   [“The Evolution of Data Products”](https://oreil.ly/DNk8x) and [“What Is Data Science”](https://oreil.ly/xWL0w) by Mike Loukides
*   Forrester’s [“Self-Service Business Intelligence: Dissolving the Barriers to Creative Decision-Support Solutions” blog article](https://oreil.ly/c3bpO)
*   [“Fundamentals of Self-Service Machine Learning”](https://oreil.ly/aALpB) by Paramita (Guha) Ghosh
*   [“The Future of BI Is Headless”](https://oreil.ly/INa17) by ZD
*   [“How to Build Great Data Products”](https://oreil.ly/9cI55) by Emily Glassberg Sands
*   [“How to Structure a Data Analytics Team”](https://oreil.ly/mGtii) by Niall Napier
*   [“Know Your Customers’ ‘Jobs to Be Done’”](https://oreil.ly/1W1JV) by Clayton M. Christensen et al.
*   [“The Missing Piece of the Modern Data Stack”](https://oreil.ly/NYs1A) and [“Why Is Self-Serve Still a Problem?”](https://oreil.ly/0vYvs) by Benn Stancil
*   [“Self-Service Analytics” in the Gartner Glossary](https://oreil.ly/NG1yA)
*   Ternary Data’s [“What’s Next for Analytical Databases? w/ Jordan Tigani (MotherDuck)” video](https://oreil.ly/8C4Gj)
*   [“Understanding the Superset Semantic Layer”](https://oreil.ly/YqURr) by Srini Kadamati
*   [“What Do Modern Self-Service BI and Data Analytics Really Mean?”](https://oreil.ly/Q9Ux8) by Harry Dix
*   [“What Is Operational Analytics (and How Is It Changing How We Work with Data)?”](https://oreil.ly/5yU4p) by Sylvain Giuliani
*   [“What Is User-Facing Analytics?”](https://oreil.ly/HliJe) by Chinmon Soman

^([1](#ch09.html#idm45075121881520-marker)) Quoted in Benjamin Snyder, “7 Insights from Legendary Investor Warren Buffett,” CNBC *Make It*, May 1, 2017, [*https://oreil.ly/QEqF9*](https://oreil.ly/QEqF9).

^([2](#ch09.html#idm45075121845120-marker)) D. J. Patil, “Data Jujitsu: The Art of Turning Data into Product,” *O’Reilly Radar*, July 17, 2012, [*https://oreil.ly/IYS9x*](https://oreil.ly/IYS9x).

^([3](#ch09.html#ch01fn80-marker)) Clayton M. Christensen et al., “Know Your Customers’ ‘Jobs to Be Done,’” *Harvard Business Review*, September 2016, [*https://oreil.ly/3uU4j*](https://oreil.ly/3uU4j).

^([4](#ch09.html#ch01fn81-marker)) Benn Stancil, “The Missing Piece of the Modern Data Stack,” *benn.substack*, April 22, 2021, [*https://oreil.ly/wQyPb*](https://oreil.ly/wQyPb).

^([5](#ch09.html#ch01fn82-marker)) Srini Kadamati, “Understanding the Superset Semantic Layer,” Preset blog, December 21, 2021, [*https://oreil.ly/6smWC*](https://oreil.ly/6smWC).

# 第三部分. 安全、隐私与数据工程的未来

# 第 10 章. 安全与隐私

既然你已经了解了数据工程生命周期，我们想重申安全的重要性，并分享一些你可以融入日常工作流的简单实践。安全是数据工程实践的关键。这显而易见，但我们不断惊讶于数据工程师将安全视为事后诸葛亮的频率。我们认为，安全是数据工程师在工作的各个方面和数据工程生命周期的每个阶段首先需要考虑的事情。你每天都在处理敏感数据、信息和访问权限。你的组织、客户和业务合作伙伴期望这些宝贵资产得到最周全的照料和关注。一次安全漏洞或数据泄露就可能让你的业务陷入瘫痪；如果是你的过错，你的职业生涯和声誉将毁于一旦。

安全是隐私的关键要素。隐私长期以来一直是企业信息技术领域信任的关键；工程师直接或间接处理与人们私人生活相关的数据。这包括财务信息、私人通信数据（电子邮件、短信、电话通话）、病历、教育记录和工作履历。泄露或滥用这些信息的公司，一旦曝光，可能会成为众矢之的。
