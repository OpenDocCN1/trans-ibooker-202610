

## 2.
在 `ex8` 目录中，`report.go` 文件包含一个名为 `ReportBuilder` 的类型，其中有一个名为 `PriceSummary` 的方法。`ReportBuilder` 依赖于一个大型接口 (`DataStore`)，但 `PriceSummary` 仅使用了该接口中的一个方法。编写一个测试来验证 `PriceSummary` 是否工作正确。

## 3.
在 `ex9` 目录中，`weather.go` 文件包含一个名为 `Client` 的类型，其中有一个名为 `GetTemperature` 的方法，该方法连接到远程 HTTP 服务器以获取天气数据。编写一个单元测试来验证此方法，而无需运行外部服务器。

# Wrapping Up

在本章中，你已学习如何使用 Go 内置的测试支持、代码覆盖率、基准测试、模糊测试和数据竞争检查来编写测试并提高代码质量。在下一章中，你将探索一些允许你打破规则的 Go 特性：`unsafe` 包、反射和 `cgo`。

# About the Author

**Jon Bodner** 在过去 25 多年里一直担任软件工程师、首席开发人员和架构师。在此期间，他从事过教育、金融、商业、医疗保健、法律、政府和互联网基础设施等多个领域的软件开发工作。

Jon 目前是 Datadog 的员工工程师，负责改善客户的入职体验。

此前，Jon 在 Capital One 担任高级杰出工程师，负责技术商业化、为公司开发和测试工作流程做出贡献、开发了用于检测和填充网页支付页面的专利技术，并共同撰写了用于查找和管理软件开发问题的工具。

Jon 是 Go 大会的常客，他在 Go 和软件工程方面的博客文章已被浏览超过 300,000 次。他是 [Proteus 数据访问库](https://github.com/jonbodner/proteus) 的创建者，也是 [checks-out](https://github.com/capitalone/checks-out) 的共同开发者，这是一个 LGTM 项目的分支。

