

在本节结束时，您将更好地了解攻击者如何利用代码注入漏洞和角色配置错误来从 Secrets Manager 中窃取机密。

## 在 AWS Secrets Manager 中存储机密

如果您之前使用过无服务器应用程序，您可能听说过使用 AWS Secrets Manager 来处理敏感信息（如 API 密钥、数据库凭据或访问令牌）的建议。Secrets Manager 通过提供安全且集中的方式来存储、检索和自动轮换机密，从而消除了将机密硬编码到 Lambda 函数中的风险。这在无服务器环境中尤其有价值，因为代码经常更新和部署，使硬编码机密成为一种危险的做法。

### Note
本节假定您使用的是 Serverless Lab 账户，该账户将托管易受攻击的无服务器实验室环境。

让我们逐步了解如何在 AWS Secrets Manager 中存储机密：

1. 打开一个新的浏览器标签页并导航到 [AWS 管理控制台](https://oreil.ly/qe5es)，使用您的 Serverless Lab AWS 账户。
2. 在搜索栏中输入 `secrets manager`；然后从匹配的服务列表中选择 Secrets Manager，以导航到 Secrets Manager 控制台。
3. 在 Secrets Manager 控制台上，选择左侧导航窗格中的 Secrets，然后单击“Store a new secret。”
4. 在“Choose secret type”页面下，在“Secret type”下，选择“Other type of secret。” 在“Key/value pairs”下，指定以下配对：
   * `secret1`: `secret-value1`
   * `secret2`: `secret-value2`
   * `secret3`: `secret-value3`
   然后单击 Next。
5. 在“Configure secret”页面下，在“Secret name and description”下，在“Secret name”字段中输入 `prod/lambda-secret`。然后单击 Next。
6. 在“Configure rotation”页面，单击 Next。
7. 在 Review 页面，向下滚动到底部并单击 Store。

### Note
出现以下通知，表明机密已成功创建：`You successfully stored the secret prod/lambda-secret. To show it in the list, click Refresh`。

有了这个，您可以继续更新附加到 Lambda 函数的 IAM 角色，以确保它具有正确的安全访问 AWS Secrets Manager 的权限。

## 更新 Lambda 函数角色权限以访问 Secrets Manager

目前，附加到 Lambda 函数的 IAM 角色没有访问 AWS Secrets Manager 所需的权限。在本节中，我将指导您为 Lambda 角色创建自定义内联策略，允许函数安全地访问和检索机密。

### Note
本节假定您使用的是 Serverless Lab 账户，该账户托管易受攻击的无服务器实验室环境。

让我们继续更新 Lambda 执行角色：

1. 切换回已打开 AWS Lambda 控制台的浏览器标签页。确保在继续下一步之前，已打开 `lambda-0010` 的 AWS Lambda 函数详细信息页面。
2. 导航到 Configuration 选项卡。然后单击 Permissions。在 Execution Role 部分，单击“Role name”下的链接，在新标签页中打开 IAM 角色详细信息，在其中查看或修改其权限。
3. 在“Permissions policies”部分（在 Permissions 选项卡上），单击“Add permissions。” 然后从可用选项列表中选择“Create inline policy”。

### Note
这应该指导您完成创建内联策略的过程，该过程包括两个步骤：指定权限以及查看和创建策略。

4. 在“Specify permissions”页面下，在“Select a service”下，从下拉菜单中选择 Secrets Manager。
   在“Actions allowed”下，单击 List 以显示可用选项。切换打开“All list actions”复选框。这应自动选择与 Secrets Manager 相关的所有仅列出权限。
   然后单击 Read 以显示可用选项。切换打开“All read actions”复选框。这应自动选择与 Secrets Manager 相关的所有只读权限。
   在 Resources 下，选择 All。然后单击 Next。
5. 在“Review and create”页面，在“Policy name”字段中指定策略名称（例如，`read-secrets-manager-policy`）。然后单击“Create policy。”

### Note
出现以下通知，表明策略已成功创建：`Policy read-secrets-manager-policy created.` 

请随意查看策略 JSON 以确认已应用正确的权限。您可以使用生成式 AI 工具进行审查和验证，以便了解策略授予的权限并确认包含正确的操作和资源。由于生成式 AI 工具可能会产生不准确的信息，请记住始终将详细信息与官方 AWS 文档进行交叉检查。

一旦您验证内联策略授予了必要的 Secrets Manager 权限，您就可以继续更新 Lambda 函数代码以完成设置。

## 更新 Lambda 函数代码以从 Secrets Manager 检索机密

在本节中，您将更新前几节中的 Lambda 函数代码以从 Secrets Manager 检索机密。更具体地说，您将定义一个 `get_secret()` 函数，该函数使用提供的机密名称和区域通过 `GetSecretValue` API 检索机密。您会看到，通过 `boto3` Python 包从 Secrets Manager 检索机密只需要几行额外的代码。

### Note
本节假定您使用的是 Serverless Lab 账户，该账户托管易受攻击的无服务器实验室环境。

让我们继续更新 Lambda 函数：

1. 切换回已打开 AWS Lambda 控制台的浏览器标签页。确保在继续下一步之前，已打开 `lambda-0010` 的 AWS Lambda 函数详细信息页面。
2. 导航到 Code 选项卡。在“Code source”部分，用以下内容替换现有的 `lambda_function.py` 代码：

```
import re
import boto3
import json

from os import environ
get_env = environ.get

HARDCODED_KEY_01 = "ABCDE"
HARDCODED_KEY_02 = "FGHIJ"


def get_secret(secret_name, region_name="us-east-1"):
    client = boto3.client("secretsmanager", region_name=region_name)
    response = client.get_secret_value(SecretId=secret_name)
    secret_dict = json.loads(response['SecretString'])

    return secret_dict

def get_statement(event):
    params = event.get('queryStringParameters', {})
    statement = params.get('statement', None)

    return statement


def process_statement(statement):
    output = "No statement parameter value provided"

    if statement:
        output = eval(statement)

    return output


def sanitize_output(output):
    return re.sub(r'[^0-9\.]', '', str(output))


def lambda_handler(event, context):
    print("event: ", event)
    print("HARDCODED_KEY_01: ", HARDCODED_KEY_01)
    print("HARDCODED_KEY_02: ", HARDCODED_KEY_02)
    print("CUSTOM_ENV_VAR_01: ", get_env('CUSTOM_ENV_VAR_01'))
    print("CUSTOM_ENV_VAR_02: ", get_env('CUSTOM_ENV_VAR_02'))

    sm_data = get_secret("prod/lambda-secret")
    print("SECRETS MANAGER SECRETS: ", str(sm_data))

    statement = get_statement(event)
    result = process_statement(statement)

    return {
        'statusCode': 200,
        'body': sanitize_output(result)
    }
```
