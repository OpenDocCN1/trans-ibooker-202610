
```
curl \
  -u token:$DATABRICKS_TOKEN \
  -X POST \
  -H "Content-Type: application/json; format=pandas-records" \
  -d@data.json \
  https://adb-2951765055089996.16.azuredatabricks.net/model/Fake-News/1/invocations
```

另一种调用模型的方式是通过 Python，如 [以下示例](https://github.com/noahgift/mlflow-project-best-practices/blob/main/predict-fake-news.py) 所示。一般流程是创建包含输入数据的 `DataFrame`，在这种情况下是对可疑新闻报道的文本进行分类 [示例 5-4](#ch05.xhtml#predict_fake_news_via_Python)。

### 示例 5-4. 列出模型 (`predict-fake-news.py`)
```
#!/usr/bin/env python

import os
import requests
import pandas as pd

my_data = {
    "text": {"aliens are coming to invade earth"},
}
df = pd.DataFrame(data=my_data)

def create_tf_serving_json(data):
    return {
        "inputs": {name: data[name].tolist() for name in data.keys()}
        if isinstance(data, dict)
        else data.tolist()
    }

def score_model(dataset):
    url = "https://adb-2951765055089996.16.azuredatabricks.net/model/Fake-News/1/invocations"
    headers = {"Authorization": f'Bearer {os.environ.get("DATABRICKS_TOKEN")}'}
    data_json = (
        dataset.to_dict(orient="split")
        if isinstance(dataset, pd.DataFrame)
        else create_tf_serving_json(dataset)
    )
    response = requests.request(method="POST", headers=headers, url=url, json=data_json)
    if response.status_code != 200:
        raise Exception(
            f"Request failed with status {response.status_code}, {response.text}"
        )
    return response.json()


if __name__ == "__main__":
    print(score_model(df))
```

你可以在 Azure Cloud shell 中看到此 API 调用，见 [图 5-9](#ch05.xhtml#Figure-5.15-azure-cloud-shell)。

![](assets/5.15-azure-cloud-shell.png)

你可以在 GitHub Codespace 中看到此 API 调用，见 [图 5-10](#ch05.xhtml#Figure-5.16-github-codespaces)。

![](assets/5.16-github-codespaces.png)

#### 注
这里是对整个过程的逐步讲解。
* [YouTube 讲解](https://studio.youtube.com/video/PUXhWZQW8BI/edit?c=UCNDfiL0D1LUeKWAkRE1xO5Q)
* [从开发到生产的 MLOps 平台](https://learning.oreilly.com/videos/mlops-platforms-from/032232022VIDEOPAIML)

你还可以将之前下载的模型放入 `FastAPI` 微服务中，如 [以下示例](https://github.com/noahgift/mlflow-project-best-practices/blob/main/main.py) 所示，见 [示例 5-5](#ch05.xhtml#predict_fake_news_via_FastAPI)。

### 示例 5-5. 通过 `FastAPI` 预测 (`main.py`)
```
# pylint: disable=no-name-in-module
# pylint: disable=no-self-argument

from fastapi import FastAPI
import uvicorn
import mlflow
import pandas as pd
from pydantic import BaseModel
from fastapi.responses import JSONResponse
from fastapi.encoders import jsonable_encoder

class Story(BaseModel):
    text: str

def predict(text):
    print(f"Accepted payload: {text}")
    my_data = {
        "text": {0: text},
    }
    data = pd.DataFrame(data=my_data)
    result = loaded_model.predict(pd.DataFrame(data))
    return result


# Load model as a PyFuncModel.
loaded_model = mlflow.pyfunc.load_model('model')
app = FastAPI()

@app.post("/predict")
async def predict_story(story: Story):
    print(f"predict_story accepted json payload: {story}")
    result = predict(story.text)
    print(f"The result is the following payload: {result}")
    payload = {"FakeNewsTrueFalse": result.tolist()[0]}
    json_compatible_item_data = jsonable_encoder(payload)
    return JSONResponse(content=json_compatible_item_data)

@app.get("/")
async def root():
    return {"message": "Hello Model"}

@app.get("/add/{num1}/{num2}")
async def add(num1: int, num2: int):
    """Add two numbers together"""

    total = num1 + num2
    return {"total": total}


if __name__ == '__main__':
    uvicorn.run(app, port=8080, host='0.0.0.0')
```

要将工作流程提升到下一个层次，请将其容器化，如以下 [示例 5-6](#ch05.xhtml#containerize_inference_application) [Dockerfile](https://github.com/noahgift/mlflow-project-best-practices/blob/main/Dockerfile) 所示。

### 示例 5-6. `FastAPI` 应用的 `Dockerfile`
```
FROM public.ecr.aws/lambda/python:3.8

RUN mkdir -p /app
COPY ./main.py /app/
COPY model/ /app/model/
COPY ./requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir --upgrade -r /app/requirements.txt
WORKDIR /app
EXPOSE 8080
CMD [ "main.py" ]
ENTRYPOINT [ "python" ]
```

将 ml 应用容器化的原因是它允许你一次构建，多次部署。例如，你可以部署到 AWS App Runner 或 Azure App Services，或 Google Cloud Run。

# 自动记录和 MLOps 自动化

`MLflow` 的一个有价值的方面是它可以自动记录正在发生的事情 [示例 5-7](#ch05.xhtml#mlflow_inference_application) [在项目中](https://mlflow.org/docs/latest/tracking.xhtml#automatic-logging)。它通过使用 `mlflow.autolog()` 实现这一点。

### 示例 5-7. mlflow 推理日志记录
```
import mlflow

from sklearn.model_selection import train_test_split
from sklearn.datasets import load_diabetes
from sklearn.ensemble import RandomForestRegressor

mlflow.autolog()

db = load_diabetes()
X_train, X_test, y_train, y_test = train_test_split(db.data, db.target)

# Create and train models.
rf = RandomForestRegressor(n_estimators = 100, max_depth = 6, max_features = 3)
rf.fit(X_train, y_train)

# Use the model to make predictions on the test dataset.
predictions = rf.predict(X_test)
autolog_run = mlflow.last_active_run()
```

接下来，让我们转向另一个主题：处理大规模训练。

# 处理大规模训练

大规模训练是一个引人入胜的问题，值得先从理论规模开始。在 [图 5-11](#ch05.xhtml#Figure-5.3-unique-characteristics) 中，注意一次构建 ML 模型并多次部署的想法。

![](assets/5.3-unique-characteristics.png)

使这一过程成为可能的是首先为 `ONNX runtime` 构建 [ONNX 运行时](https://github.com/microsoft/onnxruntime)。接下来，开发人员决定部署到哪些潜在目标。对于使用专用硬件和商业 MLOps 推理服务的 Web 应用而言，部署已有成熟方案。

另一个新兴用例是基于二进制的工作流概念，它嵌入 `ONNX runtime`，然后利用其高性能和低内存占用，解锁在无服务器平台如 AWS Lambda 上进行推理、构建 CLI 工具以及部署到边缘硬件如 [Coral.ai 设备](https://coral.ai/) 的能力。在这种情况下，官方支持的 C# 和 Rust 运行时都是潜在解决方案。

## 构建和运行多阶段工作流

为什么要使用 MLOps 平台的一个绝佳例子是把生命周期的多个阶段联系起来。这些包括数据收集和准备、特征工程、模型训练、模型选择和调优，以及部署和监控。临时编排所有这些在现实世界中是不可扩展的。让我们接下来深入了解细节。

## 扩展和自动化深度学习任务

在现代 MLOps 工作流中，笔记本具有多种用途，这增加了它在未来仍保持在 MLOps 中心的挑战，如 [图 5-12](#ch05.xhtml#Figure-5.15-mlops-notebooks) 所示。

![](assets/5.15-mlops-notebooks.jpg)

注意挑战在于将科学重点与工程重点合并。让我们接下来更详细地探讨这一点。

## 高效管理计算资源

在规模化 MLOps 中，挑战在于编排从摄取数据、进行探索性数据分析、建模，然后得出结论的连续过程，如 [图 5-13](#ch05.xhtml#Figure-5.17-sagemaker) 所示

![](assets/5.17-sagemaker.jpg)

