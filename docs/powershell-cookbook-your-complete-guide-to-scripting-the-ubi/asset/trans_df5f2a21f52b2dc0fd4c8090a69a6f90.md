
## 12.4 从 Internet 下载网页

### 讨论

之所以这样，是因为所有 Web 浏览器都会在其 Web 请求中发送用户代理标识符。此标识符会告知网站是哪个应用程序发出了请求——例如 Edge、Firefox 或搜索引擎的自动爬虫。许多网站会检查此用户代理标识符以确定如何显示页面。遗憾的是，如果无法确定传入请求的用户代理，许多网站将完全无法工作。

默认情况下，PowerShell 使用类浏览器的用户代理标识自身：`Mozilla/5.0 (Windows NT 10.0; Microsoft Windows 10.0.19041; en-US) PowerShell/7.1.0`。如果你需要为请求自定义用户代理字符串，可以使用 `-UserAgent` 参数指定。该参数接受一个简单的字符串。`[Microsoft.PowerShell.Commands.PSUserAgent]` 类的静态属性提供了一些预配置的默认值：

```
PS > $userAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome
PS > $result = Invoke-WebRequest http://www.bing.com -UserAgent $userAgent
```

有关解析网页的更多信息，请参阅 [配方 12.5](#ch12.xhtml#parse_analyze_web_page)。

### 另请参阅

[配方 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

[配方 10.1，“访问 XML 文件中的信息”](#ch10.xhtml#access_information_in_an_xml_file)

[配方 12.5，“从 Internet 解析和分析网页”](#ch12.xhtml#parse_analyze_web_page)

# 12.5 从 Internet 解析和分析网页

## 问题

你想要解析并交互网页内容。

## 解决方案

使用 `Invoke-WebRequest` cmdlet 下载网页，然后访问 `ParsedHtml` 属性：

```
PS > $source = "http://www.bing.com/search?q=sqrt(2)"
PS > $result = Invoke-WebRequest $source
PS > $resultContainer = $result.ParsedHtml.GetElementById("results_container")
PS > $answerElement = $resultContainer.getElementsByTagName("div") |
    Where-Object ClassName -eq "ans" | Select -First 1
PS > $answerElement.innerText
```

要仅检索图像、链接或输入字段，请访问 `Invoke-WebRequest` 结果的那些属性：

```
PS > $source = "http://www.bing.com/search?q=sqrt(2)"
PS > $result = Invoke-WebRequest $source
PS > $result.Links
```

## 讨论

当你从互联网上的网页检索数据时，通常的方法依赖于文本操作——正则表达式、字符串替换和格式化。如果你非常幸运，网页编写得非常仔细，使其也成为一个 XML 文档——在这种情况下，你可以使用 PowerShell 的 XML 支持来提取信息。[配方 12.4](#ch12.xhtml#download_a_web_page_from_the_internet) 描述了这种方法。

> **注意**
>
> 如果你需要与基于 XML 或 REST 的互联网 API 交互，请参阅 [配方 12.7](#ch12.xhtml#interact_rest_api)。

这些方法的风险在于，几个字符或空格的变化就可能轻易破坏你设计的任何文本操作。

解决方案通常来自于使用工具包，以浏览器解析网页的方式来解析网页。最重要的是，这些工具包需要考虑到编写不良的 HTML：不匹配的引号字符、缺少的结束标签、字符编码，以及互联网“下水道”能扔过来的任何其他东西。

幸运的是，PowerShell 的 `Invoke-WebRequest` cmdlet 暴露了一个极其强大的解析引擎：随操作系统附带的 Internet Explorer 引擎。

当你访问 `Invoke-WebRequest` 返回对象的 `ParsedHtml` 属性时，你将直接获得 Internet Explorer 在解析网页时使用的文档对象模型 (DOM) 的访问权限。此属性返回一个 HTML 元素，最初代表整个 HTML 文档。要访问 HTML 元素，它支持有用的方法和属性——最有用的是 `getElementById()`（查找具有特定 ID 的元素）、`getElementsByTagName()`（查找所有 `DIV` 元素、`IMG` 元素等）和 `childNodes`（专门按位置检索子元素）。

> **注意**
>
> `ParsedHtml` 属性所需的 Internet Explorer 引擎在某些版本的 PowerShell 上不受支持。如果你想在该属性不存在时进行网页解析，请务必提供 `Invoke-WebRequest` 的 `-UseBasicParsing` 参数。此模式仅对请求的网页执行有限的解析——图像、输入字段、链接和原始 HTML 内容。对于额外的功能，请考虑外部 HTML 解析库，如 `HTML Agility Pack`。

要查看可通过 `ParsedHtml` 属性获得的所有方法和属性，请使用 `Get-Member` cmdlet：

```
PS > $result = Invoke-WebRequest $source
PS > $result.ParsedHtml | Get-Member
```

当你使用这些方法和属性检索项目（如 `DIV` 或段落）时，你会得到另一个支持相同属性的元素。这使得迭代和细化既成为可能且通常准确。你通常必须查看 HTML 内容本身，以发现可用于查找所需特定 HTML 元素的元素 ID、名称和类名。

考虑到网页中包含的大量信息，尽快缩小搜索范围非常重要，这样 Internet Explorer 和 PowerShell 就不需要搜索每个元素来查找匹配的项目。`getElementById()` 方法是缩小搜索范围的最快方法，其次是 `getElementsByTagName()`，最后是使用 `Where-Object` cmdlet。

> **注意**
>
> 如果你必须依赖 `Where-Object` cmdlet 来筛选结果，请务必使用 `Select-Object` cmdlet 仅选取第一项，如解决方案所示。这会提示 PowerShell 一旦找到你需要的 HTML 元素就停止搜索。否则，它将继续查找所有剩余的文档元素——一个非常缓慢的过程。

一旦你缩小了所需元素的范围，`InnerText` 和 `InnerHtml` 属性非常有用。如果你仍然需要进行额外的文本或 HTML 操作，它们分别代表你元素的纯文本内容和实际 HTML 文本。

除了解析单个 HTML 网页外，你可能还想编写多页面 Web 会话脚本。有关此内容的示例，请参阅 [配方 12.6](#ch12.xhtml#script_web_session)。

## 另请参阅

[配方 10.1，“访问 XML 文件中的信息”](#ch10.xhtml#access_information_in_an_xml_file)

[配方 12.4，“从 Internet 下载网页”](#ch12.xhtml#download_a_web_page_from_the_internet)

[配方 12.6，“编写 Web 应用程序会话脚本”](#ch12.xhtml#script_web_session)

[配方 12.7，“与基于 REST 的 Web API 交互”](#ch12.xhtml#interact_rest_api)

# 12.6 编写 Web 应用程序会话脚本

## 问题

你想要与需要动态 Cookie、登录或多个请求的网站或应用程序进行交互。

## 解决方案

使用 `Invoke-WebRequest` cmdlet 下载网页，并访问 `-SessionVariable` 和 `-WebSession` 参数。例如，要检索活动 Facebook 通知的数量：

```
$cred = Get-Credential
$login = Invoke-WebRequest http://www.facebook.com/login.php -SessionVariable fb
$login.Forms[0].Fields.email = $cred.GetNetworkCredential().UserName
$login.Forms[0].Fields.pass = $cred.GetNetworkCredential().Password
$main = Invoke-WebRequest $login.Forms[0].Action
    -WebSession $fb -Body $login -Method Post
$main.ParsedHtml.getElementById("notificationsCountValue").InnerText
```

## 讨论

虽然互联网上的许多页面在你访问网页时直接提供其信息，但许多其他页面并非如此简单。例如，该站点可能受登录页面保护（随后设置 Cookie），后面跟着另一个表单（需要这些 Cookie）以返回搜索结果。

