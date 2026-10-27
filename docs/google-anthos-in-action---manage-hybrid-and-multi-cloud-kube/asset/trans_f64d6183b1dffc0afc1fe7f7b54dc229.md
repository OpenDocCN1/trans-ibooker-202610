
安装执行包括节点初始化和加入。

- **安装附加组件：** 附加组件如 GKE Connect Agent、日志和监控组件、裸金属操作器、MetalLB 均被安装。
- **Pivot（切换）：** 将裸金属资源从 KinD 集群迁移到已配置集群的过程。之后，K8s 资源将在 KinD 集群中被删除。

最后，让我们深入探讨如何创建集群！

## 登录 GCP 并下载 bmctl 工具

请记住，Google 提供了一个名为 bmctl 的工具，用于在裸金属上部署 Anthos。在用于部署集群的工作站上，按照以下步骤下载 bmctl 工具（在我们的示例中，我们假设工作目录为 ~/baremetal）

- 1. 使用 gcloud 登录，执行 `gcloud auth application-default login`，使用一个拥有“安装要求”段落中“安装账户角色要求”部分所描述角色的用户。
- 2. 从文档中提供的 URL 或存储桶下载 bmctl 工具——在撰写本文时，bmctl 可用于 Anthos 1.9，地址为：

```
gsutil cp gs://anthos-baremetal-release/bmctl/1.9.0/linux-amd64/b
```

- 3. 使 bmctl 可执行：
- 4. `chmod a+x bmctl`

现在我们已经有了可执行的 bmctl，可以继续创建集群配置了。

## 创建集群配置

要部署集群，我们需要一个包含所有部署参数和选项的集群配置文件。bmctl 工具可以通过 *create config* 选项为我们创建一个新的配置文件：

```
bmctl create config -c CLUSTER_NAME --project-id=CLOUD_PROJECT_ID
```

其中，_CLUSTER_NAME_ 是您要赋予集群的名称，_CLOUD_PROJECT_ID_ 是您要在 Anthos on bare metal 中使用的项目的项目 ID。

如果您尚未启用所需的 API，可以在上述命令中添加 `--enable-apis` 选项来启用它们；如果您之前尚未创建所需的服务账户，可以添加 `--create-service-accounts` 选项，使其连同所需的角色一起创建。

## 填充集群配置文件

在创建集群之前，我们需要妥善准备由 bmctl 创建的配置文件。该文件默认保存在一个名为 _bmctl-workspace_ 的文件夹内、以集群名称命名的子文件夹中。

在本节中，我们将解释配置文件中的各项选项。

### SSH 私钥

这是集群部署期间用于连接节点的 SSH 私钥。

添加 `sshPrivateKeyPath:` 规范，指定有权以 root 身份访问所有目标节点的 SSH 私钥的完整路径，例如：

```
sshPrivateKeyPath: /root/.ssh/anthoskey
```

### GCP 服务账户密钥

如果您在运行 bmctl 之前手动创建了 GCP 服务账户，则需要按照下面的示例，使用服务账户密钥的完整路径填写相关字段；如果您使用了 bmctl 的 `--create-service-accounts` 参数，这些字段将已经填充完成。

```
gkeConnectAgentServiceAccountKeyPath: /root/bmctl-workspace/.sa-k
gkeConnectRegisterServiceAccountKeyPath: /root/bmctl-workspace/.s
cloudOperationsServiceAccountKeyPath: /root/bmctl-workspace/.sa-k
```

### 集群类型

根据所选的集群部署模型，相应地在 Cluster 自定义资源中设置 type 规范的值，在 admin、hybrid 或 standalone 之间选择，如下例所示：

```yaml
---
apiVersion: baremetal.cluster.gke.io/v1
kind: Cluster
metadata:
  name: admin-cluster
  namespace: cluster-admin-cluster
spec:
  type: hybrid
```

### 控制平面配置

根据所选的控制平面架构，在 `controlPlane:` 部分的 `nodePoolSpec:` 规范中添加目标控制平面节点的 IP 地址。以下示例展示了基于三个控制平面节点的高可用架构。请记住，如果您想启用高可用控制平面，至少需要提供三个 IP 地址。

```yaml
controlPlane:
  nodePoolSpec:
    nodes:
      # 控制平面节点池。通常，如果使用高可用部署，这可以是
      # 单个或 3 台机器。
      - address: 172.16.0.3
      - address: 172.16.0.4
      - address: 172.16.0.5
```

### Pod 和 Service 的 CIDR 块

`clusterNetwork:` 部分包含分配给集群内 Pod 和 Kubernetes Service 对象的 CIDR 范围；这些范围仅在集群内部可见，永远不会在外部使用。仅当这些默认值与您的网络上已有服务（集群内任何正在运行的 Pod 可能需要联系的服务）存在重叠时，才更改默认值。

### 负载均衡器配置

您需要根据所选的负载均衡器模式（bundled 或 manual）以及该模式所需的配置选项来填充 `loadBalancer:` 部分。以下是各种规范的说明：

- **mode**：负载均衡器模式，您需要在 bundled 或 manual 之间选择
- **ports.controlPlaneLBPort**：负载均衡器为 Kubernetes API server 提供服务的端口
- **vips.controlPlaneVIP**：分配给集群上 Kubernetes API server 的 VIP
- **vips.ingressVIP**：分配给集群上 L7 Istio Ingress 网关的 VIP，必须属于下面定义的地址池。此 VIP 仅在集群为 hybrid、standalone 或 user 时需要，在 admin 集群上不需要，该情况下可以保持注释状态。
- **addressPools**：数据平面负载均衡器用来向 Ingress 网关和 LoadBalancer 类型 Kubernetes Service 对象分配 VIP 的地址池，必须包含上面定义的 Ingress VIP。在 admin 集群上不需要，该情况下可以保持注释状态。
- **nodePoolSpec**：列出您想在其中部署捆绑负载均衡器的节点地址。仅当您想为捆绑负载均衡器指定专用工作节点时才需要使用，如果保持注释状态，负载均衡器将部署在控制平面节点上。

需要记住，如果部署的是捆绑负载均衡器，所有 VIP（控制平面 VIP 以及包含 Ingress 网关的地址池）必须与负载均衡器节点在同一子网中。

以下示例展示了一个 hybrid 集群的配置，捆绑负载均衡器部署在两个 IP 地址为 172.16.0.7 和 172.16.0.8 的专用工作节点上。

```yaml
# 负载均衡器配置
loadBalancer:
  mode: bundled
  ports:
    controlPlaneLBPort: 443
  vips:
    controlPlaneVIP: 172.16.0.16
    ingressVIP: 172.16.0.17
  addressPools:
  - name: pool1
    addresses:
    - 172.16.0.17-172.16.0.26
  nodePoolSpec:
    nodes:
    - address: 172.16.0.7
    - address: 172.16.0.8
```

### 代理配置

如果节点需要通过 http 代理连接到互联网，请使用所需信息填充 `proxy:` 部分：

- url：代理服务器可访问的 URL，格式为 http://username:password@fqdn:port
- noProxy：不应通过代理访问的 IP、主机名或域名列表

以下示例为代理服务器配置了一个条目，该代理服务器可通过 http://172.16.0.101:3128 访问，无需身份验证，并为 172.16.0.0/16 范围添加了一个 noProxy 条目，告知系统将该范围内的 IP 排除在代理服务器之外。

```
proxy:
  noProxy:
    # 指定 IP、主机名和域名的列表
    - 172.16.0.0/16
  url: http://172.16.0.101:3128 # 代理服务器的地址。
```

### Cloud Operations 日志与监控

要配置日志和监控选项，您需要在 `clusterOperations:` 部分中添加 projectID 和 location：

- **projectID**：您希望接收指标和日志的项目 ID
- **location**：您希望存储日志和指标的 Google Cloud 区域。建议选择靠近您本地数据中心的区域。

