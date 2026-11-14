
# 18.13 在磁盘上存储密码

在磁盘上存储密码的第一步通常是手动操作。文件名没有强制要求，但我们将遵循一种约定，将文件命名为 `CurrentScript.ps1.credential`。给定一个存储在 `$credential` 变量中的凭据，你可以安全地使用 `Export-CliXml` cmdlet 将凭据保存到磁盘。将 `CurrentScript` 替换为将要加载它的脚本的名称：

```
PS > $credPath = Join-Path (Split-Path $profile) CurrentScript.ps1.credential
PS > $credential | Export-CliXml $credPath
```

## 从磁盘上存储的密码重新创建凭据

在你想要自动运行的脚本中，添加以下命令：

```
$credPath = Join-Path (Split-Path $profile) CurrentScript.ps1.credential
$credential = Import-CliXml $credPath
```

这些命令创建一个新的凭据对象（针对 `CachedUser` 用户），并将该对象存储在 `$credential` 变量中。

## 讨论

阅读解决方案时，你可能最初会对在磁盘上存储密码感到担忧。虽然对在硬盘上散布敏感信息保持谨慎是自然（且明智）的，但 `Export-CliXml` cmdlet 使用 Windows 标准数据保护 API (DPAPI) 对凭据对象进行加密。这确保只有你的用户帐户能正确解密其内容。同样，`ConvertFrom-SecureString` cmdlet 也会加密你提供的密码。

虽然保护密码安全是一项重要的安全功能，但有时你可能希望将密码（或其他敏感信息）存储在磁盘上，以便其他帐户可以访问它。这在由服务帐户运行的脚本或设计为在计算机之间传输的脚本中经常发生。`ConvertFrom-SecureString` 和 `ConvertTo-SecureString` cmdlet 通过允许你指定加密密钥来支持此功能。

### 注意

当使用硬编码的加密密钥时，此技术不再起安全措施的作用。如果用户可以访问你的自动化脚本的内容，该用户就能访问加密密钥。如果用户有权访问加密密钥，该用户就能访问你试图保护的数据。

虽然解决方案将密码存储在包含你配置文件的目录中，但你也可以从与脚本相同的位置加载它。要了解如何从与脚本相同的位置加载它，请参阅 [Recipe 16.6](#ch16.xhtml#find_your_scripts_location)。

有关 `ConvertTo-SecureString` 和 `ConvertFrom-SecureString` cmdlet 的详细信息，请键入 `Get-Help ConvertTo-SecureString` 或 `Get-Help` `ConvertFrom-SecureString`。

## 另请参阅

[Recipe 16.6, “查找脚本的位置”](#ch16.xhtml#find_your_scripts_location)

# 18.14 访问用户和计算机证书

## 问题

你想要检索当前用户或本地计算机的证书信息。

## 解决方案

若要浏览和检索本地计算机上的证书，请使用 PowerShell 的证书驱动器。此驱动器由证书提供程序创建，如 [示例 18-5](#ch18.xhtml#exploring_certificates_in_the_certificat) 所示。

### 示例 18-5. 在证书提供程序中探索证书

```
PS > Set-Location cert:\CurrentUser\
PS > $cert = Get-ChildItem -Rec -CodeSign
PS > $cert | Format-List

Subject      : CN=PowerShell User
Issuer       : CN=PowerShell Local Certificate Root
Thumbprint   : FD48FAA9281A657DBD089B5A008FAFE61D3B32FD
FriendlyName :
NotBefore    : 4/22/2007 12:32:37 AM
NotAfter     : 12/31/2039 3:59:59 PM
Extensions   : {System.Security.Cryptography.Oid, System.Security.
               Cryptography.Oid}
```

## 讨论

证书驱动器提供了一种浏览和查看当前用户或本地计算机证书的有用方法。例如，如果你的执行策略要求使用数字签名，以下命令将告诉你哪些发行者受信任可以在你的系统上运行脚本：

```
Get-ChildItem cert:\CurrentUser\TrustedPublisher
```

证书提供程序最常用于为 `Set-AuthenticodeSignature` cmdlet 选择代码签名证书。以下命令选择“最佳”代码签名证书（即过期时间最晚的那个）：

```
$certificates = Get-ChildItem Cert:\CurrentUser\My -CodeSign
$signingCert = @($certificates | Sort-Object -Desc NotAfter)[0]
```

`-CodeSign` 参数允许你搜索证书存储中支持代码签名的证书。若要搜索用于其他目的的证书，请参阅 [Recipe 18.15](#ch18.xhtml#program_search_the_certificate_store)。

虽然证书提供程序可用于浏览和检索计算机证书存储中的信息，但它不允许你向这些位置添加项目。如果你想管理证书存储中的证书，.NET Framework 中的 `System.Security.Cryptography.X509Certificates.X509Store` 类（以及来自 `System.Security.Cryptography.X509Certificates` 命名空间的其他相关类）支持该功能。有关此方法的示例，请参阅 [Recipe 18.16](#ch18.xhtml#add_remove_certificates)。

有关证书提供程序的详细信息，请键入 `Get-Help Certificate`。

## 另请参阅

[Recipe 18.15, “程序：搜索证书存储”](#ch18.xhtml#program_search_the_certificate_store)

[Recipe 18.16, “添加和删除证书”](#ch18.xhtml#add_remove_certificates)

# 18.15 程序：搜索证书存储

证书提供程序的一个有用功能是它支持 `-CodeSign` 参数，该参数允许你搜索证书存储中支持代码签名的证书。

此参数称为*动态参数*：由提供程序添加到核心 PowerShell cmdlet 的参数。你可以通过导航到该提供程序，然后查看 `Get-Command -Syntax` 的输出来发现提供程序的动态参数。例如：

```
PS > Set-Location cert:\
PS > Get-Command Get-ChildItem -Syntax
Get-ChildItem [[-Path] <String[]>] [[-Filter] <String>] (...) [-CodeSigningCert]
```

除了 `Get-Command` 的输出外，提供程序的帮助主题通常还描述了它支持的动态参数。若要获取提供程序帮助主题的列表，请键入 `Get-Help -Category Provider`。

然而，代码签名证书并不是唯一的证书类型；其他常用的证书类型包括加密文件系统、客户端身份验证等。

[示例 18-6](#ch18.xhtml#search-certificatestoreps1) 允许你搜索证书提供程序，以查找支持给定增强密钥用法 (EKU) 的证书。

### 示例 18-6. Search-CertificateStore.ps1

```
##############################################################################
##
## Search-CertificateStore
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Search the certificate provider for certificates that match the specified
Enhanced Key Usage (EKU).

.EXAMPLE

PS > Search-CertificateStore "Encrypting File System"
Searches the certificate store for Encrypting File System certificates

#>

param(
    ## The friendly name of an Enhanced Key Usage
    ## (such as 'Code Signing')
    [Parameter(Mandatory = $true)]
    $EkuName
)

Set-StrictMode -Off

## Go through every certificate in the current user's "My" store
foreach($cert in Get-ChildItem cert:\CurrentUser\My)
{
    ## For each of those, go through its extensions
    foreach($extension in $cert.Extensions)
    {
        ## For each extension, go through its Enhanced Key Usages
        foreach($certEku in $extension.EnhancedKeyUsages)
        {
            ## If the friendly name matches, output that certificate
            if($certEku.FriendlyName -eq $ekuName)
            {
                $cert
            }
        }
    }
}
```
