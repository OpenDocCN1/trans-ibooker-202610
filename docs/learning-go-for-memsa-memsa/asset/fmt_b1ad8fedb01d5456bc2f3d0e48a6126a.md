
## 2.
在 `ex8` 目录中，`report.go` 文件包含一个名为 `ReportBuilder` 的类型，其中有一个名为 `PriceSummary` 的方法。`ReportBuilder` 依赖于一个大型接口 (`DataStore`)，但 `PriceSummary` 仅使用了该接口中的一个方法。编写一个测试来验证 `PriceSummary` 是否工作正确。

## 3.
在 `ex9` 目录中，`weather.go` 文件包含一个名为 `Client` 的类型，其中有一个名为 `GetTemperature` 的方法，该方法连接到远程 HTTP 服务器以获取天气数据。编写一个单元测试来验证此方法，而无需运行外部服务器。

# Wrapping Up

在本章中，你已学习如何使用 Go 内置的测试支持、代码覆盖率、基准测试、模糊测试和数据竞争检查来编写测试并提高代码质量。在下一章中，你将探索一些允许你打破规则的 Go 特性：`unsafe` 包、反射和 `cgo`。

# About the Author

**Jon Bodner** has been a software engineer, lead developer, and architect for over 25 years. In that time, he has worked on software across many fields, including education, finance, commerce, healthcare, law, government, and internet infrastructure.

Jon is currently a staff engineer at Datadog, working on improving the onboarding experience for customers.

Previously, Jon was a senior distinguished engineer at Capital One, where he worked on commercialization of technology, contributed to the company’s development and testing workflow, developed patented techniques for web payment page detection and population, and coauthored tools for finding and managing software development issues.

Jon is a frequent speaker at Go conferences, and his blog posts on Go and software engineering have been viewed more than 300,000 times. He is the creator of the [Proteus data access library](https://github.com/jonbodner/proteus) and codeveloper of [checks-out](https://github.com/capitalone/checks-out), a fork of the LGTM project.
