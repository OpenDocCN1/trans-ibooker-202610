# 第 12 章. 支持 Internet 的脚本

## 12.0 简介

尽管 PowerShell 即使在脚本仅与本地系统交互时也能提供巨大的好处，但处理来自 Internet 的数据源将开启令人兴奋且独特的机会。例如，您可能从 Internet 下载文件或信息、与 Web 服务交互、将输出存储为 HTML，甚至发送一封报告长时间运行脚本结果的电子邮件。

通过其 cmdlet 和对 .NET Framework 中网络支持的访问，PowerShell 为支持 Internet 的管理提供了充足的机会。

## 12.1 从 FTP 或 Internet 站点下载文件

### 问题

您想从 Internet 上的 FTP 位置或网站下载文件。

### 解决方案

使用 `Invoke-WebRequest` cmdlet 的 `-OutFile` 参数：

```
PS > $source = "http://www.leeholmes.com/favicon.ico"
PS > $destination = "c:\temp\favicon.ico"
PS >
PS > Invoke-WebRequest $source -OutFile $destination
```

### 讨论

`Invoke-WebRequest` cmdlet 让您能轻松地从远程 Web 服务器上传和下载数据。它的行为很像 Web 浏览器，您可以指定用户代理、代理（如果您的出站连接需要一个），甚至凭据。

虽然解决方案演示了从 Web (HTTP) 资源下载文件，但 `Invoke-WebRequest` cmdlet 也支持 FTP 位置。要指定 FTP 位置，请在源的开头使用 `ftp://`，如示例 12-1 所示。

#### 示例 12-1. 从 FTP 站点下载文件

```
PS > $source = "ftp://site.com/users/user/backups/backup.zip"
PS > $destination = "c:\temp\backup.zip"
PS >
PS > Invoke-WebRequest $source -OutFile $destination -Credential myFtpUser
```

与从大多数 Internet 站点下载的文件不同，FTP 传输通常需要用户名和密码。要指定您的用户名和密码，请使用 `-Credential` 参数。

如果您下载的文件最终是一个您想要解析或阅读的网页，`Invoke-WebRequest` cmdlet 还有其他专门为该场景设计的功能。有关如何下载和解析网页的更多信息，请参阅 食谱 12.4。

### 另请参考

- 食谱 12.4，“从 Internet 下载网页”

## 12.2 将文件上传到 FTP 站点

### 问题

您想将文件上传到 FTP 站点。

### 解决方案

要将文件上传到 FTP 站点，请使用来自 .NET Framework 的 `System.Net.WebClient` 类：

```
PS > $source = "c:\temp\backup.zip"
PS > $destination = "ftp://site.com/users/user/backups/backup.zip"
PS > $cred = Get-Credential
PS > $wc = New-Object System.Net.WebClient
PS > $wc.Credentials = $cred
PS > $wc.UploadFile($destination, $source)
PS > $wc.Dispose()
```

### 讨论

对于上传文件到远程 FTP 站点的基本需求，`System.Net.WebClient` 类提供了一个极其简单的解决方案。对于更高级的 FTP 场景（如删除文件），`System.Net.WebRequest` 类提供了更精细的控制，如示例 12-2 所示。

#### 示例 12-2. 从 FTP 站点删除文件

```
$file = "ftp://site.com/users/user/backups/backup.zip"
$request = [System.Net.WebRequest]::Create($file)
$cred = Get-Credential
$request.Credentials = $cred
$request.Method = [System.Net.WebRequestMethods+Ftp]::DeleteFile
$response = $request.GetResponse()
$response
$response.Close()
```

除了 `Delete`，`WebRequest` 类还支持许多其他 FTP 方法。您可以通过获取 `[System.Net.WebRequestMethods+Ftp]` 类的静态属性来查看所有这些方法，如示例 12-3 所示。

#### 示例 12-3. 标准支持的 FTP 方法

```
PS > [System.Net.WebRequestMethods+Ftp] | Get-Member -Static -Type Property


   TypeName: System.Net.WebRequestMethods+Ftp

Name                     MemberType Definition
----                     ---------- ----------
AppendFile               Property   static string AppendFile {get;}
DeleteFile               Property   static string DeleteFile {get;}
DownloadFile             Property   static string DownloadFile {get;}
GetDateTimestamp         Property   static string GetDateTimestamp {get;}
GetFileSize              Property   static string GetFileSize {get;}
ListDirectory            Property   static string ListDirectory {get;}
ListDirectoryDetails     Property   static string ListDirectoryDetails {get;}
MakeDirectory            Property   static string MakeDirectory {get;}
PrintWorkingDirectory    Property   static string PrintWorkingDirectory {get;}
RemoveDirectory          Property   static string RemoveDirectory {get;}
Rename                   Property   static string Rename {get;}
UploadFile               Property   static string UploadFile {get;}
UploadFileWithUniqueName Property   static string UploadFileWithUniqueName {get;}
```

这些属性只是对应标准 FTP 命令的字符串，因此如果您知道它们，也可以直接使用它们的值：

```
$request.Method = "DELE"
```

如果您想从 FTP 站点下载文件，请参阅 食谱 12.1。

### 另请参考

- 食谱 12.1，“从 FTP 或 Internet 站点下载文件”

## 12.3 程序：解析 Internet 重定向的目标

示例 12-4 展示了如何使用 `System.Net.HttpWebRequest` 类连接到 Web 服务器以确定链接重定向到何处，而不仅仅是从最终目标获取页面内容。

#### 示例 12-4. 解析重定向的目标

```
##############################################################################
##
## Resolve-Uri
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Resolve a URI to the URIs it redirects to

.EXAMPLE

PS> Resolve-Uri https://bit.ly/e0Mw9w

https://bit.ly/e0Mw9w
http://www.leeholmes.com/projects/ps_html5/Invoke-PSHtml5.ps1

#>

param(
    ## The URI to resolve
    [Parameter(Mandatory, Position = 0)]
    $Uri
)

$ProgressPreference = "Ignore"
$ErrorActionPreference = "Stop"

## While we still have a URI to process
while($Uri)
{
    $Uri

    ## Connect to the URI. Don't allow redirects, so that we can see
    ## where it redirects to.
    $wc = [System.Net.HttpWebRequest]::Create($Uri)
    $wc.AllowAutoRedirect = $false

    try
    {
        $response = $wc.GetResponse()

        ## If it was a redirect (with a "Location" header), store that and
        ## process it the next time around.
        if($response.Headers["Location"])
        {
            $Uri = $response.Headers["Location"]
        }
        else
        {
            $Uri = $null
        }
    }
    catch
    {
        ## Some scenarios handle the scenario above through an exception, so
        ## handle that here.
        if($_.Exception.InnerException.Response.StatusCode -eq "Moved")
        {
            $Uri = $_.Exception.InnerException.Response.Headers["Location"]
        }
        else
        {
            throw $_
        }
    }
}
```

有关运行脚本的更多信息，请参阅 食谱 1.2。

### 另请参考

- 食谱 1.2，“运行程序、脚本和现有工具”

## 12.4 从 Internet 下载网页

### 问题

您想从 Internet 下载网页并直接处理其内容。

### 解决方案




使用 `Invoke-WebRequest` cmdlet 下载网页，然后访问 `Content` 属性（或将结果转换为 `[string]`）：

```powershell
PS > $source = "http://www.bing.com/search?q=sqrt(2)"
PS > $result = [string] (Invoke-WebRequest $source)
```

## 讨论

在编写面向互联世界的自动化脚本时，我们并不总是有幸能访问返回丰富结构化数据的 Web 服务。因此，从互联网上的服务检索数据通常通过*屏幕抓取*来实现：下载网页的 HTML，然后小心翼翼地从绝大多数不需要的内容中分离出你想要的内容。

### 注意

如果从网页提取结构化数据是你的主要目标，`Invoke-WebRequest` cmdlet 提供的选项比基本的屏幕抓取强大得多。有关详细信息，请参阅 食谱 12.5。

屏幕抓取技术的历史比互联网还要悠久！只要计算机系统生成的输出主要是为人类设计的，屏幕抓取工具就会应运而生，使这些输出可供其他计算机程序使用。

不幸的是，屏幕抓取是一种容易出错的内容提取方式。这绝非夸张！作为证据，示例 12-6（稍后在本食谱中展示）在本书第一版撰写期间就损坏了四五次，出版后又损坏了一次。随后在第二版期间又损坏了几次，出版后再次损坏。然后，第三版出版后又损坏了。虽然第四版修复了它，但当你阅读此文时，它很可能已经再次损坏。这就是屏幕抓取的风险。

如果网页作者更改了底层 HTML，你的代码通常将停止正常工作。如果网站的 HTML 编写为有效的 XHTML，你可能可以利用 PowerShell 内置的 XML 支持更轻松地解析内容。

有关 PowerShell 内置 XML 支持的更多信息，请参阅 食谱 10.1。

尽管脆弱，纯屏幕抓取往往是唯一的选择。既然屏幕抓取只是文本操作，你拥有的选项与处理其他文本报告时相同。对于一些结构相对良好的网页，你可以通过单个正则表达式替换（加上清理）来搞定，如 示例 12-5 所示。

### 示例 12-5. Search-Bing.ps1

```powershell
##############################################################################
##
## Search-Bing
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

搜索 Bing 指定术语

.EXAMPLE

PS > Search-Bing PowerShell
搜索 Bing 中的术语 "PowerShell"

#>

param(
    ## 要搜索的术语
    $Pattern = "PowerShell"
)

Set-StrictMode -Version 3

## 创建包含 Bing 搜索结果的 URL
Add-Type -Assembly System.Web
$queryUrl = 'http://www.bing.com/search?q={0}'
$queryUrl = $queryUrl -f ([System.Web.HttpUtility]::UrlEncode($pattern))

## 下载网页
$results = [string] (Invoke-WebRequest $queryUrl)

## 提取结果文本，这些文本包含在类似于 "<div class="sb_tlst">...</div>" 的片段中
$matches = $results |
    Select-String -Pattern '(?s)<div[^>]*sb_tlst[^>]*>.*?</div>' -AllMatches

foreach($match in $matches.Matches)
{
    ## 提取 URL，仅保留 HREF 引号内的文本
    $url = $match.Value -replace '.*href="(.*?)".*','$1'
    $url = [System.Web.HttpUtility]::UrlDecode($url)

    ## 提取页面名称，将尖括号内的任何内容替换为空字符串。
    $item = $match.Value -replace '<[^>]*>', ''

    ## 输出项目
    [PSCustomObject] @{ Item = $item; Url = $url }
}
```

虽然可以使用复杂的正则表达式解析结构较松散的网页，但通过更直接的文本操作通常可以让事情简单得多。示例 12-6 使用这种第二种方法从 Bing 获取“即时答案”。

### 示例 12-6. Get-Answer.ps1

```powershell
##############################################################################
##
## Get-Answer
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

使用 Bing Answers 回答你的问题

.EXAMPLE

PS > Get-Answer "sqrt(2)"
sqrt(2) = 1.41421356

.EXAMPLE

PS > Get-Answer msft stock
Microsoft Corp (US:MSFT) NASDAQ
29.66  -0.35 (-1.17%)
After Hours: 30.02 +0.36 (1.21%)
Open: 30.09    Day's Range: 29.59 - 30.20
Volume: 55.60 M    52 Week Range: 17.27 - 31.50
P/E Ratio: 16.30    Market Cap: 260.13 B

.EXAMPLE

PS > Get-Answer "What is the time in Seattle, WA?"
Current time in Seattle, WA
01:12:41 PM
08/18/2012 ? Pacific Daylight Time

#>

Set-StrictMode -Version 3

$question = $args -join " "

function Main
{
    ## 加载 System.Web.HttpUtility DLL，以便我们进行 URLEncode
    Add-Type -Assembly System.Web

    ## 将网页获取到单个字符串中，行之间用换行符分隔。
    $encoded = [System.Web.HttpUtility]::UrlEncode($question)
    $url = "http://www.bing.com/search?q=$encoded"
    $text = [String] (Invoke-WebRequest $url)

    ## 查找答案部分的开始
    $startIndex = $text.IndexOf('<li class="b_ans')

    ## 结束位置可能由以下字符串之一定义，具体取决于答案类型。取第一个找到的。
    $endStrings = "Was this helpful","finf_mod",'<li class="b_algo',
        "People also search for","Feedback"
    $answerIndexes = $endStrings | Foreach-Object { $text.IndexOf($_, $startIndex) } |
        Where-Object { $_ -ge 0 }
    $endIndex = ($answerIndexes | Sort-Object)[0]

    ## 如果找到结果，则筛选结果
    if(($startIndex -ge 0) -and ($endIndex -ge 0))
    {
        ## 提取开始和结束部分之间的文本
        $partialText = $text.Substring($startIndex, $endIndex - $startIndex)

        ## 非常脆弱的屏幕抓取。将一堆放置在新行上的标签替换为换行符，将其他一些替换为空格。
        $partialText = $partialText -replace '<div[^>]*>',"`n"
        $partialText = $partialText -replace '<tr[^>]*>',"`n"
        $partialText = $partialText -replace '<li[^>]*>',"`n"
        $partialText = $partialText -replace '<br[^>]*>',"`n"
        $partialText = $partialText -replace '<p [^>]*>',"`n"
        $partialText = $partialText -replace '<span[^>]*>'," "
        $partialText = $partialText -replace '<td[^>]*>',"    "

        $partialText = CleanHtml $partialText

        ## 现在按换行符拆分结果，修剪每一行，然后再将它们连接起来。
        $partialText = $partialText -split "`n" |
            Foreach-Object { $_.Trim() } | Where-Object { $_ }
        $partialText = $partialText -join "`n"

        [System.Web.HttpUtility]::HtmlDecode($partialText.Trim())
    }
    else
    {
        "No answer found."
    }
}

## 从文本块中清理 HTML
function CleanHtml ($htmlInput)
{
    $tempString = [Regex]::Replace($htmlInput, "(?s)<[^>]*>", "")
    $tempString.Replace("&nbsp&nbsp", "")
}

Main
```

使用 `Invoke-WebRequest` cmdlet 时，你可能会注意到某些 Web 应用程序表现异常，或返回错误，提示你使用的浏览器不受支持。




## 12.4 从 Internet 下载网页

### 讨论

之所以这样，是因为所有 Web 浏览器都会在其 Web 请求中发送用户代理标识符。此标识符会告知网站是哪个应用程序发出了请求——例如 Edge、Firefox 或搜索引擎的自动爬虫。许多网站会检查此用户代理标识符以确定如何显示页面。遗憾的是，如果无法确定传入请求的用户代理，许多网站将完全无法工作。

默认情况下，PowerShell 使用类浏览器的用户代理标识自身：`Mozilla/5.0 (Windows NT 10.0; Microsoft Windows 10.0.19041; en-US) PowerShell/7.1.0`。如果你需要为请求自定义用户代理字符串，可以使用 `-UserAgent` 参数指定。该参数接受一个简单的字符串。`[Microsoft.PowerShell.Commands.PSUserAgent]` 类的静态属性提供了一些预配置的默认值：

```
PS > $userAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome
PS > $result = Invoke-WebRequest http://www.bing.com -UserAgent $userAgent
```

有关解析网页的更多信息，请参阅 配方 12.5。

### 另请参阅

配方 1.2，“运行程序、脚本和现有工具”

配方 10.1，“访问 XML 文件中的信息”

配方 12.5，“从 Internet 解析和分析网页”

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

当你从互联网上的网页检索数据时，通常的方法依赖于文本操作——正则表达式、字符串替换和格式化。如果你非常幸运，网页编写得非常仔细，使其也成为一个 XML 文档——在这种情况下，你可以使用 PowerShell 的 XML 支持来提取信息。配方 12.4 描述了这种方法。

> **注意**
>
> 如果你需要与基于 XML 或 REST 的互联网 API 交互，请参阅 配方 12.7。

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

除了解析单个 HTML 网页外，你可能还想编写多页面 Web 会话脚本。有关此内容的示例，请参阅 配方 12.6。

## 另请参阅

配方 10.1，“访问 XML 文件中的信息”

配方 12.4，“从 Internet 下载网页”

配方 12.6，“编写 Web 应用程序会话脚本”

配方 12.7，“与基于 REST 的 Web API 交互”

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




# 使用 PowerShell 自动化 Web 场景

自动化这些场景几乎总是需要对相关 Web 应用程序有相当深入的了解，以及对 Web 应用程序一般工作原理的了解。

即使有了这种了解，自动化这些场景通常也需要大量的脚本编写：解析 HTTP 头、在后续请求中发送它们、手工构建表单 `POST` 响应等。

作为 Facebook 登录裸脚本的一个示例，请考虑以下示例，它仅确定用于后续页面请求的登录 Cookie：

```powershell
$Credential = Get-Credential

## Get initial cookies
$wc = New-Object System.Net.WebClient
$wc.Headers.Add("User-Agent", "User-Agent: Mozilla/4.0 (compatible; MSIE 7.0;)")

$result = $wc.DownloadString("https://www.facebook.com/")
$cookie = $wc.ResponseHeaders["Set-Cookie"]
$cookie = ($cookie.Split(',') -match '^\S+=\S+;' -replace ';.*','') -join '; '

$wc = New-Object System.Net.WebClient
$wc.Headers.Add("User-Agent", "User-Agent: Mozilla/4.0 (compatible; MSIE 7.0;)")
$wc.Headers.Add("Cookie", $cookie)
$postValues = New-Object System.Collections.Specialized.NameValueCollection
$postValues.Add("email", $credential.GetNetworkCredential().Username)
$postValues.Add("pass", $credential.GetNetworkCredential().Password)

## Get the resulting cookie, and convert it into the form to be returned
## in the query string
$result = $wc.UploadValues(
    "https://login.facebook.com/login.php?login_attempt=1", $postValues)
$cookie = $wc.ResponseHeaders["Set-Cookie"]
$cookie = ($cookie.Split(',') -match '^\S+=\S+;' -replace ';.*','') -join '; '
$cookie
```

这只是用于登录。使用这种手动方法编写完整的 Web 会话脚本轻松就能达到数百行。

如果你的 PowerShell 版本支持，`Invoke-WebRequest` cmdlet 的 `-SessionVariable` 和 `-WebSession` 参数并不会消除对目标 Web 应用程序工作原理的理解需求。但是，它们确实消除了处理裸 HTTP 请求和响应的枯燥和复杂性。这种改进的会话支持主要通过以下四个功能实现：

## 自动 Cookie 管理

大多数 Web 应用程序将状态存储在 Cookie 中——会话 ID 和登录信息是最常见的两种存储内容。当 Web 应用程序请求存储或删除 Cookie 时，`Invoke-WebRequest` 会自动将此信息记录在提供的会话变量中。使用此会话变量的后续请求会自动提供 Web 应用程序所需的任何 Cookie。你可以通过查看会话变量的 `Cookies` 属性来查看正在使用的 Cookie：

```powershell
$fb.Cookies.GetCookies("https://www.facebook.com") | Select Name,Value
```

## 自动重定向支持

提交 Web 表单（尤其是登录表单）后，许多网站会在你最终到达目标页面之前，通过一系列中间页面进行重定向。在基本的 HTTP 脚本编写中，这迫使你处理许多 HTTP 重定向状态代码、解析 `Location` 头并重新提交所有适当的值。`Invoke-WebRequest` cmdlet 会为你处理这个问题；它返回的结果来自任何重定向序列中的最终页面。如果你想覆盖此行为，请使用 `-MaximumRedirection` 参数。

## 表单检测

需要高级会话脚本的应用程序倾向于从 HTML 表单中的字段获取大部分输入数据，而不是从 URL 本身的项目获取。`Invoke-WebRequest` 通过其结果的 `Forms` 属性公开这些表单。此集合返回表单 ID（如果有多个表单则很有用）、表单操作（用于提交表单的 URL）以及表单定义的字段。

## 表单提交

在传统的 HTTP 脚本编写中，提交表单是一个复杂的过程。你需要收集所有表单字段、正确编码它们、确定生成的编码长度，并将所有这些数据 `POST` 到目标 URL。

`Invoke-WebRequest` 通过在将 `-Method` 参数的值设为 `POST` 时用作输入的 `-Body` 参数，使这变得非常简单。`-Body` 参数接受以下三种格式之一的输入：

* 之前 `Invoke-WebRequest` 调用的结果，在这种情况下将使用第一个表单的值（如果响应仅包含一个表单）。
* 一个特定表单（从之前 `Invoke-WebRequest` 调用的 `Forms` 属性手动选择），在这种情况下将使用该表单的值。
* 一个 `IDictionary`（哈希表），在这种情况下将使用该字典中的名称和值。
* 一个 XML 节点，在这种情况下 XML 将被直接编码。这主要用于编写 REST API 脚本，在编写 Web 应用程序会话脚本时不太可能用到。
* 一个字节数组，在这种情况下字节将被直接使用和编码。这主要用于编写数据上传脚本。

让我们看看这些功能在解决方案中的脚本中是如何发挥作用的，该脚本用于检测 Facebook 上有多少待处理的通知。考虑到 Web 应用程序变化的速度，这个示例不太可能长期继续有效。不过，它确实演示了思维过程。

当你首次连接到 Facebook 时，需要登录。Facebook 通过一个名为 `login.php` 的页面来处理此过程：

```powershell
$login = Invoke-WebRequest http://www.facebook.com/login.php -SessionVariable fb
```

如果你查看返回的页面，会发现有一个包含 `email` 和 `pass` 字段的单个表单：

```powershell
PS > $login.Forms.Fields

Key                Value
---                -----
(...)
return_session     0
legacy_return      1
session_key_only   0
trynum             1
email
pass
persist_box        1
default_persistent 0
(...)
```

我们填写这些字段：

```powershell
$cred = Get-Credential
$login.Forms[0].Fields.email = $cred.UserName
$login.Forms[0].Fields.pass = $cred.GetNetworkCredential().Password
```

然后提交表单。我们将 `$fb` 用于 `-WebSession` 参数，因为这是我们在原始请求中使用的。我们将 `POST` 到登录表单 `Action` 字段中引用的 URL，并使用 `$login` 变量作为请求正文。`$login` 变量是我们从第一个请求获得的响应，我们在其中自定义了 `email` 和 `pass` 表单字段。PowerShell 识别出这是之前 Web 请求的结果，并将该单个表单用作 `POST` 正文：

```powershell
$mainPage = Invoke-WebRequest $login.Forms[0].Action -WebSession $fb `
    -Body $login -Method Post
```

如果你查看此响应返回的原始 HTML（`Content` 属性），可以看到通知计数包含在一个 ID 为 `notificationsCountValue` 的 `span` 元素中：

```html
(...) <span id="notificationsCountValue">1</span> (...)
```

要检索此元素，我们使用响应的 `ParsedHtml` 属性，调用 `GetElementById` 方法，并返回 `InnerText` 属性：

```powershell
$mainPage.ParsedHtml.getElementById("notificationsCountValue").InnerText
```

使用这些技术，我们可以解锁互联网上大量功能，这些功能以前隐藏在复杂的 HTTP 脚本编写背后。

有关使用 `ParsedHtml` 属性解析和分析网页的更多信息，请参阅 食谱 12.5。

## 另请参阅

食谱 12.5，“从互联网解析和分析网页”

# 12.7 与基于 REST 的 Web API 交互

## 问题

你想要使用基于 XML 或 JSON 的 REST API。

## 解决方案

使用 `Invoke-RestMethod` cmdlet 来处理基于 REST 的 API。示例 12-7 演示了如何使用 StackOverflow API 检索标记为“PowerShell”的 10 个最新未回答问题。

### 示例 12-7. 将 Invoke-RestMethod 与 StackOverflow API 配合使用



# 12.7 与基于 REST 的 Web API 交互

```
PS > $url = "https://api.stackexchange.com/2.0/questions/unanswered" +
    "?order=desc&sort=activity&tagged=powershell&pagesize=10&site=stackoverflow"
PS > $result = Invoke-RestMethod $url
PS > $result.Items | ForEach-Object { $_.Title; $_.Link; "" }
```

可以在没有扩展名的文件中放置 PowerShell 脚本吗？
http://stackoverflow.com/questions/12230228/can-i-have-powershell-scripts...

PowerShell：用变量替换正则命名组
http://stackoverflow.com/questions/12225415/powershell-replacing-regex-named...

(...)

## 讨论

大多数返回有用数据的网页提供这些信息，初衷是仅由 Web 浏览器显示。提取这些信息向来困难，尽管 食谱 12.5 通常能让解决方案比直接文本操作更简单。

当网页设计为供其他程序或脚本使用时，通常称为 *Web 服务* 或 *Web API*。Web 服务是两者中功能更全面的。它们依赖于一种称为 SOAP（简单对象访问协议）的技术，并模拟支持严格结构、标准化调用行为和强类型对象的传统编程 API。食谱 12.8 演示了如何从 PowerShell 与 Web 服务交互。

虽然结构松散得多，但 Web API 倾向于遵循一些相似的基本设计理念——主要是 URL 结构、标准 HTTP 方法（`GET`/`POST`）和数据类型（`JSON`/`XML`）。这些松散定义的设计理念通常归类为 *REST*（表述性状态传输）一词，使得 *REST API* 成为非 SOAP Web 服务最常用的术语。

虽然仍设计为供程序或脚本使用，但 REST API 的结构要松散得多。由于其简洁性，它们已成为互联网上占主导地位的 Web 服务形式。

`Invoke-RestMethod` cmdlet 构成了你从 PowerShell 与 REST API 交互的基础。它的作用很像 `Invoke-WebRequest` cmdlet，允许你对 URL 调用标准 HTTP 操作：`GET`、`PUT`、`POST` 等。不过，与 `Invoke-WebRequest` 不同，`Invoke-RestMethod` 假设网站返回的数据是设计供程序使用的。根据 Web 服务返回的数据（`XML` 或 `JSON`），它会自动解析返回的数据并将其转换为 PowerShell 对象。

### 注意

如果这种解析对某个网站或 REST API 不正确，你始终可以直接使用 `Invoke-WebRequest` cmdlet。

作为与 REST API 交互的另一个示例，示例 12-8 演示了使用 StackOverflow API 查找与搜索词匹配的 PowerShell 问题的已采纳答案。

### 示例 12-8. 在 StackOverflow 中搜索 PowerShell 问题的答案

```
##############################################################################
##
## Search-StackOverflow
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

搜索与搜索词相关的 PowerShell 问题，并提供指向已采纳答案的链接。


.EXAMPLE

PS > Search-StackOverflow upload ftp
搜索关于如何上传 FTP 文件的 StackOverflow 问题

.EXAMPLE

PS > $answers = Search-StackOverflow.ps1 upload ftp
PS > $answers | Out-GridView -PassThru | Foreach-Object { start $_ }

启动 Out-GridView 显示搜索得到的答案。选择你想要打开的 URL，然后按确定。PowerShell 随后会为这些 URL 启动默认 Web 浏览器。

#>

Set-StrictMode -Off
Add-Type -Assembly System.Web

$query = $args -join " "
$query = [System.Web.HttpUtility]::UrlEncode($query)

## 使用 StackOverflow API 检索问题的答案
$url = "https://api.stackexchange.com/2.0/search?order=desc&sort=relevance" +
    "&pagesize=5&tagged=powershell&intitle=$query&site=stackoverflow"
$question = Invoke-RestMethod $url

## 现在遍历并显示问题和答案
$question.Items | Where-Object accepted_answer_id | Foreach-Object {
        "Question: " + $_.Title
        "https://www.stackoverflow.com/questions/$($_.accepted_answer_id)"
        ""
}
```

## 另请参阅

食谱 12.5，“从互联网解析和分析网页”

# 12.8 连接到 Web 服务

## 问题

你想要连接并与互联网 Web 服务交互。

## 解决方案

使用 `New-WebserviceProxy` cmdlet 处理 Web 服务。

```
PS > $url = "http://dneonline.com/calculator.asmx"
PS > $calculator = New-WebserviceProxy $url -Namespace Cookbook
PS > $calculator.Add(2, 3)
5
```

## 讨论

尽管屏幕抓取（解析网页的 HTML）是从互联网获取数据的最常用方法，但 Web 服务正变得越来越普遍。Web 服务比 HTML 解析提供了显著优势，因为当网页设计师更改设计中的细微功能时，它们不太可能中断。

### 注意

如果你需要与基于 XML 或 REST 的互联网 API 交互，请参阅 食谱 12.7。

然而，Web 服务的优势不仅在于其更稳定的接口。使用 Web 服务时，.NET Framework 允许你生成 *代理*，使你能像使用常规 .NET 对象一样轻松地与 Web 服务交互。这是因为对你这个 Web 服务用户而言，这些代理的行为几乎与任何其他 .NET 对象完全相同。要调用 Web 服务上的方法，只需调用代理上的方法。

`New-WebserviceProxy` cmdlet 简化了连接 Web 服务所需的所有工作，使其就像调用 `New-Object` cmdlet 一样简单。

使用 Web 服务代理（与常规 .NET 对象相比）时，你会注意到的主要区别是速度和互联网连接要求。视条件而定，Web 服务代理上的方法调用可能轻松耗费数秒才能完成。如果你的计算机（或远程计算机）遇到网络困难，该调用甚至可能返回网络错误消息（如超时），而不是你期望的信息。

如果 Web 服务需要域身份验证，请指定 `-UseDefaultCredential` 参数。如果需要显式凭据，请使用 `-Credential` 参数。

当你创建新的 Web 服务代理时，PowerShell 会代表你创建一个连接到该 Web 服务的新 .NET 对象。所有 .NET 类型都存在于 *命名空间* 中，以防止它们与其他同名类型冲突，因此 PowerShell 会自动为你生成命名空间名称。通常你不需要关注此命名空间。然而，某些 Web 服务需要 Web 服务也定义的输入对象，例如解决方案中的 `Place` 对象。对于这些 Web 服务，请使用 `-Namespace` 参数将 Web 服务（及其支持对象）置于你选择的命名空间中。

### 注意

一个 Web 服务代理的支持对象无法被另一个 Web 服务代理使用，即使它们是指向同一 URL 的 Web 服务的两个代理。如果你需要处理指向同一 URL 的 Web 服务的两个连接，且任务要求为该服务创建支持对象，请务必为这些代理使用两个不同的命名空间。

有关运行脚本的详细信息，请参阅 食谱 1.2。

## 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

食谱 12.7，“与基于 REST 的 Web API 交互”

# 12.9 与远程 SSL 证书交互并进行管理

## 问题




## 12.9 从远程服务器检索 SSL/TLS 证书

### 问题

您想要从远程 Web 服务器检索 SSL/TLS 证书，以检查其有效期或其他属性。

### 解决方案

使用 .NET Framework 中的 `System.Net.WebRequest` 类，然后设置其 `ServerCertificateValidationCallback` 属性以捕获证书：

```
$webRequest = [Net.WebRequest]::Create("https://www.powershellcookbook.com")
$webRequest.ServerCertificateValidationCallback = {
    param($Request, $Certificate, $SslPolicyErrors)
    $GLOBAL:certificateResult = $Certificate

    ## 返回 $true 将忽略所有错误
    $true
}
$null = $webRequest.GetResponse()
$certificateResult | Format-List
```

```
Subject      : CN=sni.cloudflaressl.com, O="Cloudflare, Inc.", L=San Francisco, S=CA,
Issuer       : CN=Cloudflare Inc ECC CA-3, O="Cloudflare, Inc.", C=US
Thumbprint   : 53D9FE57E262D018AB656BA212FAB67D84C75660
FriendlyName :
NotBefore    : 8/7/2020 5:00:00 PM
NotAfter     : 8/8/2021 5:00:00 AM
Extensions   : {System.Security.Cryptography.Oid, System.Security.Cryptography.Oid,
               System.Security.Cryptography.Oid, System.Security.Cryptography.Oid...}
```

### 讨论

尽管 SSL/TLS 证书提供了极大的安全性，但过期证书却是互联网上导致重大服务中断的主要原因之一。在您的环境中降低此风险的一个好方法是定期扫描 Web 服务器上即将过期的证书，以便您可以优先轮换和替换它们。一旦您从远程计算机检索了证书（如解决方案所示），您就可以轻松检查其 `NotAfter` 属性或您感兴趣的任何其他属性。

此外，您有时可能需要与拥有过期、配置错误或损坏的 SSL 证书的网站进行交互。这些网站使用起来有风险，因为许多看似配置错误的错误实际上可能是恶意的。出于这个原因，PowerShell 在您尝试默认连接时会生成错误。要跳过此错误，请提供 `-SkipCertificateCheck` 参数：

```
PS > Invoke-WebRequest https://expired-rsa-dv.ssl.com/
Invoke-WebRequest: The remote certificate is invalid because of errors in the
certificate chain: NotTimeValid
PS > Invoke-WebRequest https://expired-rsa-dv.ssl.com/ -SkipCertificateCheck

StatusCode        : 200
StatusDescription : OK
Content           : <HTML>
                    This is a test site authenticated by <a
                    href="https://www.ssl.com"
                    target="_blank">SSL.com</a> using SSL/TLS Certificate!
                    </HTML>
```

### 另请参阅

食谱 12.4，“从互联网下载网页”

## 12.10 将命令输出导出为网页

### 问题

您想要将命令的结果导出为网页，以便将其发布到 Web 服务器。

### 解决方案

使用 PowerShell 的 `ConvertTo-Html` cmdlet 将命令输出转换为网页。例如，要创建 PowerShell 命令的快速 HTML 摘要：

```
PS > $filename = "c:\temp\help.html"
PS >
PS > $commands = Get-Command | Where { $_.CommandType -ne "Alias" }
PS > $summary = $commands | Get-Help | Select Name,Synopsis
PS > $summary | ConvertTo-Html | Set-Content $filename
```

### 讨论

当您使用 `ConvertTo-Html` cmdlet 将命令输出导出到文件时，PowerShell 会生成一个表示命令输出的 HTML 表格。在表格中，它会为您提供的每个对象创建一行。对于每一行，PowerShell 会创建列来表示对象属性的值。

如果表格格式使输出难以阅读，`ConvertTo-Html` 提供了 `-As` 参数，允许您将输出样式设置为 `Table` 或 `List`。

虽然默认输出很有用，但您可以根据需要自定义生成的 HTML 的结构和样式。例如，`-PreContent` 和 `-PostContent` 参数允许您在生成的表格或列表之前和之后包含额外的文本。`-Head` 参数允许您定义 HTML 的 head 部分的内容。即使您想从头生成大部分 HTML，您仍然可以使用 `-Fragment` 参数仅生成内部表格或列表。

有关 `ConvertTo-Html` cmdlet 的更多信息，请输入 `Get-Help ConvertTo-Html`。

### 另请参阅

食谱 12.4，“从互联网下载网页”

## 12.11 发送电子邮件

### 问题

您想要发送一封电子邮件。

### 解决方案

使用 `Send-MailMessage` cmdlet 发送电子邮件。

```
PS > Send-MailMessage -To guide@leeholmes.com `
    -From user@example.com `
    -Subject "Hello!" `
    -Body "Hello, from another satisfied Cookbook reader!" `
    -SmtpServer mail.example.com
```

### 讨论

`Send-MailMessage` cmdlet 支持您期望以电子邮件为中心的 cmdlet 所支持的所有功能：附件、纯文本消息、HTML 消息、优先级、回执请求等。最困难的方面通常是记住要使用的正确 SMTP 服务器。

`Send-MailMessage` cmdlet 也有助于解决此问题。如果您不指定 `-SmtpServer` 参数，它将使用 `$PSEmailServer` 变量中指定的服务器（如果有）。

对于其大部分功能，`Send-MailMessage` cmdlet 利用了 .NET Framework 中的 `System.Net.Mail.MailMessage` 类。如果您需要 `Send-MailMessage` cmdlet 未公开的功能，直接使用该类可能是一个选择。

## 12.12 程序：监控网站正常运行时间

在管理网站（甚至您自己的博客）时，跟踪 URL 的响应时间和可用性非常有用。这有助于检测网站中断，或仅仅是意外负载的时间。

`Invoke-WebRequest` cmdlet 使这非常容易实现：

```
PS > Test-Uri http://www.leeholmes.com/blog

Time              : 9/1/2012 8:10:22 PM
Uri               : http://www.leeholmes.com/blog
StatusCode        : 200
StatusDescription : OK
ResponseLength    : 126750
TimeTaken         : 1800.7406
```

如果您将其与记录结果到 CSV 的计划作业相结合，您可以轻松地随时间监控网站的健康状况。有关此方法的示例，请参阅 食谱 27.14。

#### 示例 12-9. 测试 URI 的状态和响应能力

```
##############################################################################
##
## Test-Uri
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

连接到给定的 URI 并返回其状态：URI、响应代码和耗时。

.EXAMPLE

PS > Test-Uri bing.com

Uri               : bing.com
StatusCode        : 200
StatusDescription : OK
ResponseLength    : 34001
TimeTaken         : 459.0009

#>

param(
    ## 要测试的 URI
    $Uri
)

$request = $null
$time = try
{
    ## 请求 URI，并测量响应所花费的时间。
    $result = Measure-Command { $request = Invoke-WebRequest -Uri $uri }
    $result.TotalMilliseconds
}
catch
{
    ## 如果请求生成异常（即：500 服务器错误或 404 未找到），我们可以从 Exception.Response 属性中提取状态代码
    $request = $_.Exception.Response
    $time = -1
}

$result = [PSCustomObject] @{
    Time = Get-Date;
    Uri = $uri;
    StatusCode = [int] $request.StatusCode;
    StatusDescription = $request.StatusDescription;
    ResponseLength = $request.RawContentLength;
    TimeTaken = $time;
}

$result
```

有关运行脚本的更多信息，请参阅 食谱 1.2。

### 另请参阅

食谱 1.2，“运行程序、脚本和现有工具”

## 12.13 程序：与 Internet 协议交互




虽然在抽象层面处理网站和 Web 服务很常见，但通过在较低层级与远程计算机交互，会产生一种完全不同风格的互联网启用脚本。这个较低层级（称为 TCP 层，即传输控制协议）构成了大多数互联网协议的通信基础——例如 Telnet、SMTP（发送邮件）、POP3（接收邮件）和 HTTP（检索 Web 内容）。

.NET Framework 提供了一些类，让你直接与许多互联网协议交互：用于 SMTP 的 `System.Net.Mail.SmtpClient` 类、用于 HTTP 的 `System.Net.WebClient` 类，以及其他一些类。不过，当 .NET Framework 不支持你所需的互联网协议时，如果你知道其工作原理的细节，通常可以直接编写应用程序协议脚本。

示例 12-10 演示了如何使用 示例 12-11 中给出的 `Send-TcpRequest` 脚本，接收远程 POP3 邮箱中等待邮件的信息。

### 示例 12-10. 与远程 POP3 邮箱交互

```powershell
## Get the user credential
if(-not (Test-Path Variable:\mailCredential))
{
   $mailCredential = Get-Credential
}
$address = $mailCredential.UserName
$password = $mailCredential.GetNetworkCredential().Password

## Connect to the remote computer, send the commands, and receive the output
$pop3Commands = "USER $address","PASS $password","STAT","QUIT"
$output = $pop3Commands | Send-TcpRequest mail.myserver.com 110
$inbox = $output.Split("`n")[3]

## Parse the output for the number of messages waiting and total bytes
$status = $inbox |
    ConvertFrom-String -PropertyName "Response","Waiting","BytesTotal","Extra"
"{0} messages waiting, totaling {1} bytes." -f $status.Waiting, $status.BytesTotal
```

在 示例 12-10 中，你连接到远程邮件服务器的 110 端口。然后，你发出命令以邮件服务器能理解的形式请求邮箱状态。此网络对话的格式由标准 POP3 协议指定并要求。示例 12-10 使用了 `ConvertFrom-String` 命令，该命令在 配方 5.15 中提供。

示例 12-11 支持 示例 12-10 的核心功能。它让你能轻松处理基于纯文本的 TCP 协议。

### 示例 12-11. Send-TcpRequest.ps1



## Send-TcpRequest

源自 *PowerShell Cookbook*（O'Reilly），作者 Lee Holmes (<http://www.leeholmes.com/guide>)

### 概要

向远程计算机发送 TCP 请求，并返回响应。如果不向此脚本提供输入（通过管道或 `-InputObject` 参数），则脚本将在交互模式下运行。

### 示例

```powershell
$http = @"
  GET / HTTP/1.1
  Host:bing.com
  `n`n
"@

$http | Send-TcpRequest bing.com 80
```

### 参数

```powershell
[CmdletBinding()]
param(
    ## The computer to connect to
    [Parameter()]
    [string] $ComputerName = "localhost",

    ## A switch to determine if you just want to test the connection
    [Parameter()]
    [switch] $Test,

    ## The port to use
    [Parameter()]
    [int] $Port = 80,

    ## A switch to determine if the connection should be made using SSL
    [Parameter()]
    [switch] $UseSSL,

    ## The input string to send to the remote host
    [Parameter(ValueFromPipeline)]
    [string] $InputObject,

    ## The delay, in milliseconds, to wait between commands
    [Parameter()]
    [int] $Delay = 100
)
```

### 脚本主体

```powershell
Set-StrictMode -Version 3

[string] $SCRIPT:output = ""

## Store the input into an array that we can scan over. If there was no input,
## then we will be in interactive mode.
$currentInput = $InputObject
if (-not $currentInput) {
    $currentInput = @($input)
}
$scriptedMode = ([bool] $currentInput) -or $Test

function Main {
    ## Open the socket, and connect to the computer on the specified port
    if (-not $scriptedMode) {
        Write-Host "Connecting to $ComputerName on port $Port"
    }

    try {
        $tcpClient = New-Object Net.Sockets.TcpClient($ComputerName, $Port)
    }
    catch {
        if ($Test) { $false }
        else { Write-Error "Could not connect to remote computer: $_" }
        return
    }

    ## If we're just testing the connection, we've made the connection
    ## successfully, so just return $true
    if ($Test) { $true; return }

    ## If this is interactive mode, supply the prompt
    if (-not $scriptedMode) {
        Write-Host "Connected.  Press ^D followed by [ENTER] to exit.`n"
    }

    $stream = $tcpClient.GetStream()

    ## If we wanted to use SSL, set up that portion of the connection
    if ($UseSSL) {
        try {
            $sslStream = New-Object System.Net.Security.SslStream $stream, $false
            $sslStream.AuthenticateAsClient($ComputerName)
            $stream = $sslStream
        }
        catch [System.IO.IOException] {
            ## Try again with explicit SSL (TLS)
            $tcpClient = New-Object System.Net.Sockets.TcpClient($ComputerName, $Port)
            $stream = $tcpClient.GetStream()

            $writer = New-Object System.IO.StreamWriter $stream

            $writer.WriteLine("EHLO")
            $writer.Flush()

            $writer.WriteLine("STARTTLS")
            $writer.Flush()
            $null = GetOutput

            $sslStream = New-Object System.Net.Security.SslStream $stream, $false
            $sslStream.AuthenticateAsClient($ComputerName)
            $stream = $sslStream
        }
    }

    $writer = New-Object System.IO.StreamWriter $stream

    while ($true) {
        ## Receive the output that has buffered so far
        $SCRIPT:output += GetOutput

        ## If we're in scripted mode, send the commands,
        ## receive the output, and exit.
        if ($scriptedMode) {
            foreach ($line in $currentInput) {
                $writer.WriteLine($line)
                $writer.Flush()
                Start-Sleep -m $Delay
                $SCRIPT:output += GetOutput
            }
            break
        }
        ## If we're in interactive mode, write the buffered
        ## output, and respond to input.
        else {
            if ($output) {
                foreach ($line in $output.Split("`n")) {
                    Write-Host $line
                }
                $SCRIPT:output = ""
            }

            ## Read the user's command, quitting if they hit ^D
            $command = Read-Host
            if ($command -eq ([char] 4)) { break; }

            ## Otherwise, write their command to the remote host
            $writer.WriteLine($command)
            $writer.Flush()
        }
    }

    ## Close the streams
    $writer.Close()
    $stream.Close()

    ## If we're in scripted mode, return the output
    if ($scriptedMode) {
        $output
    }
}

## Read output from a remote host
function GetOutput {
    ## Create a buffer to receive the response
    $buffer = New-Object System.Byte[] 1024
    $encoding = New-Object System.Text.AsciiEncoding

    $outputBuffer = ""
    $foundMore = $false

    ## Read all the data available from the stream, writing it to the
    ## output buffer when done.
    do {
        ## Allow data to buffer for a bit
        Start-Sleep -m 1000

        ## Read what data is available
        $foundMore = $false
        $stream.ReadTimeout = 1000

        do {
            try {
                $read = $stream.Read($buffer, 0, 1024)

                if ($read -gt 0) {
                    $foundMore = $true
                    $outputBuffer += ($encoding.GetString($buffer, 0, $read))
                }
            } catch { $foundMore = $false; $read = 0 }
        } while ($read -gt 0)
    } while ($foundMore)

    $outputBuffer
}

. Main
```

有关运行脚本的更多信息，请参阅 Recipe 1.2。

## 另请参阅

- Recipe 1.2，“运行程序、脚本和现有工具”
- Recipe 5.15，“将文本流转换为对象”
