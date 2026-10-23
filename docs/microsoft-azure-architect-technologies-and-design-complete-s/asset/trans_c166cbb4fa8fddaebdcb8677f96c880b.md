

根据需要配置 Service Fabric 集群。关键在于，仅仅部署一个 Service Fabric 实例并不能让你立即运行起来。在此产品变得可用之前，需要进行编码和一些重要的配置。实施的门槛有点高，需要一些熟练的 IT 专业人员来部署和支持它。然而，对于面向 .NET Framework 的大型微服务工作负载而言，这已被证明是一个不错的选择。

## Azure Kubernetes Service

从原则上讲，Service Fabric 和 Azure Kubernetes Service (AKS) 提供了类似的功能。两者之间的区别，或者说你会选择其中一个而不是另一个的原因，归结为两点：开源与 .NET Framework（图 4.2）和中级复杂性与高复杂性。在阅读上一节关于 Service Fabric 的内容时，并没有提到 Docker、Azure Container Instance (ACI) 或 Azure Container Registry (ACR) 等。例如。AKS 专门设计用于使用 Docker 文件格式或 ACR 创建的容器的消费。AKS 不仅从头开始使用开源技术开发，而且专门设计用于运行开源应用程序。Service Fabric 可以运行开源代码并提供 Linux 作为运行它们的操作系统；然而其最初的目标是 .NET Framework 开发堆栈。此外，AKS 启动和运行起来稍微不那么复杂。入门门槛要求较低。就 Service Fabric 而言，工作负载的编排需要编码。就 AKS 而言，工作负载的默认部署和管理不需要编码。这两个编排器都专注于微服务的容器化；然而，每个都针对特定类型的应用程序（开源与 .NET Framework）。每个的先决条件和维护活动的复杂程度都不同。图 4.61 说明了 AKS 概念，这将在本节中讨论。

该说明不仅描述了 AKS 组件（如集群主节点），还描述了它与 Azure DevSpaces、Azure DevOps、Azure Monitor 以及 Docker Hub 和 ACR 等容器存储库的紧密集成。让我们先简要讨论 Kubernetes 与 AKS 以及 Azure 上在此之前存在的内容。

## Kubernetes vs. AKS

Kubernetes 本身就是一个用于自动化部署、扩展和管理应用程序的完整系统。如果你愿意，可以将 Kubernetes 本身部署到 Azure 平台上并使用 Kubernetes 产品，而无需触及 AKS。AKS 提供的就是免费运行集群主节点（Kubernetes 控制平面）的基础设施。你将在图 4.61 中看到集群主节点。只有运行你的应用程序的集群中的节点才会产生费用，因此仅使用 AKS，你就能获得一些免费的计算资源，否则会产生账单。集群主节点具体如何架构是平台特定的，你不需要太担心；然而，花一些时间介绍构成集群主节点的各个组件是值得的。集群主节点组件如下：

- The APIs
- etcd
- The scheduler
- The controller

### FIGURE 4.61 An end-to-end illustration of Azure Kubernetes Services

![](img/95043b3df926bcdadadb082b49262037_378_0.png)

API 服务器使用 JSON over HTTP REST 连接到内部 Kubernetes API，该 API 读取/写入 etcd 数据存储以配置集群中节点上的工作负载和容器。etcd 数据存储是集群当前状态存储的位置。假设你在集群中的多个节点上运行了大量 pod，并且你想用新版本的容器镜像更新它们。pod 有多少实例以及在哪个节点上运行，所有这些都存储在 etcd 中，有助于找到更新需要推出到哪里。此外，了解你有多少节点实例在扩展时是有帮助的。调度器也发挥着重要作用，如果你有多个实例，则不希望同时对所有实例进行更改。调度器将确保它们一个接一个地更新，从而实现最高可用性。控制器是维护集群期望状态与当前状态之间的引擎。假设你请求手动扩展到运行特定 pod 的特定节点的五个实例；请求将发送到 API 服务器。控制器将通过 etcd 了解当前存在多少实例，确定还需要添加多少实例，然后承担将它们全部预配、配置并接受流量的责任。然后，一旦实际集群状态与期望状态匹配，就会更新 etcd。如果实现非常庞大，这里可能会有很多事情发生。AKS 为你免费提供集群主节点计算和接口。

开发者由一个名为 Azure Dev Spaces 的服务提供，你稍后会看到并使用它。我想在这里简要向你介绍的最后一个概念是 GitHub 和 Azure DevOps。这两个产品都设计用于应用程序源代码的管理和存储。记住在练习 4.2 中，使用 Azure Container Instance (ACI) 时，你从 GitHub 获取了一些源代码来创建在该容器中运行的应用程序。Azure DevOps 存在相同的一组功能，如图 4.61 中所示，其中展示了源代码存储库和 Azure Dev Pipeline。有关 Azure DevOps 的更多内容将在第 8 章中讨论部署的细节。如有需要，请在那里阅读更多内容。

要了解有关 AKS 的更多信息并使用 Azure Dev Spaces，请完成练习 4.17，在其中你将使用 Azure CLI 创建一个 Azure Kubernetes Service 集群，然后使用 Visual Studio 将 ASP .NET Core Web 应用程序部署到该集群。对于此示例，我使用了 Visual Studio Community 2019（16.2.4），可免费从此处下载：

```
visualstudio.microsoft.com/downloads/
```

## EXERCISE 4.17

## Creating an AKS Cluster and Deploying an ASP.NET Core Web Application

- 1. Log in to Azure Shell at https://shell.azure.com and enter the following command, replacing <name> with your resource group and ASK cluster name. Note that the line is broken for clarity; please enter each command on a single line, one after the other. See Figure 4.62, which illustrates a summarized example.

```
az aks create --resource-group <name> --name <name>
  --node-count 1 --enable-addons monitoring --generate-ssh-keys

az aks get-credentials --resource-group <name> --name <name>

kubectl get nodes
```

### FIGURE 4.62 AKS Azure CLI commands

![](img/95043b3df926bcdadadb082b49262037_380_0.png)

- 2. After the commands complete, log in to the Azure Portal at https://portal.azure.com. Navigate to the Azure Kubernetes Services blade, select the AKS cluster that you just created, click Node Pools, and view the nodepool1. Click the Dev Spaces link on the navigation menu, toggle the switch to Yes to enable it, and then click Save.

## EXERCISE 4.17 (continued)

- 3. Open Visual Studio 2019 and create an ASP.NET Core Web Application, walking through the creation wizard. Select the project shown in Figure 4.63.

### FIGURE 4.63 Selecting the ASP.NET Core Web Application Visual Studio project

![](img/95043b3df926bcdadadb082b49262037_381_0.png)

- 4. Create a .NET Core/ASP.NET Core Web Application, similar to that shown in Figure 4.64.

### FIGURE 4.64 Creating the .NET Core/ASP.NET Core Web Application Visual Studio project

![](img/95043b3df926bcdadadb082b49262037_381_1.png)

- 5. Click Azure Dev Spaces, as shown in Figure 4.65.

![](img/95043b3df926bcdadadb082b49262037_382_0.png)

- 6. When the Azure Dev Spaces window pops up, select the identity, the subscription, and the AKS cluster created in step 1. Keep the other defaults. Check the Public Accessible check box and then click the OK button. Open the Output window in Visual Studio to watch the progress.

- 7. Once built and deployed, the public IP address will be found in the Visual Studio Output window and will look similar to that shown in Figure 4.66. Access the URL in a browser.

![](img/95043b3df926bcdadadb082b49262037_382_1.png)

- 8. Navigate back to Azure Cloud Shell and enter the following command to view additional information about the application running on the AKS cluster:

```
```bash
kubectl get service azure-vote-front --watch
```
```

Figure 4.67 illustrates the command and the potential output.

