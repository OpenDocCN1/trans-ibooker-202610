

* 通过后门 Lambda 版本建立持久性，[通过后门 Lambda 版本建立持久性](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc21)\-[通过后门 Lambda 版本建立持久性](#ch05.html#id653)

* 使用新用户凭证建立持久性，[使用新的 IAM 用户凭证建立持久性](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc19)\-[使用新的 IAM 用户凭证建立持久性](#ch05.html#id648)

* 组/用户设置，[设置 IAM 组和用户](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc8)\-[设置 IAM 组和用户](#ch05.html#id628)

* Lambda 函数，配置错误，[部署具有过度宽松 IAM 角色的 Lambda 函数](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc11)\-[部署具有过度宽松 IAM 角色的 Lambda 函数](#ch05.html#id633)，[通过配置错误的 IAM 角色的 AWS Lambda 函数提升权限并建立持久性](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc13)\-[通过后门 Lambda 版本建立持久性](#ch05.html#id657)

* 过度宽松的 IAM 角色设置，[设置过度宽松的 IAM 角色](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc5)\-[设置过度宽松的 IAM 角色](#ch05.html#id624)

* 通过 AssumeRole 提升权限，[滥用 AssumeRole 进行权限提升](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc3)\-[使用新的 IAM 用户凭证建立持久性](#ch05.html#id637)

* 通过 Lambda 函数提升权限，[通过配置错误的 IAM 角色的 AWS Lambda 函数提升权限并建立持久性](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc14)\-[通过后门 Lambda 版本建立持久性](#ch05.html#id656)

* 通过过度宽松的执行角色提升权限，[滥用函数的过度宽松执行角色以提升权限](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc17a)\-[滥用函数的过度宽松执行角色以提升权限](#ch05.html#id644)

* AWS Lambda 函数，[入侵公开可访问的 AWS Lambda 函数](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc0)\-[总结](#ch06.html#id705)

  * API Gateway REST API 设置用于将请求路由到，[设置 API Gateway REST API 将请求路由到 Lambda 函数](#ch07.html#ix_ch07-running-and-securing-serverless-functions-in-a-vpc-asciidoc10)\-[设置 API Gateway REST API 将请求路由到 Lambda 函数](#ch07.html#id740)

  * 附加到虚拟私有云 VPC，[将 Lambda 函数附加到具有受限出站访问的 VPC](#ch07.html#ix_ch07-running-and-securing-serverless-functions-in-a-vpc-asciidoc5)\-[将 Lambda 函数附加到具有受限出站访问的 VPC](#ch07.html#id727)

  * 攻击流程模拟，[攻击易受攻击的无服务器 Web 应用程序](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc17)\-[攻击易受攻击的无服务器 Web 应用程序](#ch06.html#id692)

  * 代码注入攻击在其中执行任意代码，[使用代码注入攻击在 Lambda 函数中执行任意代码](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc9)\-[使用代码注入攻击在 Lambda 函数中执行任意代码](#ch06.html#id678)

  * 代码注入保护措施，有限，[使用有限的代码注入保护措施更新 Lambda 函数](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc14)\-[使用有限的代码注入保护措施更新 Lambda 函数](#ch06.html#id681)

  * 代码注入，通过其外泄敏感数据，[通过代码注入和出站网络访问外泄敏感数据](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc11)\-[攻击易受攻击的无服务器 Web 应用程序](#ch06.html#id695)

  * 定义，[设置易受攻击的 AWS Lambda 函数](#ch06.html#id660)

  * 通过后门 Lambda 版本建立持久性，[通过后门 Lambda 版本建立持久性](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc22)\-[通过后门 Lambda 版本建立持久性](#ch05.html#id652)

  * 配置错误，[部署具有过度宽松 IAM 角色的 Lambda 函数](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc12)\-[部署具有过度宽松 IAM 角色的 Lambda 函数](#ch05.html#id632)

  * 出站网络访问，通过其外泄敏感数据，[通过代码注入和出站网络访问外泄敏感数据](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc12)\-[攻击易受攻击的无服务器 Web 应用程序](#ch06.html#id694)

  * 通过其提升权限，[通过配置错误的 IAM 角色的 AWS Lambda 函数提升权限并建立持久性](#ch05.html#ix_ch05-exploiting-and-securing-misconfigured-aws-iam-roles-asciidoc15)\-[通过后门 Lambda 版本建立持久性](#ch05.html#id655)

  * S3 存储桶配置用于静态网站托管，[配置 S3 存储桶用于静态网站托管](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc3)\-[配置 S3 存储桶用于静态网站托管](#ch06.html#id673)

  * S3 存储桶配置错误，测试，[测试常见的 S3 存储桶配置错误](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc7)\-[测试常见的 S3 存储桶配置错误](#ch06.html#id676)

  * 接收外泄数据的服务器，设置，[设置接收外泄数据的服务器](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc16)\-[设置接收外泄数据的服务器](#ch06.html#id691)

  * 无服务器 Web 应用程序，准备/部署，[准备和部署易受攻击的无服务器 Web 应用程序](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc1)\-[配置 S3 存储桶用于静态网站托管](#ch06.html#id674)

  * 通过易受攻击的函数窃取 Secrets Manager 密钥，[通过易受攻击的 Lambda 函数窃取 Secrets Manager 密钥](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc18)\-[通过代码注入攻击窃取 Secrets Manager 密钥](#ch06.html#id704)

  * 更新执行角色，[使用所需的 VPC 权限更新 Lambda 函数执行角色](#ch07.html#ix_ch07-running-and-securing-serverless-functions-in-a-vpc-asciidoc1)\-[使用所需的 VPC 权限更新 Lambda 函数执行角色](#ch07.html#id707)

  * 通过 API 网关端点 URL 更新，[更新 S3 托管网站以使用 API 网关端点 URL](#ch07.html#ix_ch07-running-and-securing-serverless-functions-in-a-vpc-asciidoc13)\-[更新 S3 托管网站以使用 API 网关端点 URL](#ch07.html#id743)

  * 易受攻击的 AWS Lambda 函数设置，[设置易受攻击的 AWS Lambda 函数](#ch06.html#ix_ch06-hacking-publicly-accessible-lambda-functions-asciidoc2)\-[设置易受攻击的 AWS Lambda 函数](#ch06.html#id667)

