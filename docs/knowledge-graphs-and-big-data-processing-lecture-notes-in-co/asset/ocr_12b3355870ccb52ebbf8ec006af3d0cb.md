# Function Calling 介绍

本文主要介绍如何使用 Function Calling 功能，并结合代码示例讲解使用细节。

# ## 什么是 Function Calling

Function Calling 允许模型在对话中识别需要调用外部函数的场景，并输出结构化的调用参数，由应用侧执行真实的函数调用。

+   - 支持多轮对话中的函数调用
+   - 支持自动填充函数参数
+   - 支持并行函数调用

# ## 快速开始

```
```python
from openai import OpenAI

client = OpenAI()

response = client.chat.completions.create(
    model="moonshot-v1-8k",
    messages=[{"role": "user", "content": "上海天气怎么样？"}],
    tools=[{
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get current weather",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {"type": "string"}
                }
            }
        }
    }]
)
```
```

通过 `tools` 参数声明可用函数，模型根据用户输入自动生成调用请求。