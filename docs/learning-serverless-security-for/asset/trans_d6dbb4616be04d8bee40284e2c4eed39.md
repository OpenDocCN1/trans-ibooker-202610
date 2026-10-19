

# 设置 S3 主机前端

让我们继续设置 S3 主机前端：

1. 在 AWS 管理控制台中，在搜索栏中输入 `cloudshell`，然后从匹配的服务列表中选择 CloudShell。

###### Tip

如果出现 Welcome to CloudShell 弹出窗口，只需单击 Close 即可继续。

2. 在 CloudShell 终端中（在 `$` 符号之后）运行以下命令以克隆 `vulnerable-lambda-function-frontend` 仓库：

```
USERNAME=learning-serverless-security
REPO=vulnerable-lambda-function-frontend

git clone https://github.com/$USERNAME/$REPO.git

cd vulnerable-lambda-function-frontend
```

###### Note

本节使用的所有代码示例和命令都可以在 [`vulnerable-lambda-function-frontend` GitHub 仓库](https://oreil.ly/0Ew49) 中找到。

3. 使用 `tree` 命令检查项目结构：

```
sudo yum install tree -y

tree
```

这会输出以下树状的文件和文件夹结构：

```
.
├── backup
│   └── lambda_function.py
├── index.html
└── README.md

1 directory, 3 files
```

4. 将 `[SPECIFY LAMBDA FUNCTION URL]` 替换为“[设置易受攻击的 AWS Lambda 函数”](#ch06.html#setting-up-a-vulnerable-aws-lambda-function)中的实际函数 URL，然后运行以下命令以更新 `index.html` 文件中指定的占位符值：

```
FUNCTION_URL="[SPECIFY LAMBDA FUNCTION URL]"

sed -i "s|<INSERT FUNCTION URL HERE>|$FUNCTION_URL|g" index.html
```

5. 检查 `index.html` 的内容并确认代码已正确更新：

```
less -N index.html
```

确保 `index.html` 文件第 98 行的占位符 `<INSERT FUNCTION URL HERE>` 已被实际的 Lambda 函数 URL 替换。^[4](#ch06.html#id668)

###### Tip

在验证代码更新后，按 Q 退出 `less` 查看器并返回终端。

6. 创建一个新的 S3 存储桶：

```
BUCKET_NAME="serverless-static-website-$(date +%s)"
REGION="us-east-1"

aws s3 mb s3://$BUCKET_NAME --region $REGION
```

运行该命令会在 `us-east-1` 中创建 S3 存储桶——对应美国东部（弗吉尼亚北部）AWS 区域。

###### Note

附加 `date +%s` 命令的输出有助于确保生成的 S3 存储桶名称在所有 AWS 账户中都是全局唯一的，因为它返回自 1970 年 1 月 1 日以来的秒数。例如，如果你运行此命令，你会得到类似 `1751705486` 的值。这确保即使两个用户运行相同的命令来创建新的 S3 存储桶，由于唯一的时间戳，结果的存储桶名称也会不同。

7. 配置 S3 存储桶以进行公共访问和静态网站托管：

```
aws s3api put-public-access-block \
  --bucket "$BUCKET_NAME" \
  --public-access-block-configuration '{
    "BlockPublicAcls": false,
    "IgnorePublicAcls": false,
    "BlockPublicPolicy": false,
    "RestrictPublicBuckets": false
  }'

aws s3 cp index.html s3://$BUCKET_NAME/index.html

aws s3 website "s3://$BUCKET_NAME/" --index-document index.html
```

8. 将 Lambda 函数源代码的备份副本上传到 S3 存储桶：

```
aws s3 cp backup s3://$BUCKET_NAME/backup --recursive
```

###### Caution

这是在实践中*不应该*做的事情的一个例子。切勿将应用程序的源代码存储到配置为静态网站托管的存储桶中。正如你在本章后面会看到的，攻击者可以利用 S3 存储桶的配置错误来发现源代码备份文件夹并下载其内容。

9. 设置一个公共存储桶策略，允许用户访问 S3 主机 Web 应用程序：

```
cat > bucket-policy.json <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::$BUCKET_NAME",
        "arn:aws:s3:::$BUCKET_NAME/*"
      ]
    }
  ]
}
EOF

aws s3api put-bucket-policy \
  --bucket "$BUCKET_NAME" \
  --policy file://bucket-policy.json
```

10. 打印你的 S3 主机网站的 URL：^[5](#ch06.html#id669)

```
echo "http://$BUCKET_NAME.s3-website-$REGION.amazonaws.com"
```

###### Tip

你也可以通过 AWS 管理控制台检索存储桶网站端点，方法是转到 S3 存储桶的 Properties 选项卡，并在“Static website hosting”下查看“Bucket website endpoint”。

11. 复制上一步的 URL 输出并将其粘贴到浏览器地址栏中。这应该会打开一个 S3 主机网站（[Figure 6-3](#ch06.html#vulnerable-s3-hosted-website-00)）。

![](assets/lsse_0603.png)

该页面有一个文本区域，可在其中输入数学表达式。单击 Evaluate 按钮会将输入表达式发送到后端 Lambda 函数（`lambda-0010`）进行处理。Lambda 函数通过使用 `eval()`（在 `process_statement()` 函数内部）来评估表达式并将输出返回给前端。然后结果会显示在页面上，以便你立即查看评估输出。

###### Tip

尝试在文本区域中输入并评估一个简单的表达式（例如 `1 + 1`）以确认你的设置按预期工作。

在实践中，S3 主机网站可能会位于 Amazon CloudFront 分发后面，以启用 HTTPS、提供安全的内容交付，并通过将静态资源缓存到更靠近用户的边缘位置来提高性能。借助 Route 53，你可以配置域名系统（DNS）记录，将来自自定义域的流量路由到 CloudFront 分发，如 [Figure 6-4](#ch06.html#full-frontend-setup) 所示。

![](assets/lsse_0604.png)

为了避免不必要的开销，我们将跳过完整的生产设置，而是专注于模拟攻击者如何利用 S3 存储桶配置错误以及 Lambda 函数中的代码注入漏洞。本章中此无服务器实验室环境设置的初始设置如 [Figure 6-5](#ch06.html#lab-setup-01) 所示。

![](assets/lsse_0605.png)

有了此设置，用户可以通过浏览器访问 S3 主机网站，在文本区域中输入有效的数学表达式，单击 Evaluate 按钮，并查看 Lambda 函数返回的评估结果。从攻击者的角度来看，此设置为利用不安全的输入处理并发起代码注入攻击提供了机会。如果代码注入攻击成功，攻击者可能会获得对 Lambda 执行环境中存储的环境变量和密钥的未经授权的访问，并可能通过滥用函数的 IAM 角色权限来提升权限并访问 AWS 账户中的其他资源。

当你阅读本章的每个部分时，你将看到此实验室设置如何演变以阻止出站连接并通过具有限制性网络控制的 VPC 防止数据外泄。这将帮助你以逐步的方式模拟攻击和防御，每一节都建立在前一节的基础上。

# 测试常见的 S3 存储桶配置错误

虽然你可能很想立即测试代码注入，但最好先查看相关资源和组件是否存在配置问题。由于此无服务器应用程序使用 Amazon S3 来托管前端，因此自然的起点是检查 S3 存储桶是否存在常见的配置错误以及是否存在任何可能暴露的源代码或凭据。

有鉴于此，让我们模拟攻击者发现并从 S3 存储桶下载暴露的文件：

