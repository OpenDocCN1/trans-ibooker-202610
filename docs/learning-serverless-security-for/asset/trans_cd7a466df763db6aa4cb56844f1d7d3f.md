

1. 将 `[S3 BUCKET NAME]` 替换为你的 S3 存储桶的实际名称，然后在浏览器中打开以下 URL：

```
http://[S3 BUCKET NAME].s3.amazonaws.com/
```

###### Note

如果你的存储桶名称是 `serverless-static-website-abc123`，则 URL 将为 *http​://serverless-static-website-abc123.s3​.ama⁠zonaws.com/*。

这会返回一个类似以下的 XML 响应：

```
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
 <Name>serverless-static-website-...</Name>
 <Prefix/>
 <Marker/>
 <MaxKeys>1000</MaxKeys>
 <IsTruncated>false</IsTruncated>
 <Contents>
  <Key>backup/lambda_function.py</Key>
  <LastModified>...</LastModified>
  ...
 </Contents>
 <Contents>
  <Key>index.html</Key>
  <LastModified>...</LastModified>
  ...
 </Contents>
</ListBucketResult>
```

如 XML 响应所示，S3 存储桶包含键 `backup/lambda_function.py`，如果存储桶权限允许公开读取，则该键可能可以访问。

2. 将 `[S3 BUCKET NAME]` 替换为你的 S3 存储桶的实际名称，然后在浏览器中打开以下 URL 以尝试直接访问该文件：

```
http://[S3 BUCKET NAME].s3.amazonaws.com/backup/lambda_function.py
```

如果文件可公开访问，则会直接在浏览器中显示 `lambda_function.py` 代码：

```
from os import environ
get_env = environ.get

HARDCODED_KEY_01 = "ABCDE"
HARDCODED_KEY_02 = "FGHIJ"

...
```

###### Note

如果源代码包含硬编码密钥，能够访问此文件 的攻击者将能够检索并滥用这些凭据。他们还可能了解应用程序某些组件的实现方式，这可能会泄露有助于他们识别和利用额外漏洞的实现和配置细节。

3. 在本地机器上打开终端，然后运行以下命令以删除任何存储的 AWS 凭据：

```
rm ~/.aws/credentials
```

###### Tip

或者，你可以在删除凭据文件之前先创建其备份——例如，通过运行 `mv ~/.aws/​cre⁠dentials ~/.aws/credentials.bak`。然后你可以使用 `aws configure list` 验证当前是否没有配置任何配置文件，并且 AWS CLI 未使用任何凭据。

4. 将 `[S3 BUCKET NAME]` 替换为你的 S3 存储桶的实际名称，然后运行以下命令将存储桶名称存储在 shell 变量中：

```
BUCKET_NAME="[S3 BUCKET NAME]"
```

5. 列出存储桶的内容以检查是否允许公开列出：

```
aws s3 ls s3://$BUCKET_NAME/
```

这会输出以下消息：

```
Unable to locate credentials. You can configure credentials by running "aws configure".
```

6. 运行以下命令以尝试对存储桶内容进行未经身份验证的列出：

```
aws s3 ls s3://$BUCKET_NAME/ --no-sign-request
```

这会产生以下输出：

```
                               PRE backup/
    2025-00-00 00:00:00       3257 index.html
```

7. 让我们重复前面的步骤，但这次递归列出所有对象以查看嵌套文件夹中的文件：

```
aws s3 ls s3://$BUCKET_NAME/ --recursive --no-sign-request
```

以下是输出：

```
2025-00-00 00:00:00        875 backup/lambda_function.py
2025-00-00 00:00:00       3257 index.html
```

8. 要获取更详细的输出，请使用 `aws s3api list-objects` 命令以 JSON 格式输出存储桶内容：

```
aws s3api list-objects --bucket $BUCKET_NAME --no-sign-request
```

你应该会看到类似以下输出的 JSON 响应：

```
{
    "Contents": [
        {
            "Key": "backup/lambda_function.py",
            "LastModified": "2025-00-00T00:00:00+00:00",
            "ETag": "\"...\"",
            "ChecksumAlgorithm": [
                "CRC64NVME"
            ],
            "ChecksumType": "FULL_OBJECT",
            "Size": 875,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "...",
                "ID": "..."
            }
        },
        {
            "Key": "index.html",
            "LastModified": "2025-00-00T00:00:00+00:00",
            "ETag": "\"...\"",
            "ChecksumAlgorithm": [
                "CRC64NVME"
            ],
            "ChecksumType": "FULL_OBJECT",
            "Size": 3257,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "...",
                "ID": "..."
            }
        }
    ],
    "RequestCharged": null,
    "Prefix": ""
}
```

9. 创建一个用于下载文件的目录，然后检查是否可以在无需身份验证的情况下从存储桶下载特定文件：

```
mkdir downloaded_files

aws s3 cp s3://$BUCKET_NAME/backup/lambda_function.py \
    ./downloaded_files/ --no-sign-request
```

这应该会成功将 `lambda_function.py` 文件下载到 `downloaded_files` 目录中：

```
download: s3://serverless-static-website.../backup/lambda_function.py
to downloaded_files/lambda_function.py
```

10. 创建一个示例文件并尝试在无需身份验证的情况下将其上传到 S3 存储桶：

```
touch sample.txt

aws s3 cp ./sample.txt s3://$BUCKET_NAME/ --no-sign-request
```

由于存储桶不允许公开写入，因此此命令应以 `AccessDenied` 错误失败：

```
upload failed: ./sample.txt to
s3://serverless-static-website.../sample.txt An error occurred
(AccessDenied) when calling the PutObject operation: Access Denied
```

在实践中，团队可能会对多个用途重复使用同一个 S3 存储桶，例如托管静态资源、存储备份，有时甚至存储应用程序代码和配置，这可能会无意中将敏感文件和配置数据与托管网站一起暴露。攻击者不会假设 S3 存储桶仅包含 HTML 或 JavaScript 文件。他们通常会枚举存储桶以检查是否存在其他对象，例如备份、凭据或日志文件，这些文件可能会无意中泄露有助于发起进一步攻击的有用信息。

从防御安全角度来看，你应仔细规划每个 S3 存储桶的用途，并倾向于为不同类型的数据使用单独的存储桶，以避免意外暴露敏感信息。你应确保每个存储桶都应用了正确的访问控制配置，并启用日志记录以持续监控和审计访问。此外，除了这些措施之外，你还应考虑实施自动化警报，以便在 S3 存储桶配置错误或变得公开可访问时通知你或你的团队。

# 使用代码注入攻击在 Lambda 函数中执行任意代码

在本节中，你将探索攻击者如何通过滥用动态代码执行漏洞来利用编写不佳的 Lambda 函数。你将首先通过使用有效输入表达式来实验，以确定应用程序的正常行为。然后你将模拟攻击者使用无效和恶意输入，以观察函数对意外情况的反应，包括检索源代码文件和提取环境变量。在使用代码注入有效载荷从 Lambda 执行环境中检索 AWS 临时凭据后，你将使用它们在攻击者账户中配置新的 AWS CLI 配置文件。
