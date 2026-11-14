

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

[食谱 12.4，“从互联网下载网页”](#ch12.xhtml#download_a_web_page_from_the_internet)

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

[食谱 12.4，“从互联网下载网页”](#ch12.xhtml#download_a_web_page_from_the_internet)

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

如果您将其与记录结果到 CSV 的计划作业相结合，您可以轻松地随时间监控网站的健康状况。有关此方法的示例，请参阅 [食谱 27.14](#ch27.xhtml#manage_scheduled_tasks_on_a_computer)。

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

有关运行脚本的更多信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

### 另请参阅

[食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

## 12.13 程序：与 Internet 协议交互

