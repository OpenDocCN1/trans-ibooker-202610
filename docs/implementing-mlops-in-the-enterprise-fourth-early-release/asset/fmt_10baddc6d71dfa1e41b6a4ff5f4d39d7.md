
## PyTorch FastAPI Prediction Service

```
"""
Build a PyTorch model that used for prediction served out via FastAPI
"""

import io
import json

from torchvision import models
import torchvision.transforms as transforms
from PIL import Image
import fastapi
from fastapi import File, UploadFile
import uvicorn

app = fastapi.FastAPI()

model = models.densenet121(weights="DenseNet121_Weights.IMAGENET1K_V1")
model.eval()
imagenet_class_index = json.load(open("imagenet_class_index.json", encoding="utf-8"))


def transform_image(image_bytes):
    my_transforms = transforms.Compose(
        [
            transforms.Resize(255),
            transforms.CenterCrop(224),
            transforms.ToTensor(),
            transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225]),
        ]
    )
    image = Image.open(io.BytesIO(image_bytes))
    return my_transforms(image).unsqueeze(0)


def get_prediction(image_bytes):
    tensor = transform_image(image_bytes=image_bytes)
    outputs = model.forward(tensor)
    _, y_hat = outputs.max(1)
    predicted_idx = str(y_hat.item())
    return imagenet_class_index[predicted_idx]


@app.get("/")
def index():
    return {"message": "Hello World"}


@app.post("/files/")
async def create_file(file: bytes = File()):
    return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile):
    return {"filename": file.filename}


@app.post("/predict")
async def predict(file: UploadFile = File(...)):
    image_bytes = await file.read()
    print(len(image_bytes))
    class_id, class_name = get_prediction(image_bytes=image_bytes)
    return {"class_id": class_id, "class_name": class_name}


if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8080)
```

The `Dockerfile` is very simple as shown. It serves out traffic to port 8080 on the external interface of “0.0.0.0”.

```
FROM tiangolo/uvicorn-gunicorn-fastapi:python3.9

WORKDIR /code

COPY . /code

RUN pip install --upgrade pip &&\
    pip install --no-cache-dir --upgrade -r /code/requirements.txt

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8080"]
```

The container image gets pushed to Amazon ECR, or you can point to the source code repo, as shown in [Figure 5-20](#ch05.xhtml#Figure-5-19-container-registery) and then you setup [AWS App Runner](https://aws.amazon.com/apprunner/).

Next, you configure the build process in [Figure 5-21](#ch05.xhtml#Figure-5-20-configure-build).

Next, you configure the service in [Figure 5-22](#ch05.xhtml#Figure-5-21-configure-service).

Next, you configure the deployment in [Figure 5-23](#ch05.xhtml#Figure-5-22-configure-deploy).

At the end of this process, you get to the deployed endpoint and connect to `/docs`; from there, it is straightforward to upload images to predict. You can view [entire walkthrough on YouTube here](https://www.youtube.com/watch?v=-CoOotWG30E).

## Guest Section on GitHub Actions with Azure Machine Learning Studio

In this section, I contacted [O’Reilly Alfredo Deza](https://www.oreilly.com/pub/au/7872), a Developer Advocate for Azure. He worked extensively with DevOps and MLOps workflows in education and shared this technique with the readers of this book.

One of the things I tend to use when working with Azure Machine Learning Studio is to add GitHub Actions, making it a powerful combination. GitHub Actions work with YAML files called workflow files. These files are easy to read and, therefore, easy to maintain - an essential mix when working with enterprise-level software.

There are two common patterns I use, and a base to build on further:

1.- Registering models or datasets from GitHub to Azure ML Studio

2.- Retrieving specific versions of models for packaging

### Authenticating to Azure

Any time you are interacting with Azure to create or retrieve data from you account will require authentication. There are different ways that you can authenticate with GitHub Actions but one that I tend to use the most is using an Azure Service Principal. A Service Principal is a way to create accounts that have limited scope to resources and it only needs one simple command.

Using the [Azure Cloud Shell](https://shell.azure.com/?WT.mc_id=academic-0000-alfredodeza) run the following command to find your subscription id:

```
az account show --query id -o tsv
```

Capture the resulting ID and use it in the next step, replace `$AZURE_SUBSCRIPTION_ID` with the result from the previous command:

```
az ad sp create-for-rbac  --sdk-auth --name "github-actions" \
   --role contributor --scopes /subscriptions/$AZURE_SUBSCRIPTION_ID
```

That command will generate a JSON output that you must use it in a GitHub repository secret. A repository secret is a way to securely store sensitive information like the one provided by the Azure CLI command. In the GitHub repository where you want to use GitHub Actions with Azure, go to Settings, then Actions, and select New. For the name, use `AZURE_CREDENTIALS` and for the value, paste the JSON output from the last command.

Although you will need to configure the YAML workflow with more components, this is the step you would use to authenticate to Azure:

```
  - uses: azure/login@v1
    with:
     creds: ${{ secrets.AZURE_CREDENTIALS }}
```

The fact that it only requires three lines to authenticate properly with Azure with an account that has enough permissions allows you to concentrate in the other aspects of your Machine Learning project, rather than spending time in trying to make authentication work. This is one of the main reasons I like GitHub Actions.

### Use your Azure ML Workspace

This section will assume that you already have created an Azure Machine Learning workspace and you have access to its portal at [*https://ml.azure.com/?WT.mc\_id=academic-0000-alfredodeza*](https://ml.azure.com/?WT.mc_id=academic-0000-alfredodeza). One common use for the Azure ML workspace is to create and store models in Azure. You can use these registered models with GitHub Actions after authenticating to perform different workflows including packaging.

The following example shows you how to make sure that the Azure CLI will have everything it needs to work with the Azure ML workspace and retrieve the model:

```
- name: set auto-install of extensions
  run: az config set extension.use_dynamic_install=yes_without_prompt
```

Next, replace `workspace-name` and `workspace-group` with your Azure ML workspace and resource group respectively so that GitHub Actions can attach the workspace to the job:

```
- name: attach workspace
  run: az ml folder attach -w "workspace-name" -g "workspace-group"
```

Finally, you can retrieve a model with a specific version. The version comes after the colon in the value to `--model-id`. In the following example, we use a GPT-2 model in the ONNX format that was previously registered in Azure ML:

```
- name: retrieve the ONNX model
  run: az ml model download -t "." --model-id "GPT-2-onnx:1"
```

Although I haven’t gone into more details to build an end-to-end example, hopefully, I’ve demonstrated some powerful building blocks you can use to create more complex jobs using Azure ML and GitHub Actions. In the past, I’ve used these examples to package Machine Learning models and deploy them to a container registry. The increased readability of GitHub Actions, with the ease of the Azure CLI, makes this an outstanding combination worth experimenting with.

Let’s move on to wrap up the chapter next.

## Conclusion
