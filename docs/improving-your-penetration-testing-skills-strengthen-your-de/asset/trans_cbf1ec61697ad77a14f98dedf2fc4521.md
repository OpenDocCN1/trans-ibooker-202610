

以下示例使用 s_client 命令行选项，该选项使用 SSL/TLS 建立与远程服务器的连接。对新手来说，该命令的输出难以解读，但它对于识别服务器与客户端之间协商的 TLS/SSL 版本和密码套件很有用：

![](img/6b6e557533100c0e47951d0e5e14c4ee_304_0.png)

OpenSSL 实用程序包含各种命令行选项，可用于使用特定的 SSL 版本和密码套件测试服务器。在以下示例中，我们尝试使用 TLS 版本 1.2 和一种弱算法 RC4 进行连接：

```
openssl s_client -tls1_2 -cipher 'ECDHE-RSA-AES256-SHA' -connect <target>:<port>
```

以下屏幕截图显示了命令的输出。由于客户端无法与 ECDHE-RSA-AES256-SHA 密码套件协商，因此握手失败，未选择任何密码：

![](img/6b6e557533100c0e47951d0e5e14c4ee_305_0.png)

在以下屏幕截图中，我们尝试与服务器协商一种弱加密算法。它失败了，因为 Google 已正确在服务器上禁用了弱密码套件：

```
root@kali:~# openssl s_client -tls1_2 -cipher "NULL,EXPORT,LOW,DES" -connect www.google.com:443
CONNECTED(00000003)
139783222056192:error:141640B5:SSL routines:tls_construct_client_hello:no ciphers available:../ssl/m/state_m_clnt.c:800:
---
no peer certificate available
---
No client certificate CA names sent
---
SSL handshake has read 0 bytes and written 0 bytes
Verification: OK
---
New, (NONE), Cipher is (NONE)
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : 0000
    Session-ID:
    Session-ID-ctx:
    Master-Key:
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    Start Time: 1517833355
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: no
---
```

要查找出使用当前可用计算能力即可轻易破解的密码套件，请输入以下屏幕截图中所示的命令：

```
root@kali:~# openssl ciphers -v "NULL,EXPORT,LOW,DES"
ECDHE-ECDSA-NULL-SHA    TLSv1 Kx=ECDH   Au=ECDSA Enc=None  Mac=SHA1
ECDHE-RSA-NULL-SHA      TLSv1 Kx=ECDH   Au=RSA   Enc=None  Mac=SHA1
AECDH-NULL-SHA          TLSv1 Kx=ECDH   Au=None  Enc=None  Mac=SHA1
NULL-SHA256             TLSv1.2 Kx=RSA  Au=RSA   Enc=None  Mac=SHA256
ECDHE-PSK-NULL-SHA384   TLSv1 Kx=ECDHEPSK Au=PSK   Enc=None  Mac=SHA384
ECDHE-PSK-NULL-SHA256   TLSv1 Kx=ECDHEPSK Au=PSK   Enc=None  Mac=SHA256
ECDHE-PSK-NULL-SHA      TLSv1 Kx=ECDHEPSK Au=PSK   Enc=None  Mac=SHA1
RSA-PSK-NULL-SHA384     TLSv1 Kx=RSAPSK Au=RSA   Enc=None  Mac=SHA384
RSA-PSK-NULL-SHA256     TLSv1 Kx=RSAPSK Au=RSA   Enc=None  Mac=SHA256
DHE-PSK-NULL-SHA384     TLSv1 Kx=DHEPSK Au=PSK   Enc=None  Mac=SHA384
DHE-PSK-NULL-SHA256     TLSv1 Kx=DHEPSK Au=PSK   Enc=None  Mac=SHA256
RSA-PSK-NULL-SHA        SSLv3 Kx=RSAPSK Au=RSA   Enc=None  Mac=SHA1
DHE-PSK-NULL-SHA        SSLv3 Kx=DHEPSK Au=PSK   Enc=None  Mac=SHA1
NULL-SHA                SSLv3 Kx=RSA    Au=RSA   Enc=None  Mac=SHA1
NULL-MD5                SSLv3 Kx=RSA    Au=RSA   Enc=None  Mac=MD5
PSK-NULL-SHA384         TLSv1 Kx=PSK    Au=PSK   Enc=None  Mac=SHA384
PSK-NULL-SHA256         TLSv1 Kx=PSK    Au=PSK   Enc=None  Mac=SHA256
PSK-NULL-SHA            SSLv3 Kx=PSK    Au=PSK   Enc=None  Mac=SHA1
```

你经常会看到密码套件写成 **ECDHE-RSA-RC4-MD5**。其格式可分解为以下几部分：

- **ECDHE**：这是一种密钥交换算法
- **RSA**：这是一种身份验证算法
- **RC4**：这是一种加密算法
- **MD5**：这是一种哈希算法

SSL 和 TLS 密码套件的完整列表可在 https://www.openssl.org/docs/apps/ciphers.html 找到。

## SSLScan

尽管 OpenSSL 命令行工具提供了许多用于测试 SSL 配置的选项，但该工具的输出并不友好。该工具还要求对要测试的密码套件有相当多的了解。

Kali Linux 附带了许多工具，可自动执行识别 SSL 配置错误、过时协议版本以及弱密码套件和哈希算法的任务。其中一个工具是 **SSLScan**，可通过 **应用程序 | 信息收集 | SSL 分析** 访问。

默认情况下，SSLScan 会检查服务器是否易受 CRIME 和 Heartbleed 漏洞的影响。`-tls` 选项将强制 SSLScan 仅使用 TLS 协议测试密码套件。

输出以各种颜色分布，其中绿色表示密码套件安全，而红色和黄色部分则试图引起你的注意：

![](img/6b6e557533100c0e47951d0e5e14c4ee_308_0.png)

客户端支持的密码套件可通过运行以下命令来识别。它将显示客户端支持的长密码列表：

```
sslscan -show-ciphers www.example.com:443
```

如果要分析证书相关数据，请使用以下命令，它将显示证书的详细信息：

```
sslscan --show-certificate --no-ciphersuites www.amazon.com:443
```

命令的输出可使用 -xml=<filename> 选项导出为 XML 文档。

请注意支持的密码名称中出现的 NULL。如果选择了 NULL 密码，SSL/TLS 握手将完成，浏览器将显示安全挂锁，但 HTTP 数据将以明文传输。

## SSLyze

另一个随 Kali Linux 提供的、有助于分析 SSL 配置的有趣工具是 iSEC Partners 发布 的 SSLyze 工具。该工具托管在 GitHub 上的 https://github.com/iSECPartners/sslyze ，可在 Kali Linux 中在 应用程序 | 信息收集 | SSL 分析 中找到。SSLyze 用 Python 编写。

该工具附带了各种插件，可帮助测试以下内容：

- 检查旧版本的 SSL
- 分析密码套件并识别弱密码
- 使用输入文件扫描多个服务器
- 检查会话恢复支持

使用 -regular 选项可包含你可能感兴趣的所有常见选项，例如测试所有可用协议（SSL 版本 2 和 3 以及 TLS 1.0、1.1 和 1.2）、测试不安全的密码套件以及识别是否启用了压缩。

在以下示例中，服务器不支持压缩，并且易受 Heartbleed 攻击。输出还列出了接受的密码套件：

```
SCAN RESULTS FOR 10.7.7.8:8443 - 10.7.7.8:8443
------------------------------------------------
* Session Renegotiation:
    Client-initiated Renegotiations:  OK - Rejected
    Secure Renegotiation:            OK - Supported
* Deflate Compression:
    OK - Compression disabled
* Session Resumption:
    With Session IDs:                NOT SUPPORTED (0 successful, 5 failed, 0 errors
    With TLS Session Tickets:        OK - Supported
* OpenSSL Heartbleed:
    VULNERABLE - Server is vulnerable to Heartbleed
* TLSV1_2 Cipher Suites:
    Preferred:
        ECDHE-RSA-AES256-GCM-SHA384    ECDH-256 bits  256 bits    HTTP 200 OK
    Accepted:
        ECDHE-RSA-AES256-SHA384        ECDH-256 bits  256 bits    HTTP 200 OK
        ECDHE-RSA-AES256-SHA            ECDH-256 bits  256 bits    HTTP 200 OK
        ECDHE-RSA-AES256-GCM-SHA384     ECDH-256 bits  256 bits    HTTP 200 OK
        DHE-RSA-CAMELLIA256-SHA         DH-1024 bits    256 bits    HTTP 200 OK
        DHE-RSA-AES256-SHA256           DH-1024 bits    256 bits    HTTP 200 OK
        DHE-RSA-AES256-SHA              DH-1024 bits    256 bits    HTTP 200 OK
        DHE-RSA-AES256-GCM-SHA384       DH-1024 bits    256 bits    HTTP 200 OK
        CAMELLIA256-SHA                 -               256 bits    HTTP 200 OK
        AES256-SHA256                   -               256 bits    HTTP 200 OK
        AES256-SHA                      -               256 bits    HTTP 200 OK
        AES256-GCM-SHA384               -               256 bits    HTTP 200 OK
        ECDHE-RSA-AES128-SHA256         ECDH-256 bits   128 bits    HTTP 200 OK
        ECDHE-RSA-AES128-SHA            ECDH-256 bits   128 bits    HTTP 200 OK
        ECDHE-RSA-AES128-GCM-SHA256     ECDH-256 bits   128 bits    HTTP 200 OK
        DHE-RSA-CAMELLIA128-SHA         DH-1024 bits    128 bits    HTTP 200 OK
        DHE-RSA-AES128-SHA256           DH-1024 bits    128 bits    HTTP 200 OK
```

## 使用 Nmap 测试 SSL 配置

Nmap 包含一个名为 ssl-enum-ciphers 的脚本，可识别服务器支持的密码套件，并根据其密码强度对其进行评级。它使用 SSLv3、TLS 1.1 和 TLS 1.2 进行多次连接。还有可识别已知漏洞（例如 Heartbleed 或 POODLE）的脚本。

