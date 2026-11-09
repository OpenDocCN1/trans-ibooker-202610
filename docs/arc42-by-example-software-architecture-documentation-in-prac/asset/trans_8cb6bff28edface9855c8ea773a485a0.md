

// 以及图像被复制到构建目录

    dependsOn asciidoctor

    sourceDir = new File("${buildDir}/html5")

// 要检查的文件，以集合表示法表示

sourceDocuments = ["many-errors.html", "no-errors.html"]

// 如果遇到任何错误则使构建失败

failOnErrors = false

// 将 HTTP 连接超时设置为 2 秒

httpConnectionTimeout = 2000

ignoreLocalHost = false

ignoreIPAddresses = false

    }

    defaultTasks 'htmlSanityCheck'
```
## II.8 横切概念
应解释横切关注点以及系统的普遍规则。`arc42`称之为概念：它们通常影响多个构建块，并在系统的多个部分及其实现中相关。示例包括技术和/或框架的使用规则以及实现规则，以及所使用的设计或架构模式。

### 8.1 领域模型
下图显示了一个领域模型：

![图 2.20：领域检查模型](image/B15546_02_20.jpg)

###### 图 2.20：领域检查模型

实现类的属性是私有的，因为我们通过 getter/setter 方法来操作它们。

下表描述了领域模型中的每个组件：

![图 2.21：HTML 领域检查模型中术语的描述](image/B15546_02_21.jpg)

###### 图 2.21：HTML 领域检查模型中术语的描述

### 8.2 HTML 链接的结构
对于许多网页开发者或 HTML 专家来说，以下关于 URI 语法的信息可能完全显而易见。我们为不同类型的人编写了这本书，因此以防万一我们包含了这些信息。

`HtmlSC` 对 HTML 链接（超链接）执行各种检查，这些链接通常遵循 RFC-2396 指定的 URI 语法（[https://www.ietf.org/rfc/rfc2396.txt](https://www.ietf.org/rfc/rfc2396.txt)）。URI 通常用于提供对任意资源（文档、文件或文档内的部分）的链接。

其通用结构在下图中描述。随后是一个单元测试：

![图 2.22：通用 URI 结构](image/B15546_02_22.jpg)

###### 图 2.22：通用 URI 结构

测试通用 URI 语法

以下代码片段是用于通用 URI 语法的一些测试代码：
```
@Test

public void testGenericURISyntax() {

// based upon an example from the Oracle(tm) Java tutorial:

// https://docs.oracle.com/javase/tutorial/networking/urls/urlInfo.\\

html

def  aURL  =  new  URL(

"https://example.com:42/docs/tutorial/index.html?name=aim42#INT\\

     RO");

    aURL.with  {

assert getProtocol() == "http"

assert getAuthority() == "example.com:42"

assert getHost() == "example.com"

assert getPort() == 42

assert getPath() == "/docs/tutorial/index.html"

assert getQuery() == "name=aim42"

assert getRef() == "INTRO"

    }

    }
```

### 8.3 多种检查算法
`HtmlSC` 使用模板方法模式（[https://sourcemaking.com/design_patterns/template_method/](https://sourcemaking.com/design_patterns/template_method/)）来实现灵活的检查算法：

"The Template Method defines *a skeleton of an algorithm* in an operation and defers some steps to subclasses."

我们通过在一个操作（`performCheck`）中定义检查算法的骨架来实现这一点，从而将具体的检查算法步骤推迟到子类。不变的步骤在抽象基类中实现，而可变的检查算法必须由子类提供。

使用模板方法执行单一类型检查
```
/**
* Prerequisite: pageToCheck has been successfully parsed,
* prior to constructing this Checker instance.
*/

public CheckingResultsCollector performCheck() {

// assert prerequisite

assert  pageToCheck  !=  null

initResults()

return check() // subclass executes the actual checking algorithm

    }
```
下图描述了模板方法：

![图 2.23：模板方法（摘录）](image/B15546_02_23.jpg)

###### 图 2.23：模板方法（摘录）

下表描述了模板的组件：

![图 2.24：模板组件的描述](image/B15546_02_24.jpg)

###### 图 2.24：模板组件的描述

### 8.4 报告
`HtmlSC` 支持以下输出（`== reporting`）格式和目标：
* 格式包括 HTML 和文本。
* 目标包括文件和控制台。

报告子系统使用模板方法模式来允许不同的输出格式（例如，控制台和 HTML）。报告的整体结构始终相同。

使用模板方法模式报告发现

（通用且抽象的）报告在抽象 `Reporter` 类中实现，如下所示：
```
/**
* main entry point for reporting - to be called when a report is requested
* Uses template-method to delegate concrete implementations to subclasses
*/

public void reportFindings() {

initReport() // (1)

reportOverallSummary() // (2)

reportAllPages() // (3)

closeReport() // (4)

}

private void reportAllPages() {

pageResults.each { pageResult ->

reportPageSummary( pageResult ) // (5)

pageResult.singleCheckResults.each { resultForOneCheck ->

reportSingleCheckSummary( resultForOneCheck ) // (6)

reportSingleCheckDetails( resultForOneCheck ) // (7)

reportPageFooter()

}

}
```
前述代码可总结如下：
1. 初始化报告，即创建并打开文件，并复制 `CSS`、`JavaScript` 和图像文件。
2. 创建总体摘要，以及总体成功百分比和所有已检查页面及其成功率的列表。
3. 遍历所有页面。
4. 写入报告页脚。在 HTML 报告中，还创建返回顶部的链接。
5. 对于单个页面，报告检查次数和问题数，以及成功率。
6. 对于该页面上的每个 `singleCheck`，报告摘要和 `singleCheck` 的所有详细发现。
7. 对于每个已检查页面，创建页脚、分页符或类似内容以在图形上区分页面。

以下示例报告说明了这些步骤：

![图 2.25：显示运行/页面/检查结果层级关系的示例报告](image/B15546_02_25.jpg)

###### 图 2.25：显示运行/页面/检查结果层级关系的示例报告

## II.9 设计决策
#### 注
您可能希望了解一些重要、巨大、昂贵、有风险或其它特殊的架构和设计决策。

了解这些决策的原因尤其有趣。

### 9.1 外部链接检查推迟
在 `HtmlSC` 的当前版本中，我们无法检查外部链接。这些检查已推迟到后续版本。

### 9.2 使用 jsoup 进行 HTML 解析
为了检查 HTML，我们将其解析为内部（`DOM` 类似）表示。为此任务，我们使用 `jsoup`（[https://jsoup.org](https://jsoup.org)），这是一个没有外部依赖的开源解析器。

引用其网站的话：

"*jsoup is a Java library for working with real-world HTML. It provides a very convenient API for extracting and manipulating data, using the best of DOM, CSS, and jQuery-like methods*."

此决策的目标：通过使用现有的 API 以编程方式检查 HTML，该 API 提供对要检查的文件（s）的 `DOM` 树的访问和查找方法。

决策标准如下：
* 依赖项很少，因此 `HtmlSC` 二进制文件保持尽可能小
* 非常易于使用：简单而优雅的（访问器和查找器）方法，可轻松在 `DOM` 树中定位图像、链接和链接目标
* 高度灵活：可以解析文件和字符串（以及其他）输入

HTML 解析器的替代方案如下：
* `jsoup`：一个没有依赖项的纯 HTML 解析器，具有丰富的 API，可以 `DOM` 类似语法访问所有 HTML 元素。这是明显的赢家！

