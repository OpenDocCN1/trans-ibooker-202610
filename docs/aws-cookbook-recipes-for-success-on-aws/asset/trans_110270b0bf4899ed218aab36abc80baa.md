
```
REPOSITORY    TAG         IMAGE ID            CREATED             SIZE
```

# 6.1 构建、标记并推送容器镜像到 Amazon ECR

## 问题

您需要一个存储库来存放已构建和标记的容器镜像。

## 解决方案

首先，您将在 Amazon ECR 中创建一个存储库。接下来，您将创建一个 `Dockerfile` 并使用它来构建 Docker 镜像。最后，您将给容器镜像打上两个标签，并将它们都推送到新创建的 ECR 存储库中。此过程如[图 6-1](#ch06.html#solution_workflow_of_buildcomma_tagcomm)所示。

![](assets/awsc_0601.png)

### 步骤

1. 登录 AWS 管理控制台并搜索 Elastic Container Registry。点击“创建存储库”按钮。

   为您的存储库命名，保留所有默认设置（如[图 6-2](#ch06.html#ecr_repository_creation)所示），向下滚动，再次点击“创建存储库”以完成操作。

   ![](assets/awsc_0602.png)

   您现在在 Amazon ECR 上拥有了一个可用于存储容器镜像的存储库。ECR 控制台的示例见[图 6-3](#ch06.html#screenshot_of_created_ecr_repository)。

   ![](assets/awsc_0603.png)

   或者，您也可以通过命令行创建 ECR 存储库：

   ```
   REPO=aws-cookbook-repo && \
   	aws ecr create-repository --repository-name $REPO
   ```

   您应该会看到类似如下的输出：

   ```
   {
     "repository": {
     "repositoryArn": "arn:aws:ecr:us-east-1:111111111111:repository/aws-cookbook-repo",
     "registryId": "1111111111111",
     "repositoryName": "aws-cookbook-repo",
     "repositoryUri": "611652777867.dkr.ecr.us-east-1.amazonaws.com/aws-cookbook-repo",
     "createdAt": "2021-10-02T19:57:56-04:00",
     "imageTagMutability": "MUTABLE",
     "imageScanningConfiguration": {
       "scanOnPush": false
     },
     "encryptionConfiguration": {
       "encryptionType": "AES256"
     }
     }
   }
   ```

2. 创建一个简单的 `Dockerfile`：

   ```
   echo FROM nginx:latest > Dockerfile
   ```

   #### 注意

   此命令创建一个 [`Dockerfile`](https://oreil.ly/VCCb7)，其中包含一行指令，指示 Docker 引擎使用 `nginx:latest` 镜像作为基础镜像。由于您在 `Dockerfile` 中仅使用基础镜像而没有其他行，因此生成的镜像与 `nginx:latest` 镜像完全相同。您可以通过使用 `COPY` 和 `ADD` `Dockerfile` 指令在此镜像中包含一些 HTML 文件。

3. 构建并标记镜像。此步骤可能需要片刻时间，因为它会下载并合并镜像层：

   ```
   docker build . -t \
   	$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/aws-cookbook-repo:latest
   ```

4. 添加一个额外的标签：

   ```
   docker tag \
   	$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/aws-cookbook-repo:latest \
   	$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/aws-cookbook-repo:1.0
   ```

5. 获取 Docker 登录信息：

   ```
   aws ecr get-login-password | docker login --username AWS \
   	--password-stdin $AWS_ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com
   ```

   以下是输出：

   ```
   Login Succeeded
   ```

   #### 注意

   每次对私有存储库执行操作时，都需要提供授权令牌。令牌的有效期为 12 小时，因此您运行的命令需要在命令行上按该间隔手动刷新。为了帮助您自动获取授权令牌，您可以使用 `ECR Docker Credential Helper`，可从 [awslabs GitHub 存储库](https://oreil.ly/xI0xD) 获取。

6. 将每个镜像标签推送到 Amazon ECR：

   ```
   docker push \
   	$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/aws-cookbook-repo:latest
   	
   docker push \
   	$AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/aws-cookbook-repo:1.0
   ```

   #### 注意

   在第二次推送时，您将看到镜像层上传显示“Layer already exists”（层已存在）。这是因为镜像因第一次推送而已存在于 ECR 存储库中，但此步骤仍然是添加额外标签所必需的。

#### 验证检查

现在您可以从控制台在 Amazon ECR 中查看这两个已标记的镜像，如[图 6-4](#ch06.html#screenshot_of_the_image_with_two_tags)所示。

![](assets/awsc_0604.png)

或者，您可以使用 AWS CLI 列出镜像：

```
aws ecr list-images --repository-name aws-cookbook-repo
```

您应该会看到类似如下的输出：

```
{
  "imageIds": [
    {
      "imageDigest": "sha256:99d0a53e3718cef59443558607d1e100b325d6a2b678cd2a48b05e5e22ffeb49",
      "imageTag": "1.0"
    },
    {
      "imageDigest": "sha256:99d0a53e3718cef59443558607d1e100b325d6a2b678cd2a48b05e5e22ffeb49",
      "imageTag": "latest"
    }
  ]
}
```

### 清理

按照 [章节代码存储库](https://github.com/AWSCookbook/Containers) 中此配方文件夹内的步骤操作。

## 讨论

拥有一个容器镜像存储库是应用程序开发过程的重要基础组件。一个由您控制的私有容器镜像存储库是提高应用程序开发过程安全性的最佳实践。您可以授予其他 AWS 账户、IAM 实体和 AWS 服务访问 Amazon ECR 的权限。现在您已经知道如何创建 ECR 存储库，您将能够存储容器镜像并将它们与 AWS 服务配合使用。

#### 注意

Amazon ECR 支持流行的 [Docker Image Manifest V2, Schema 2](https://oreil.ly/FeYVY) 和最近的 [Open Container Initiative (OCI)](https://oreil.ly/tzgdd) 镜像。它可以在拉取时在这些格式之间进行转换。提供对 Manifest V2, Schema 1 的传统支持，并且 Amazon ECR 在与旧版 Docker 客户端交互时可以即时转换。对于当今使用的大多数 Docker 客户端版本，体验应该是无缝的。

容器标记允许您对容器镜像进行版本控制和跟踪。您可以给一个镜像应用多个标签，这有助于您实施版本控制策略和部署流程。例如，您可能在开发环境中始终引用标记为“latest”的镜像，但生产环境可以锁定到特定的版本标签。`Docker CLI` 将标记的镜像推送到存储库，并且标签可用于拉取操作。

### 挑战

修改 `Dockerfile`，构建一个新镜像，用新版本号标记它，并将其推送到 ECR。

# 6.2 推送到 Amazon ECR 时扫描镜像的安全漏洞

## 问题

您希望每次推送到存储库时，自动扫描容器镜像的安全漏洞。

## 解决方案

在 Amazon ECR 的存储库上启用自动镜像扫描，推送一个镜像，并观察扫描结果，如[图 6-5](#ch06.html#container_image_scanning_solution_workf)所示。

### 先决条件

*   ECR 存储库

![](assets/awsc_0605.png)

### 准备

按照 [章节代码存储库](https://github.com/AWSCookbook/Containers) 中此配方文件夹内的步骤操作。

### 步骤

1. 您将不再从 `Dockerfile` 构建新的容器镜像（就像您在[配方 6.1](#ch06.html#buildingcomma_taggingcomma_and_pushing)中所做的那样），而是要拉取一个旧的 NGINX 容器镜像：

   ```
   docker pull nginx:1.14.1
   ```

2. 在命令行上，将扫描配置应用到您创建的存储库：

   ```
   REPO=aws-cookbook-repo && \
   	aws ecr put-image-scanning-configuration \
   	--repository-name $REPO \
   	--image-scanning-configuration scanOnPush=true
   ```

3. 获取 Docker 登录信息：

   ```
   aws ecr get-login-password | docker login --username AWS \
   	--password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
   ```

4. 给镜像打上标签，以便将其推送到 ECR 存储库：
