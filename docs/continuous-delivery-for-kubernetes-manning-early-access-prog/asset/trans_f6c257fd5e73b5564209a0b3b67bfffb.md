

```
helm repo add cdf https://cdfoundation.github.io/tekton-helm-chart/
helm install tekton cdf/tekton-pipeline
```

一旦你安装了 Tekton Helm chart，你会看到一个新的命名空间被创建了。这个名为 `tekton-pipelines` 的新命名空间包含了 pipeline 控制器（即 pipeline 引擎）和 pipeline webhook 监听器，后者用于监听来自外部源（如 git 仓库）的事件。

你也可以安装 `tkn` 命令行工具，如果你正在处理多个任务和复杂的 pipeline，这会很有帮助。你可以按照这里的安装说明进行操作：https://github.com/tektoncd/cli

如果现在列出集群中安装的所有 CustomResourceDefinitions 并属于 tekton，你会看到有一组新的资源可以使用：

```
> kubectl get crds | grep tekton #1
clustertasks.tekton.dev
conditions.tekton.dev
pipelineresources.tekton.dev
pipelineruns.tekton.dev    #2
pipelines.tekton.dev      #3
runs.tekton.dev
taskruns.tekton.dev       #4
tasks.tekton.dev          #5
```

#1 我们可以使用 kubectl 获取所有 Custom Resource Definitions (crd) 并只过滤与 tekton 相关的那些

#2 #3 #4 #5 是我们将为 pipeline 创建的资源类型，注意你也可以通过使用 `kubectl get` 和资源类型来获取这些资源

一旦你安装了 Tekton，你就可以开始创建简单的 YAML 任务定义。Tekton 中的 Task 看起来像一个普通的 Kubernetes 资源：

```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: echo-hello-world
spec:
  steps:
  - name: echo
    image: ubuntu #1
    command:
    - echo #2
    args:
    - "Hello World" #3
```

#1 名为 ubuntu 的 docker 镜像将用于此任务

#2 此任务使用的主要命令是 echo，注意它是一个命令列表

#3 命令参数（args）在这种情况下只是 "Hello World" 字符串，注意你可以为更复杂的命令发送参数列表

基于此示例，你可以为任何你想要的内容创建任务，因为你有灵活性来定义使用哪个容器以及运行哪些命令。一旦你有了任务定义，你需要通过使用 `kubectl apply -f task.yaml` 将此文件应用到集群中，使其对 Tekton 可用。通过将文件应用到 Kubernetes，我们只是使定义对集群中的 Tekton 组件可用，但任务不会运行。

如果你想运行此任务，任务可以被执行多次，Tekton 要求你创建一个如下的 TaskRun 资源：

```
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  generateName: hello-run-
spec:
  taskRef:
    name: echo-hello-world
```

或者，你可以使用 `tkn` 来启动任务定义：

```
salaboy> tkn task start echo-hello-world
TaskRun started: echo-hello-world-run-q7vgw

In order to track the TaskRun progress run:
tkn taskrun logs echo-hello-world-run-q7vgw -f -n default
```

然后通过运行建议的命令获取日志：

```
salaboy> tkn taskrun logs echo-hello-world-run-q7vgw -f -n default
[echo] Hello World
```

无论你将此 TaskRun 应用到集群（`kubectl apply -f taskrun.yaml` 或使用 `tkn task start`），Pipeline Engine 都会执行此任务。在 YAML 文件中，你可以看到此资源没有 `metadata.name`，而是具有 `metadata.generateName` 字段。当 Tekton 运行此任务时，它会为资源生成一个唯一名称以跟踪该特定执行。你可以继续应用相同的资源，每次将其应用到集群时，都会安排一次新的执行。TaskRun 资源用于保留 Task 定义的执行的所有信息以及这些执行的输出。

## TEKTON 中的 PIPELINES

Task 本身可能很有用，但当你使用 Pipelines 创建这些任务的序列时，Tekton 变得非常有趣。

pipeline 是这些任务在具体序列中的集合，让我们看一下在 Tekton 中定义的简单 Service Pipeline（`service-pipeline.yaml`）：

```
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: service-pipeline
spec:
  resources:
    ...
tasks:
  - name: clone-repository
    taskRef:
      name: git-clone
      bundle: gcr.io/tekton-releases/catalog/upstream/git-clone:0.4
    params:
      ...
  - name: maven-build
    runAfter: [clone-repository]
    taskRef:
      name: maven
      bundle: gcr.io/tekton-releases/catalog/upstream/maven:0.2
    params:
      ...
  - name: docker-image-build-and-publish
    runAfter: [maven-build]
    taskRef:
      name: kaniko
      bundle: gcr.io/tekton-releases/catalog/upstream/kaniko:0.4
    params:
      ...
```

你可以在这里找到完整的 pipeline 定义：https://github.com/salaboy/from-monolith-to-k8s/blob/master/tekton/service-pipeline.yaml

再次，你需要将此 pipeline 资源应用到你的集群，以便 Tekton 知道：`kubectl apply -f service-pipeline.yaml`。

正如你在 pipeline 定义中看到的，`spec.tasks` 字段包含一个任务引用的数组。这些任务需要在集群中已经部署，Pipeline 定义负责定义这些任务将被执行的顺序。这些 Task 引用可以是你自己的任务，或者像示例中一样，可以来自 Tekton Catalog，这是一个包含社区维护的任务定义仓库，你可以重用它们。

同样地，就像 Task 需要 TaskRun 进行执行一样，你需要为每次想要执行 Pipeline 时创建一个 PipelineRun。

```
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  generateName: service-pipeline-
spec:
  pipelineRef:
    name: service-pipeline
```

现在当你将此文件应用到集群 `kubectl apply -f pipelinerun.yaml` 时，Tekton 将通过运行 pipeline 定义中定义的所有任务来执行 pipeline。或者你也可以使用 `tkn`：

```
salaboy> tkn pipeline start service-pipeline
```

遵循与此处定义的 service pipeline 类似的方法，我们还可以创建一个 Environment Pipeline，它将负责将包含需要部署到环境中的服务的 git 仓库的状态同步到正在运行的 Kubernetes 集群。

```
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: staging-environment-pipeline
spec:
  tasks:
  - name: clone-repository
    taskRef:
      name: git-clone
      bundle: gcr.io/tekton-releases/catalog/upstream/git-clone:0.4
    params:
    - name: url
      value: $(params.gitRepository)
    - name: revision
      value: $(params.gitRevision)
  - name: helm-update
    runAfter: [clone-repository]
    taskSpec:
      steps:
      - name: update
        image: quay.io/roboll/helmfile:helm3-v0.135.0
        script: |
          #!/bin/sh
          set -ex
          helmfile sync
```

再次，此 pipeline 非常简单，你可以在这里找到完整的 pipeline 定义：https://github.com/salaboy/from-monolith-to-k8s/blob/master/tekton/environment-pipeline.yaml

此 pipeline 包含两个任务，一个从 Environment Repository 克隆源代码，在这种情况下包含一个 helmfile，定义了环境中安装的内容，第二个任务只是将此文件应用到配置的集群。用于运行此 pipeline 的仓库可以在这里找到：https://github.com/salaboy/fmtok8s-staging-env

你可以使用以下命令运行此 pipeline：

```
salaboy> tkn pipeline start staging-environment-pipeline -s runner -w name=sources,volumeClaimTemplateFile=workspace-template.yaml
```

由于此 pipeline 将执行针对 Kubernetes API 的请求，因此需要特定的 RBAC（基于角色的访问控制配置），因此 `-s` 参数指示 `runner` service account 对于此 pipeline 的工作是必需的。

如果需要，你可以在以下仓库找到有关如何在 Kubernetes 集群中安装 Tekton 以及如何运行 Service 和 Environment pipelines 的逐步教程：https://github.com/salaboy/from-monolith-to-k8s/tree/master/tekton

## TEKTON 优势和额外功能

正如我们所见，Tekton 超级灵活，允许你创建相当高级的 pipelines，并且它包含其他功能，例如：

