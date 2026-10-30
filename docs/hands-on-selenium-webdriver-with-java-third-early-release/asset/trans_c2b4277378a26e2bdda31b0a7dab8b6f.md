
# 表1-9 单元测试框架

| 名称 | 语言 | 描述 | 许可证 | 维护者 | 网站 |
|---|---|---|---|---|---|
| JUnit | Java | xUnit家族的参考实现 | EPL | JUnit团队 | https://junit.org/ |
| TestNG | Java | 受JUnit和NUnit启发，包含额外功能 | Apache 2.0 | Cedric Beust | https://testng.org/ |
| Mocha | JavaScript | 用于Node.js和浏览器的测试框架 | MIT | OpenJS基金会 | https://mochajs.org/ |
| Jest | JavaScript | 专注于简洁性，重点面向Web应用程序 | MIT | Facebook | https://jestjs.io/ |
| Karma | JavaScript | 允许你在Web浏览器中执行JavaScript测试 | MIT | Karma团队 | http://karma-runner.github.io/ |
| NUnit | .Net | 用于所有.Net语言（C#、Visual Basic 和 F#）的单元测试框架 | MIT | .NET基金会 | https://nunit.org/ |
| unittest | Python | 从Python 2.1开始作为标准库包含的单元测试框架 | PSF许可证 | Python软件基金会 | https://docs.python.org/library/unittest.html |
| minitest | Ruby | 用于Ruby的完整测试工具套件 | MIT | Seattle Ruby Brigade | https://github.com/seattlers/minitest |

xUnit家族的一个重要共同特征是测试结构，由四个阶段组成（参见图1-9）：

- *设置。* 测试用例初始化被测系统（SUT）以展示预期行为。
- *执行。* 测试用例与被测系统（SUT）进行交互。因此，测试从被测系统（SUT）获得结果。
- *验证。* 测试用例判断从被测系统（SUT）获得的结果是否符合预期。为此，测试包含一个或多个断言。*断言*（或谓词）是一个布尔值函数，用于比较预期条件是否为真。断言的执行会产生测试判定（通常为通过或失败）。
- *清理。* 测试用例将被测系统（SUT）恢复到初始状态。

![](img/b67ec8e49249e340add6ed66ab413d78_50_0.png)

*图1-9 单元测试通用结构*

**提示**

我们可以将单元测试框架与其他库或工具结合使用，来实现任何测试类型。例如，正如第2章所解释的，我们使用JUnit和TestNG来嵌入对Selenium WebDriver API的调用，从而以这种方式实现Web应用的端到端测试。

在单元测试用例中，设置和清理阶段是可选的。验证步骤虽然不是严格必须的，但强烈推荐。尽管单元测试框架包含实现断言的能力，但通常会引入第三方*断言库*。这些库旨在通过提供丰富的流畅断言集来提高测试代码的可读性。此外，这些库还提供增强的错误消息，帮助测试人员理解失败的原因。表1-10包含了一些最有用的Java断言库的总结。

| Hamcrest | Java匹配器库，旨在创建灵活的断言 | BSD | Hamcrest团队 | http://hamcrest.org/ |
| Truth | 适用于Java和Android的流畅断言库 | Apache 2.0 | Google | https://truth.dev/ |

如图1-9所示，被测系统（SUT）通常可以查询另一个组件，称为*被依赖组件*（DOC）。在某些情况下（例如，在单元测试或系统测试级别），我们可能希望将SUT与DOC隔离。我们可以找到各种各样的模拟（mock）库来实现这种隔离。表1-11展示了其中一些用于Java的模拟库的全面总结。

# 表1-11 Java的模拟库

| 名称 | 级别 | 描述 | 许可证 | 维护者 | 网站 |
|---|---|---|---|---|---|
| EasyMock | 单元测试 | 允许使用Java注解为单元测试模拟对象 | Apache | EasyMock团队 | https://easymock.org/ |
| Mockito | 单元测试 | 用于模拟创建和验证的Java模拟库 | MIT | Mockito团队 | https://site.mockito.org/ |
| JMockit | 集成测试 | 允许对Java EE和基于Spring的应用进行容器外集成测试 | Open | JMockit团队 | https://jmockit.github.io/ |
| MockServer | 系统测试 | 用于通过HTTP或HTTPS与Java客户端集成的任何系统的模拟库 | Apache 2.0 | James D. Bloom | https://www.mock-server.com/ |
| WireMock | 系统测试 | 用于模拟基于HTTP的服务的工具 | Apache 2.0 | Tom Akehurst | http://wiremock.org/ |

本节中我们分析的最后一种测试工具类别与BDD相关。如你所知，BDD是一种旨在创建验收测试的开发过程。实现这种方法有很多替代方案。例如，表1-12展示了不同相关BDD框架的精简总结。

# 表12 BDD框架

| 名称 | 语言 | 描述 | 许可证 | 维护者 | 网站 |
|---|---|---|---|---|---|
| Cucumber | Ruby、Java、JavaScript、Python | 遵循BDD方法创建自动化验收测试的测试框架。 | MIT | SmartBear Software | https://cucumber.io/ |
| FitNesse | Java | 独立的协作式wiki和验收测试框架 | CPL | FitNesse团队 | http://fitnesse.org/ |
| JBehave | Java | 适用于所有JVM语言（Java、Groovy、Kotlin、Ruby、Scala）的BDD框架 | BSD-3-Clause | JBehave团队 | https://jbehave.org/ |
| Jasmine | JavaScript | 用于JavaScript代码的BDD框架 | MIT | Jasmine团队 | https://jasmine.github.io/ |
| Capybara | Ruby | 基于Web的验收测试框架，用于模拟用户故事的场景 | MIT | Thomas Walpole | https://teamcapybara.github.io/capybara |

# 总结与展望

自从2004年诞生以来，Selenium已经走过了很长的路。如今，许多从业者认为它是为Web应用开发端到端测试的事实标准解决方案，并被全球成千上万的项目使用。在本章中，你已经了解了Selenium项目的基础（由WebDriver、Grid和IDE组成）。此外，你可以注意到Selenium拥有丰富的生态系统和活跃的社区。WebDriver是Selenium项目的核心，它是一个提供API以编程方式控制不同Web浏览器（如Chrome、Firefox、Edge等）的库。表1-13全面概述了Selenium WebDriver的主要和次要用途。

表1-13 Selenium WebDriver的主要和次要用途

| | 主要用途 | 次要用途（其他用途） |
|---|---|---|
| **目的** | 自动化测试 | 网页抓取、基于Web的维护任务 |
| **测试级别** | 端到端测试 | 系统测试（模拟后端调用）<br>验收测试（例如与Cucumber一起使用） |
| **测试类型** | 功能测试（确保预期行为）<br><br>跨浏览器测试（在不同Web浏览器中的兼容性）<br><br>回归测试（确保每次提交后在CI中的构建） | 非功能测试（例如负载、安全、可访问性或本地化） |
| **测试方法论** | TLD（当SUT可用时实现测试） | BDD（在早期开发阶段定义用户场景） |

在下一章中，你将了解如何使用Maven或Gradle作为构建工具来设置一个Java项目。该项目将包含使用JUnit和TestNG作为单元测试框架并调用WebDriver API的Web应用端到端测试。此外，你将学习如何通过一个基本的测试用例（Selenium WebDriver版本的经典*Hello World*）来控制不同的Web浏览器（如Chrome、Firefox或Edge）。

# 第2章 准备测试

# 给早期发布读者的说明

> 通过早期发布电子书，你可以获得书籍最早期的形式——作者写作时的原始未编辑内容——从而在这些技术正式发布之前就加以利用。
> 
> 这将是最终图书的第2章。请注意，GitHub仓库将在稍后激活。
> 
> 如果你对如何改进本书内容和/或示例有任何评论，或者你发现本章中缺少某些材料，请联系策划编辑：rfernando@oreilly.com。

本章旨在使用Selenium WebDriver和Java语言实现你的第一个端到端测试。为此，我们首先回顾先前知识、硬件和软件方面的技术要求。其次，本章概述了如何设置一个包含Selenium WebDriver测试的Java项目。为了简化项目设置，你可以使用Maven或Gradle等构建工具。最后，你将学习使用Selenium WebDriver实现一个基本的端到端测试，即*Hello World*测试。我们将使用不同的Web浏览器（如Chrome、Edge或Firefox）和单元测试框架（JUnit和TestNG）以多种方式实现该测试。请记住，我在本书中展示的每个代码示例都可在开源的GitHub仓库中获得。因此，你可以将该仓库的内容和配置作为自己测试的基础。

## 要求

开始使用Selenium WebDriver与Java的第一个要求是理解Java语言和面向对象编程。不一定要成为专家，但需要具备基础知识。然后，你
