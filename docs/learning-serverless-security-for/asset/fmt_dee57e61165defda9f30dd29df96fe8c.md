
The association between a principal and a role, defining the actions the principal can perform on a resource. Extending the analogy, a role assignment is like granting an employee specific permissions for a room or resource in a building, allowing them to enter the server room, use the copier, or access HR files based on their responsibilities. The founder or president of the company would then have the `Owner` role assigned, giving them full control over all rooms and resources.

### Scope

The set of resources to which a role assignment applies, such as a resource, resource group, or subscription. In the analogy, a scope is like the specific rooms or floors in a building where an employee’s access permissions apply, defining exactly where they can use their ID card.

These key terms are fundamental to managing access and privileges in Azure. Understanding them is essential to prevent misconfigurations that could lead to privilege escalation.

#### Note

This section assumes you are using your Serverless Lab account, which hosts the vulnerable-by-design serverless lab resources you prepared in the preceding chapter.

Let’s now update the function app permissions:

1. Switch back to the Cloud Shell terminal of the Serverless Lab account.

2. Run the following commands (after the `$` sign) to retrieve the principal ID of your function app’s managed identity and store it in an environment variable (`PRINCIPAL_ID`):

```
source ~/vulnerable-function/function-vars.sh

DETAILS=$(az functionapp identity show \
  --name $FUNCTION_APP \
  --resource-group $RESOURCE_GROUP)

PRINCIPAL_ID=$(echo $DETAILS | jq -r ".principalId")
```

You can run `echo $PRINCIPAL_ID` to print the principal ID and ensure that it has been retrieved successfully.

3. List all role assignments for the function app’s managed identity (using its principal ID) to verify its current permissions:

```
az role assignment list \
  --assignee $PRINCIPAL_ID \
  --all \
  -o table
```

This returns the following table of values:

```
Principal  Role                       Scope
---------  -------------------------  -------------------------
...        Key Vault Secrets Officer  /.../vaults/keyvault...
...        Owner                      /subscriptions/...
```

This identity has both the Key Vault Secrets Officer role (scoped to the Key Vault) and the Owner role (scoped to the subscription) assigned to it.

4. List all the role assignments for the function app’s managed identity in JSON format and then display only the principal ID, role, and scope:

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

Running the command gives you the following JSON output:

```
[
  {
    "principal": "...",
    "role": "Key Vault Secrets Officer",
    "scope": "/.../vaults/keyvault..."
  },
  {
    "principal": "...",
    "role": "Owner",
    "scope": "/subscriptions/..."
  }
]
```

5. Store the full list of role assignments for the managed identity in an environment variable (`ASSIGNMENT_LIST_FULL`) and then output the `principalId`, `roleDefinitionName`, and `scope` values for each of these:

```
ASSIGNMENT_LIST_FULL=$(az role assignment list \
  --assignee $PRINCIPAL_ID \
  --all \
  -o json)

ASSIGNMENT_LIST=$(echo $ASSIGNMENT_LIST_FULL | \
  jq '[.[] |
        { principal: .principalId,
          role: .roleDefinitionName,
          scope: .scope }]')

echo $ASSIGNMENT_LIST | jq .
```

This yields the following JSON output:

```
[
  {
    "principal": "...",
    "role": "Key Vault Secrets Officer",
    "scope": "/.../vaults/keyvault..."
  },
  {
    "principal": "...",
    "role": "Owner",
    "scope": "/subscriptions/..."
  }
]
```

6. Run the following block of code to remove each role assignment for the managed identity:

```
echo "$ASSIGNMENT_LIST" | jq -c '.[]' | while read assignment; do
    PRINCIPAL=$(echo "$assignment" | jq -r '.principal')
    ROLE=$(echo "$assignment" | jq -r '.role')
    SCOPE=$(echo "$assignment" | jq -r '.scope')

    echo "Deleting role assignment: $ROLE for principal $PRINCIPAL on $SCOPE"

    az role assignment delete \
        --assignee "$PRINCIPAL" \
        --role "$ROLE" \
        --scope "$SCOPE"
done
```

After a few seconds, you should get the following log output:

```
Deleting role assignment: Key Vault Secrets Officer for principal ... on
/.../.../providers/Microsoft.KeyVault/vaults/keyvault...
Deleting role assignment: Owner for principal ... on /subscriptions/...
```

7. Verify that the role assignments have been removed successfully by listing all role assignments for the managed identity again:

```
az role assignment list \
  --assignee $PRINCIPAL_ID \
  --all \
  -o json
```

Running the command returns `[]`.

8. Retrieve the principal ID and subscription ID associated with the function app’s managed identity and store these in their respective variables (`PRINCIPAL_ID` and `SUBSCRIPTION_ID`):

```
source ~/vulnerable-function/function-vars.sh

DETAILS=$(az functionapp identity show \
  --name $FUNCTION_APP \
  --resource-group $RESOURCE_GROUP)

PRINCIPAL_ID=$(echo $DETAILS | jq -r ".principalId")

SUBSCRIPTION_ID=$(az account show --query id -o tsv)
```

9. Assign the Contributor role to the managed identity at the subscription scope:

```
az role assignment create \
  --assignee $PRINCIPAL_ID \
  --role "Contributor" \
  --scope /subscriptions/$SUBSCRIPTION_ID
```

10. Assign the User Access Administrator role to the managed identity at the subscription scope:

```
az role assignment create \
  --assignee $PRINCIPAL_ID \
  --role "User Access Administrator" \
  --scope /subscriptions/$SUBSCRIPTION_ID
```

11. Verify that the role assignments were successfully applied:

```
ASSIGNMENT_LIST_FULL=$(az role assignment list \
  --assignee $PRINCIPAL_ID \
  --all \
  -o json)

ASSIGNMENT_LIST=$(echo $ASSIGNMENT_LIST_FULL | \
  jq '[.[] |
        { principal: .principalId,
          role: .roleDefinitionName,
          scope: .scope }]')

echo $ASSIGNMENT_LIST | jq .
```

This yields the following JSON output:

```
[
  {
    "principal": "...",
    "role": "Contributor",
    "scope": "/subscriptions/..."
  },
  {
    "principal": "...",
    "role": "User Access Administrator",
    "scope": "/subscriptions/..."
  }
]
```

12. Open `vulnerable-function/src/functions/evaluate.js` in the Cloud Shell Code Editor. Replace the file’s contents with the following vulnerable-by-design expression evaluator code:

```
const { app } = require('@azure/functions');

const HARDCODED_KEY = "67890ghijkl";

app.http('evaluate', {
  methods: ['POST'],
  authLevel: 'function',
  handler: async (request, context) => {
    const expression = await request.text() || '"No expression provided"';

    let result;
    try {
      result = eval(expression);
    } catch (err) {
      result = `Error: ${err.message}`;
    }

    return { body: `Evaluation result: ${result}` };
  }
});
```

#### Caution

After updating the `evaluate.js` file, make sure to save your changes before proceeding to the next steps.
