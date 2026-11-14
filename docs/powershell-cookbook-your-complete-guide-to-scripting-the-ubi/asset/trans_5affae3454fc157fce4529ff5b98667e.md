
## 另请参考

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

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

如果您下载的文件最终是一个您想要解析或阅读的网页，`Invoke-WebRequest` cmdlet 还有其他专门为该场景设计的功能。有关如何下载和解析网页的更多信息，请参阅 [食谱 12.4](#ch12.xhtml#download_a_web_page_from_the_internet)。

### 另请参考

- [食谱 12.4，“从 Internet 下载网页”](#ch12.xhtml#download_a_web_page_from_the_internet)

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

如果您想从 FTP 站点下载文件，请参阅 [食谱 12.1](#ch12.xhtml#download_a_file_from_the_internet)。

### 另请参考

- [食谱 12.1，“从 FTP 或 Internet 站点下载文件”](#ch12.xhtml#download_a_file_from_the_internet)

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

有关运行脚本的更多信息，请参阅 [食谱 1.2](#ch01.xhtml#run_programs_scripts_and_existing_tools)。

### 另请参考

- [食谱 1.2，“运行程序、脚本和现有工具”](#ch01.xhtml#run_programs_scripts_and_existing_tools)

## 12.4 从 Internet 下载网页

### 问题

您想从 Internet 下载网页并直接处理其内容。

### 解决方案
