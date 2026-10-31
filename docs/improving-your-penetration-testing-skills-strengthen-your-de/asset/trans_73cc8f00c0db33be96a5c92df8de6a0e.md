

此外，一旦扫描完成，报告将在指定的文件夹中准备就绪。下面的屏幕截图显示了 Skipfish 报告的示例：

![](img/6b6e557533100c0e47951d0e5e14c4ee_339_0.png)

报告按 Skipfish 识别出的漏洞风险从高到低排序（红色点到橙色点）。例如，Skipfish 在登录页面中识别出 SQL 注入漏洞，查询注入向量，扫描器将其评定为高风险。它还识别出目录遍历或文件包含以及可能的 XSS 漏洞，评定为中等风险，等等。

## Wapiti

Wapiti 是一款基于命令行的主动维护的 Web 漏洞扫描工具。Wapiti 3.0 版本于 2018 年 1 月发布（http://wapiti.sourceforge.net/）；然而，Kali Linux 仍包含之前的版本（2.3.0）。根据 Wapiti 网站，该工具包含用于检测以下漏洞的模块：

- 文件泄露（本地和远程包含/require、fopen、readfile...）
- 数据库注入（PHP/JSP/ASP SQL 注入和 XPath 注入）
- XSS（跨站脚本）注入（反射型和永久型）
- 命令执行检测（eval()、system()、passthru()...）
- CRLF 注入（HTTP 响应拆分、会话固定...）
- XXE（XML 外部实体）注入
- 使用已知潜在危险文件（借助 Nikto 数据库）
- 可被绕过的弱 .htaccess 配置
- 提供敏感信息的备份文件存在（源代码泄露）
- Shellshock（又名 Bash 漏洞）

要启动 Wapiti，您需要在命令行中发出启动命令，后跟要扫描的 URL 和选项。

在下面的屏幕截图中，Wapiti 在易受攻击的虚拟机上对 BodgeIt 的 HTTPS 站点进行运行，报告生成在 wapiti_output 目录中（-o 选项）。您可以跳过 SSL 证书验证，因为测试虚拟机使用的是自签名证书。Wapiti 将在未验证的情况下停止扫描，因此使用 --verify-ssl 0 来绕过此类验证。您不应发送超过 50 个相同请求的变体（-n 选项）。这是为了防止循环。最后，使用 2> null 可以防止标准错误输出过度填充屏幕，因为扫描器会发出多个带有非预期值的请求，而 Wapiti 的输出可能非常冗长：

```
wapiti https://10.7.7.5/bodgeit/ -o wapiti_output --verify-ssl 0 -n 20 2> null
```

然后您将在屏幕上看到以下输出：

```
root@kali:~/WebPentest# wapiti https://10.7.7.5/bodgeit/ -o wapiti_output --verify-ssl 0 -n 20 2> null
Wapiti-2.3.0 (wapiti.sourceforge.net)

Note
======
This scan has been saved in the file /root/.wapiti/scans/10.7.7.5.xml
You can use it to perform attacks without scanning again the web site with the "-k" parameter
[*] Loading modules:
mod_crlf, mod_exec, mod_file, mod_sql, mod_xss, mod_backup, mod_htaccess, mod_blindsql, mod_pentxss, mod_nikto

[+] Launching module exec
Received a HTTP 500 error in https://10.7.7.5/bodgeit/advanced.jsp
Evil url: https://10.7.7.5/bodgeit/advanced.jsp%3Benv
Received a HTTP 500 error in https://10.7.7.5/bodgeit/basket.jsp
Evil request:
POST /bodgeit/basket.jsp HTTP/1.1
Host: 10.7.7.5
Referer: https://10.7.7.5/bodgeit/product.jsp?prodid=2
Content-Type: application/x-www-form-urlencoded

productid=2&price=3.1&quantity=%3Benv
```

扫描需要一些时间。完成后，打开指定目录中的 index.html 文件即可查看结果。以下是 Wapiti 报告漏洞的示例：

![](img/6b6e557533100c0e47951d0e5e14c4ee_341_0.png)

Wapiti 的报告非常详细，并且包含每个发现的描述、触发潜在漏洞的请求、提出的解决方案以及获取更多信息的参考。在前面的屏幕截图中，您可以看到它在 BodgeIt 的搜索页面中发现了 XSS。

## OWASP-ZAP scanner

OWASP-ZAP 的众多功能中，有一个主动漏洞扫描器。在此情况下，主动是指扫描器会主动向服务器发送精心构造的请求，与被动扫描器不同，后者仅在通过代理正常浏览应用程序时分析 Web 服务器发送的请求和响应。

要使用扫描器，您需要右键单击要扫描的站点或目录并选择 Attack | Active Scan...：

![](img/6b6e557533100c0e47951d0e5e14c4ee_342_0.png)

主动扫描器不会对所选目标进行任何爬取或蜘蛛扫描。因此，建议您在设置代理的情况下手动浏览目标站点，或在扫描目录或主机之前先运行蜘蛛。

在 **Active Scan** 对话框中，您可以选择目标、是否递归扫描，以及如果启用高级选项，您可以选择扫描策略、攻击向量、目标技术和其他选项：

![](img/6b6e557533100c0e47951d0e5e14c4ee_343_0.png)

一旦单击 **Start Scan**，**Active Scan** 选项卡将获得焦点，扫描进度和请求日志将显示在其中：

![](img/6b6e557533100c0e47951d0e5e14c4ee_343_1.png)

扫描结果将记录在 Alerts 选项卡中：

![](img/6b6e557533100c0e47951d0e5e14c4ee_344_0.png)

此外，使用主菜单中的 Report，您可以将结果导出为 HTML、XML、Markdown 或 JSON 等多种格式。下面的屏幕截图显示了 HTML 报告的示例：

![](img/6b6e557533100c0e47951d0e5e14c4ee_344_1.png)

OWASP-ZAP 也按风险等级对扫描结果进行排序，并包含发现问题的详细描述、使用的有效载荷、解决方案建议和参考。

![](img/6b6e557533100c0e47951d0e5e14c4ee_345_0.png)

Burp Suite 在其专业版中也具有主动扫描器，可提供非常准确的结果且误报率低。

## Content Management Systems scanners

内容管理系统（CMSs），如 WordPress、Joomla 或 Drupal，是用于在无需编程或只需少量编程的情况下创建网站的框架。它们集成第三方插件以简化登录和会话管理、搜索等任务，甚至包含完整的购物车模块。

因此，CMSs 不仅在其自身代码中存在漏洞，也存在于其包含的插件中。后者不受一致的质量控制，且通常由独立程序员在业余时间制作，按其自己的时间表发布更新和补丁。

因此，我们现在将介绍一些最受欢迎的 CMS 漏洞扫描器。

## WPScan

WPScan 顾名思义，是专注于 WordPress CMS 的漏洞扫描器。它会识别 WordPress 的版本号以及已安装插件的版本号，然后将其与已知漏洞数据库进行匹配，以识别可能的 安全风险。

下面的屏幕截图显示了 WPScan 的基本用法，只需将目标 URL 作为参数添加即可：

![](img/6b6e557533100c0e47951d0e5e14c4ee_346_0.png)

首次运行时，您可能需要使用 `-update` 选项更新数据库。

## JoomScan

JoomScan 是 Kali Linux 中包含的 Joomla 站点漏洞扫描器。要使用它，您只需添加 -u 选项，后跟站点的 URL，如下所示：

```
joomscan -u http://10.7.7.5/joomla
```

JoomScan 首先尝试通过检测 Joomla 版本和插件来指纹识别服务器，如下图所示：

![](img/6b6e557533100c0e47951d0e5e14c4ee_347_0.png)

之后，JoomScan 将显示与检测到的配置或已安装插件相关的漏洞：

```
Vulnerabilities Discovered
=======================

# 1
Info -> Generic: htaccess.txt has not been renamed.
Versions Affected: Any
Check: /htaccess.txt
Exploit: Generic defenses implemented in .htaccess are not available, succeed.
Vulnerable? Yes

# 2
Info -> Generic: Unprotected Administrator directory
Versions Affected: Any
Check: /administrator/
Exploit: The default /administrator directory is detected. Attackers counts. Read: http://yehg.net/lab/pr0js/view.php/MULTIPLE%20TRICKY%20W
Vulnerable? Yes

# 3
Info -> Core: Multiple XSS/CSRF Vulnerability
Versions Affected: 1.5.9 <=
Check: /?1.5.9-x
Exploit: A series of XSS and CSRF faults exist in the administrator ap
tor components include com_admin, com_media, com_search. Both com_ad
vulnerabilities, and com_media contains 2 CSRF vulnerabilities.
Vulnerable? No

# 4
Info -> Core: JSession SSI Session Disclosure Vulnerability
```

## CMSmap

CMSmap 未包含在 Kali Linux 中，但可以从其 Git 仓库轻松安装，如下所示：

```
git clone https://github.com/Dionach/CMSmap.git
```
