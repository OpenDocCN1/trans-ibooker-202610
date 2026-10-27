

| 名称 | 主机名 |
|---|---|
| 🔒 tomcat-vm-gulli-vm | gcr.io |
| 🔒 tomcat-vm-gulli-vm-non-runnable-base | gcr.io |

所有生成的产物都可以通过 CLI 下载：

```
migctl migration get-artifacts my-migration
```

其中包括：

- **deployment_spec.yaml**：配置你的工作负载；
- **Dockerfile**：用于构建迁移后 VM 的镜像；
- **migration.yaml**：迁移计划的副本。

## Dockerfile 概述

在本节中，我们将深入研究由 M4A 生成的 Dockerfile（见清单 18.1）。该文件可以编辑以自定义你的镜像，例如安装新软件包或安装升级版的 M4A 运行时。文件包含运行时原始容器仓库、包含源 VM 捕获数据的镜像，以及初始入口点。典型的 M4A Dockerfile 如下：

### 清单 18.1 由 M4A 生成的 Dockerfile

```
# Please refer to the documentation:
# https://cloud.google.com/migrate/anthos/docs/dockerfile-referen

FROM anthos-migrate.gcr.io/v2k-run-embedded:v1.5.0 as migrate-for

# Image containing data captured from the source VM
FROM gcr.io/named-tome-295414/tomcat-vm-gulli-vm-non-runnable-base
```

```
```dockerfile
# If you want to update parts of the image, add your commands here
# For example:
# RUN apt-get update
# RUN apt-get install -y \
# package1=version \
# package2=version \
# package3=version
# RUN yum update
# RUN wget http://github.com

COPY --from=migrate-for-anthos-runtime / /

# Migrate for Anthos image includes entrypoint
ENTRYPOINT [ "/.v2k.go" ]
```
```

我们将在本章后面讨论 M4A 生成的 Dockerfile 的更多细节，当我们讨论迁移后与 CI/CD 管道的集成时。在下一节中，我们将讨论 deployment_spec.yaml 文件的细节。

## deployment_spec.yaml 概述

在本节中，我们将讨论由 M4A 生成的 deployment_spec.yaml。首先，让我们定义一些后面会用到的术语：

- **Stateless：** 当服务器不存储有关客户端会话的任何状态时，应用程序是无状态的。换句话说，没有过去交易的存储知识或引用
- **Stateful：** 当服务器存储有关客户端会话的数据时，应用程序是有状态的。换句话说，当前交易可能受到之前交易发生的影响。出于这个原因，有状态应用程序需要每次处理用户请求时都使用相同的服务器。

有了这个背景，让我们考虑 deployment_spec.yaml。根据 UI 中选择的意图标志，此文件会有所不同：

1. **Intent: Image。** YAML 定义了一个无状态应用程序，其中相同的 Pod 作为服务进行管理。YAML 的不同部分为：
   
   a) **Deployment。** 这是从

### 清单 18.2 由 M4A 生成的 Deployment_spec.yaml

```
# Stateless application specification
# The Deployment creates a single replicated Pod, indicated by th
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: tomcat-vm-gulli-vm
    migrate-for-anthos-optimization: "true"
    migrate-for-anthos-version: v1.5.0
  name: tomcat-vm-gulli-vm
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat-vm-gulli-vm
      migrate-for-anthos-optimization: "true"
      migrate-for-anthos-version: v1.5.0
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: tomcat-vm-gulli-vm
        migrate-for-anthos-optimization: "true"
        migrate-for-anthos-version: v1.5.0
    spec:
      containers:
      - image: gcr.io/named-tome-295414/tomcat-vm-gulli-vm:11-13-
        name: tomcat-vm-gulli-vm
        readinessProbe:
          exec:
            command:
            - /code/ready.sh
        resources: {}
        securityContext:
          privileged: true
        volumeMounts:
        - mountPath: /sys/fs/cgroup
          name: cgroups
      volumes:
      - hostPath:
```

| NAME | READY | STATUS | RESTARTS |
|---|---|---|---|
| tomcat-vm-gulli-vm-66b44696f-ttgq6 | 1/1 | Running | 0 |

默认情况下，容器以无负载均衡部署，并使用单个集群内部 IP，只能从集群内部访问（见图 18.31）。这意味着 Kubernetes endpoints 控制器将修改 DNS 配置以返回指向 Pod 的地址，这些 Pod 被标记为 "app": "tomcat-vm-gulli-vm"。当然，你可以更改部署并添加 LoadBalancer。我们通过在部署规范中添加以下内容来实现：

```
apiVersion: v1
kind: Service
metadata:
    name: hello-service
spec:
    selector:
        app: tomcat-vm-gulli-vm
    ports:
      - protocol: TCP
        port: 80
        targetPort: 80
    type: LoadBalancer
```

然后，让我们检查服务是否确实可访问：

```
kubectl get service hello-service
NAME             TYPE          CLUSTER-IP     EXTERNAL-IP      POR
hello-service    LoadBalancer  10.88.10.207   34.67.239.170    80:
```

让我们尝试从互联网访问（见图 18.31）：

## 图 18.31：使用 M4A 迁移后访问 Tomcat 容器

![](img/fb3bebce7b5f593bca1a7f8a1371be06_706_0.png)

## 它工作了！

如果你通过网络浏览器看到此页面，这意味着你已成功设置 Tomcat。恭喜！

恭喜！你现在拥有一个可路由的容器，其中包含之前在 VM 中可用的完整 Tomcat 安装！迁移是自动完成的，无需重新编译或访问原始源代码。

在结束之前，关于 GUI 的提示。如果你需要编辑多个文件，使用内置的基于 Eclipse 的编辑器可能会非常方便。特别是，编辑器是快速轻松地查看和更改由 M4A 生成的所有文件的好方法（见图 18.32）。

**图 18.32：用于操作迁移配置的内置编辑器。**

```
migctl source create ce my-ce-src --project my-project --json-key
```

其中 my-ce-src 是源的名称，my-project 是项目名称，m4a-ce-src.json 是在为使用 Compute Engine 作为迁移源创建服务帐户后获得的 JSON 密钥文件名称。

然后你可以使用以下命令创建迁移：

```
migctl migration create my-migration --source my-ce-src --vm-id m
```

其中 my-migration 是迁移的名称，vm-id 是 Google Cloud Console 中显示的 Compute Engine 实例的名称。截至 2020 年底，Windows 支持的唯一意图是“Image”。刚刚创建的迁移计划可以用以下命令检索：

```
migctl migration get my-migration
```

如果需要，可以通过编辑文件 my-migration.yaml 来自定义迁移计划。编辑完成后，可以使用以下命令上传编辑后的迁移计划：

```
migctl migration update my-migration
```

下一步是执行迁移并使用以下命令生成产物：

```
migctl migration generate-artifacts my-migration
```

在迁移期间，可以使用以下命令监控状态：

```
migctl migration list
```

迁移完成后，可以使用以下命令访问产物：

```
migctl migration get-artifacts my-migration
```

作为结果，你应该会看到类似以下内容：

```
Artifacts are accessible through `gsutil cp gs://PATH/artifacts.z
```

因此你可以使用以下命令获取产物：

```
```bash
gsutil cp gs://PATH/artifacts.zip ./
```
```

下一步是使用产物构建 Docker 镜像。我们可以使用 Windows powershell 使用以下命令展开 `artifacts.zip`：

```
```powershell
Expand-Archive .\artifacts.zip
```
```

然后使用以下命令登录容器注册表：

```
```bash
docker login gcr.io
```
```

下一步是构建容器：

```
```bash
docker build -t gcr.io/myproject/myimagename:v1.0.0 .\artifacts\
docker push gcr.io/myproject/myimagename:v1.0.0
```
```

构建完成后，容器镜像将位于容器注册表中，并且可以部署到 GKE 集群。请注意，Anthos 的 Google Cloud Console 自 M4A v1.5 起包含 Windows 工作负载支持。体验与之前讨论的 Linux 相同。如果你想看另一个 Windows 迁移的示例，我建议你考虑动手实验室“ Migrate for Anthos: Windows ”，可在 quicklabs[31] 上获得。

## 18.4.5 从其他云迁移

截至 2021 年 11 月，从其他云迁移基于两步方法。首先，虚拟机被迁移（技术上这些被转换为 GCE 实例）。然后，迁移的虚拟机被容器化。

M4A 使用产品 Migrate for Compute Engine (M4CE)[32] 来流式传输位于其他云或本地的虚拟机，将它们移动到 GCE 实例上。Migrate for Compute Engine 可以通过 marketplace[33] 安装，它允许跨多个数据中心和云从 VMWare、Microsoft Azure 和 Amazon EC2 等源平台迁移数千个应用程序。

