

使用 `Invoke-WebRequest` cmdlet 下载网页，然后访问 `Content` 属性（或将结果转换为 `[string]`）：

```powershell
PS > $source = "http://www.bing.com/search?q=sqrt(2)"
PS > $result = [string] (Invoke-WebRequest $source)
```

## 讨论

在编写面向互联世界的自动化脚本时，我们并不总是有幸能访问返回丰富结构化数据的 Web 服务。因此，从互联网上的服务检索数据通常通过*屏幕抓取*来实现：下载网页的 HTML，然后小心翼翼地从绝大多数不需要的内容中分离出你想要的内容。

### 注意

如果从网页提取结构化数据是你的主要目标，`Invoke-WebRequest` cmdlet 提供的选项比基本的屏幕抓取强大得多。有关详细信息，请参阅 [食谱 12.5](#ch12.xhtml#parse_analyze_web_page)。

屏幕抓取技术的历史比互联网还要悠久！只要计算机系统生成的输出主要是为人类设计的，屏幕抓取工具就会应运而生，使这些输出可供其他计算机程序使用。

不幸的是，屏幕抓取是一种容易出错的内容提取方式。这绝非夸张！作为证据，[示例 12-6](#ch12.xhtml#get-answerps1)（稍后在本食谱中展示）在本书第一版撰写期间就损坏了四五次，出版后又损坏了一次。随后在第二版期间又损坏了几次，出版后再次损坏。然后，第三版出版后又损坏了。虽然第四版修复了它，但当你阅读此文时，它很可能已经再次损坏。这就是屏幕抓取的风险。

如果网页作者更改了底层 HTML，你的代码通常将停止正常工作。如果网站的 HTML 编写为有效的 XHTML，你可能可以利用 PowerShell 内置的 XML 支持更轻松地解析内容。

有关 PowerShell 内置 XML 支持的更多信息，请参阅 [食谱 10.1](#ch10.xhtml#access_information_in_an_xml_file)。

尽管脆弱，纯屏幕抓取往往是唯一的选择。既然屏幕抓取只是文本操作，你拥有的选项与处理其他文本报告时相同。对于一些结构相对良好的网页，你可以通过单个正则表达式替换（加上清理）来搞定，如 [示例 12-5](#ch12.xhtml#search-bingps1) 所示。

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

虽然可以使用复杂的正则表达式解析结构较松散的网页，但通过更直接的文本操作通常可以让事情简单得多。[示例 12-6](#ch12.xhtml#get-answerps1) 使用这种第二种方法从 Bing 获取“即时答案”。

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

