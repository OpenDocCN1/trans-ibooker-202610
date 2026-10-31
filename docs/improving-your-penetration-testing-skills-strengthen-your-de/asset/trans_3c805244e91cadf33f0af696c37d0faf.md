

## 使用 Metasploit 中的辅助模块测试 Web 服务器

以下模块对于渗透测试人员测试 Web 服务器是否存在漏洞很有用：

- `dir_listing`: 此模块将连接到目标 Web 服务器并确定其是否启用了目录浏览。
- `dir_scanner`: 使用此模块，您可以扫描目标是否存在有趣的 Web 目录。您可以为模块提供自定义创建的字典或使用默认字典。
- `enum_wayback`: 这是一个有趣的模块，它查询 Internet Archive 网站并查找目标域中的网页。可能已被取消链接的旧网页可能仍然可访问，可以使用 Internet Archive 网站找到。您还可以识别网站多年来所经历的变化。
- `files_dir`: 此模块可用于通过定位配置文件的备份和源代码文件来扫描服务器是否存在数据泄露漏洞。
- `http_login`: 如果网页具有通过 HTTP 运行的登录页面，您可以使用 Metasploit 字典尝试对其进行暴力破解。
- `robots_txt`: robots 文件可能包含一些未探索的 URL，您可以使用此模块查询它们以找到未被搜索引擎索引的 URL。
- `webdav_scanner`: 此模块可用于查找服务器是否启用了 WebDAV，这基本上是将 Web 服务器转变为文件服务器。

## 识别 HTTPS 配置和问题

任何管理任何类型敏感或个人身份信息（姓名、电话号码、地址、健康；信用；或税务记录、信用卡和银行账户信息等）的网站或 Web 应用程序都需要实现一种机制来保护信息在客户端和服务器之间以及反之亦然传输过程中的安全。

HTTP 诞生为一种明文协议。因此，它不包括保护客户端和服务器之间交换的信息免受成功拦截的第三方查看和/或修改的机制。作为对此问题的变通方法，在客户端和服务器之间创建加密通信通道，并通过它发送 HTTP 数据包。HTTPS 是通过安全通信通道实现的 HTTP 协议。它最初是在 **Secure Sockets Layer (SSL)** 上实现的。SSL 于 2014 年被弃用并被 **Transport Layer Security (TLS)** 取代，尽管仍有许多站点支持 SSLv3，无论是由于配置错误还是为了向后兼容。

支持旧加密算法存在一个主要缺点。大多数旧的密码套件被发现可以被密码分析师在合理的时间内使用今天可用的计算能力轻易破解。

专门的攻击者可以从云服务提供商租用廉价的计算能力并用它来破解旧密码并获得明文信息的访问权限。因此，使用旧密码会提供虚假的安全感，应该被禁用。客户端和服务器只能协商被认为安全且在实践中非常难以破解的密码。

Kali Linux 包含许多工具，允许渗透测试人员识别 SSL/TLS 实现中的此类配置错误。在本节中，我们将回顾其中最受欢迎的工具。

### OpenSSL 客户端

包含在几乎所有 GNU/Linux 发行版中的 **OpenSSL** 是基本的 SSL/TLS 客户端，包含有助于您对 HTTPS 服务器执行一些基本测试的功能。

一个基本测试是与服务器建立连接。在此示例中，我们将连接到端口 443 上的测试服务器（默认 HTTPS 端口）：

```
openssl s_client -connect 10.7.7.5:443
```

您可以在以下屏幕截图中显示的结果中看到有关连接参数和证书交换的广泛信息。值得注意的是，连接使用了 SSLv3，这本身就是一个安全问题，因为 SSL 已被弃用，并且存在已知漏洞，可能导致信息的完全解密，例如 Padding Oracle On Downgraded Legacy Encryption (POODLE)，我们将在后面的章节中讨论：

```
root@kali:~# openssl s_client -connect 10.7.7.5:443
CONNECTED(00000003)
depth=0 CN = owaspbwa
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = owaspbwa
verify return:1
---
Certificate chain
 0 s:/CN=owaspbwa
   i:/CN=owaspbwa
---
Server certificate
-----BEGIN CERTIFICATE-----
MIJBnTCCAQYCCQdmhw3dcsK5SzANBgkqhkiG9w0BAQUFADATMR... [certificate data truncated in image]
-----END CERTIFICATE-----
subject=/CN=owaspbwa
issuer=/CN=owaspbwa
---
No client certificate CA names sent
Server Temp Key: DH, 1024 bits
---
SSL handshake has read 1167 bytes and written 374 bytes
Verification error: self signed certificate
---
New, SSLv3, Cipher is DHE-RSA-AES256-SHA
Server public key is 1024 bit
Secure Renegotiation IS supported
Compression: NONE
```

您经常会看到密码套件写成 ECDHE-RSA-RC4-MD5。格式分为以下几部分：

- **ECDHE**: 这是密钥交换算法
- **RSA**: 这是身份验证算法
- **RC4**: 这是加密算法
- **MD5**: 这是哈希算法

SSL 和 TLS 密码套件的综合列表可以在以下位置找到：

https://www.openssl.org/docs/apps/ciphers.html.

您可以使用 OpenSSL 测试目标的一些其他选项如下：

- **禁用或使用特定协议**：使用 `-no_ssl3`、`-no_tls1`、`-no_tls1_1` 和 `-no_tls1_2` 选项，您可以禁用相应协议的使用并测试目标接受哪些协议
- **测试特定协议**：`-tls1`、`-tls1_1` 和 `-tls1_2` 选项仅测试指定的协议

![](img/6b6e557533100c0e47951d0e5e14c4ee_135_0.png)

如今，接受 SSL 和 TLS 1.0 不被认为安全。TLS 1.1 在某些应用中可以接受，但 TLS 1.2 是推荐的选项。

### 使用 SSLScan 扫描 TLS/SSL 配置

SSLScan 是一个命令行工具，它对指定的目标执行各种测试，并返回 SSL/TLS 服务器接受的协议和密码的综合列表以及在安全测试中有用的其他信息：

```
sslscan 10.7.7.5
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_136_0.png)

您可以使用 SSLScan 的颜色代码快速了解显示结果在安全方面的严重程度。红色（允许 SSLv3 并使用 DES 和 RC4 密码）表示不安全的配置，而绿色或白色是推荐的。

命令的输出可以使用 --xml=<filename> 选项导出为 XML 文档。

### 使用 SSLyze 扫描 TLS/SSL 配置

SSLyze 是一个 Python 工具，可以通过类似 SSLScan 的方式连接到服务器来分析服务器的 SSL/TLS 配置。它能够同时扫描多个主机，并且还可以测试性能并使用客户端证书进行相互身份验证。以下命令在测试机器上运行常规 HTTPS 扫描（这包括 SSL 版本 2、SSL 版本 3 和 TLS 1.0、TLS 1.1 和 TLS 1.2 检查、证书的基本信息以及压缩、重协商和 Heartbleed 的测试）：

```
sslize --regular 10.7.7.5
```

您可以在以下屏幕截图中看到结果：

![](img/6b6e557533100c0e47951d0e5e14c4ee_137_0.png)

### 使用 Nmap 测试 TLS/SSL 配置

Nmap 包含一个名为 ssl-enum-ciphers 的脚本，可以识别服务器支持的密码套件，并根据密码强度对其进行评级。它使用 SSLv3、TLS 1.1 和 TLS 1.2 进行多次连接。该脚本还会突出显示如果它识别出 SSL 实现容易受到先前发布漏洞的影响，例如 CRIME 和 POODLE：

```
root@kali:~# nmap --script ssl-enum-ciphers -p 443 10.7.7.5
Starting Nmap 7.60 ( https://nmap.org ) at 2017-10-03 14:09 CAT
Nmap scan report for 10.7.7.5
Host is up (0.00024s latency).

PORT     STATE SERVICE
443/tcp  open  https
| ssl-enum-ciphers:
|   SSLv3:
|     ciphers:
|       TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA (dh 1024) - D
|       TLS_DHE_RSA_WITH_AES_128_CBC_SHA (dh 1024) - A
|       TLS_DHE_RSA_WITH_AES_256_CBC_SHA (dh 1024) - A
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 1024) - D
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 1024) - A
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 1024) - A
|       TLS_RSA_WITH_RC4_128_MD5 (rsa 1024) - D
|       TLS_RSA_WITH_RC4_128_SHA (rsa 1024) - D
|     compressors:
|       DEFLATE
|       NULL
|     cipher preference: client
|     warnings:
|       64-bit block cipher 3DES vulnerable to SWEET32 attack
|       Broken cipher RC4 is deprecated by RFC 7465
|       CBC-mode cipher in SSLv3 (CVE-2014-3566)
|       Ciphersuite uses MD5 for message integrity
|       Weak certificate signature: SHA1
|   TLSv1.0:
|     ciphers:
|       TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA (dh 1024) - D
|       TLS_DHE_RSA_WITH_AES_128_CBC_SHA (dh 1024) - A
|       TLS_DHE_RSA_WITH_AES_256_CBC_SHA (dh 1024) - A
|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 1024) - D
|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 1024) - A
|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 1024) - A
|       TLS_RSA_WITH_RC4_128_MD5 (rsa 1024) - D
|       TLS_RSA_WITH_RC4_128_SHA (rsa 1024) - D
```

