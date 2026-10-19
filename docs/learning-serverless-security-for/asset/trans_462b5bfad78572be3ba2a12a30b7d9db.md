

17. 由于上一步的 JSON 输出包含许多你暂时不需要的其他属性，请运行以下命令来获取你的函数应用的主机名：

```
az functionapp show \
  --resource-group $RESOURCE_GROUP \
  --name $FUNCTION_APP \
  --query defaultHostName \
  -o tsv
```

这应该返回以 `azurewebsites.net` 结尾的主机名。

18. 你也可以使用以下命令来获取你的函数的 Invoke URL：

```
az functionapp function list \
  --resource-group $RESOURCE_GROUP \
  --name $FUNCTION_APP \
  --query "[].invokeUrlTemplate" \
  -o json
```

这会产生以下输出：

```
[
  "https://functionapp....azurewebsites.net/api/evaluate"
]
```

目前，你有一个使用脚手架模板代码的工作无服务器函数。在接下来的步骤中，你将把它转换为一个有意设计为易受攻击的函数，该函数使用 `eval()` 来计算数学表达式：

1. 点击 Editor 以访问 Cloud Shell 的内置代码编辑器（[Figure 11-3](#ch11.html#accessing-the-built-in-cloud-shell-code-editor)）。

![](assets/lsse_1103.png)

2. 如果提示切换到 Classic Cloud Shell，请选择 Confirm。

3. 导航到 `vulnerable-function` 目录：

```
cd ~/vulnerable-function
```

4. 配置函数应用以将示例 API 密钥（`SAMPLE_API_KEY`）作为环境变量包含在内：

```
source function-vars.sh

az functionapp config appsettings set \
  --name $FUNCTION_APP \
  --resource-group $RESOURCE_GROUP \
  --settings SAMPLE_API_KEY=12345abcdef
```

###### Note

`SAMPLE_API_KEY` 用作真实 API 密钥或密钥的占位符。请勿在你的实验环境中使用真实凭据，因为此示例旨在展示无服务器函数环境变量如何通过攻击者通过错误配置和不安全编码实践访问或利用。

5. 找到并点击“Open editor”按钮（`{}`），在 [Figure 11-4](#ch11.html#opening-the-code-editor-to-modify-your-function-files) 中突出显示，以打开集成代码编辑器并直接在 Azure 门户中修改你的函数文件。

![](assets/lsse_1104.png)

这应该启动集成的 Cloud Shell 代码编辑器——允许你直接在 Azure 门户内查看和编辑你的函数代码。

6. 在代码编辑器中打开 `vulnerable-function/src/functions/evaluate.js`。

7. 用以下内容替换 `evaluate.js` 中的代码，使其返回 `SAMPLE_API_KEY` 环境变量的值：

```
const { app } = require('@azure/functions');

const HARDCODED_KEY = "67890ghijkl";

app.http('evaluate', {
    methods: ['GET', 'POST'],
    authLevel: 'anonymous',
    handler: async (request, context) => {
        const key = process.env.SAMPLE_API_KEY;

        return { body: `SAMPLE_API_KEY=${key}` };
    }
});
```

在这里，你有一个示例函数，其中包含硬编码密钥（`HARDCODED_KEY`）以及环境变量（`SAMPLE_API_KEY`），它允许函数访问各种服务和资源。例如，函数可以使用硬编码密钥（`HARDCODED_KEY`）和环境变量（`SAMPLE_API_KEY`）来连接到外部 API 端点。

8. 在 Cloud Shell 终端（`$` 符号之后）中，运行以下命令将更新后的函数代码（`evaluate.js`）部署到你的函数应用：

```
func azure functionapp publish $FUNCTION_APP
```

###### Note

如果在运行 `func azure functionapp publish`（或其他 CLI 命令）时遇到 `Response status code does not indicate success: 401 (Unauthorized)`，请尝试重新进行身份验证（或者，你可以刷新页面然后再尝试运行命令）。

9. 在新的浏览器标签页中打开输出中显示的 Invoke URL，以确认函数能够成功加载环境变量（`SAMPLE_API_KEY`）。你应该得到以下内容：

```
SAMPLE_API_KEY=12345abcdef
```

10. 在代码编辑器中打开 `vulnerable-function/src/functions/evaluate.js`。用以下有意设计为易受攻击的表达式求值器代码替换 `evaluate.js` 的内容：

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

在这里，除了拥有硬编码密钥外，你还有一个使用 `eval()` 执行不受信任输入的函数，这使得攻击者可以轻松地窃取授予跨云平台和第三方服务访问权限的敏感凭据。

###### Note

由于代码中使用的凭据仅为示例占位符，其余易受攻击的函数已被省略，以保持示例的聚焦和清晰。

11. 在终端中运行以下命令以部署最新更改：

```
func azure functionapp publish $FUNCTION_APP
```

12. 在新的浏览器标签页中打开 Invoke URL。你现在应该得到以下消息，因为 `authLevel` 设置为 `function`：

```
This functionapp....azurewebsites.net page can't be found

No webpage was found for the web address...
HTTP ERROR 404
```

###### Note

当 `authLevel` 设置为 `function` 时，你必须在请求中包含有效的函数密钥才能成功调用端点。由于此函数是有意设计为易受攻击的，此设置可确保只有你才能调用和测试它（来自 Attacker 账户），而不是将其暴露给攻击者利用。

13. 切换回 Cloud Shell 终端，然后运行以下命令（在 `$` 符号之后）将函数的 Invoke URL 存储在环境变量（`INVOKE_URL`）中：

```
INVOKE_URL="[SPECIFY INVOKE URL]"
```

在运行命令之前，请务必将 `[SPECIFY INVOKE URL]` 替换为函数的 Invoke URL。

14. 准备就绪后，让我们测试已部署的函数。检索函数应用中 `evaluate` 函数的函数密钥：

```
KEY=$(az functionapp function keys list \
  --resource-group $RESOURCE_GROUP \
  --name $FUNCTION_APP \
  --function-name evaluate)

echo $KEY
```

以下是输出：

```
{
  "default": "...",
  "id": null,
  "kind": null,
  "name": null,
  "properties": null,
  "type": null
}
```

从检索到的密钥中提取默认函数密钥：

```
FUNCTION_KEY=$(echo $KEY | jq -r ".default")
echo $FUNCTION_KEY
```

###### Caution

请勿公开或共享你的函数密钥，因为任何拥有此密钥的人都可以调用你的有意设计为易受攻击的函数。

通过发送带有简单表达式（`2+2`）的 `POST` 请求来测试 `evaluate` 函数：

```
curl -X POST "$INVOKE_URL?code=$FUNCTION_KEY" \
  -H "Content-Type: text/plain" \
  -d "2+2"
```

这会输出以下内容：

```
Evaluation result: 4
```

再次使用另一个有效表达式（`(2 + 2) * 7`）测试 `evaluate` 函数：

```
curl -X POST "$INVOKE_URL?code=$FUNCTION_KEY" \
  -H "Content-Type: text/plain" \
  -d "(2 + 2) * 7"
```

这应该成功返回以下求值结果：

```
Evaluation result: 28
```

