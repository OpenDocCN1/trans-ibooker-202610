

## 生产中 AI 模型失效的 7 个原因

> 模型在训练时表现良好，但在真实场景中却一败涂地。问题往往不在于模型本身，而在于我们构建模型的方式。

- 数据分布偏移（Data Drift）
- 过度拟合至验证集
- 特征分布不一致

在实际生产中，模型失效的最常见原因是训练数据与真实世界数据之间的分布差异。当数据的统计特性随时间发生变化时，模型的性能会急剧下降。

```python
skew = abs(train_mean - live_mean)
if skew > threshold:
    trigger_retraining()
```

持续监控数据分布，并在分布偏移超限时触发重新训练，是保证模型长期有效的重要手段。

- 建立特征健康度检查
- 设定模型性能回退机制
- 定期人工审计模型输出

### 负载均衡器配置

要使用捆绑的 MetalLB 负载均衡器，请在集群配置文件中指定 `loadBalancer` 部分。以下示例为控制平面设置虚拟 IP：

```yaml
loadBalancer:
    vips:
        controlPlaneVIP: "133.23.22.100"
    kind: MetalLB
```

如果使用其他类型的负载均衡器，例如 F5 BIG-IP，请相应地配置字段。

### 反亲和性组

`antiAffinityGroups.enabled` 控制控制平面节点是否放置在不同的 ESXi 主机上。启用反亲和性组有助于保护控制平面免受单个主机故障或 ESXi 维护事件的影响。如果禁用此选项，您可以减少所需的 ESXi 主机数量，但会增加控制平面停机的风险。

```yaml
antiAffinityGroups:
    enabled: true/false
```

### Stackdriver / Cloud Monitoring

要将指标和日志导出到 Google Cloud，请配置 `stackdriver` 部分：

```yaml
stackdriver:
    projectID: "my-logs-project"
    clusterLocation: "us-central1"
    enableVPC: false
    serviceAccountKeyPath: "my-key-folder/log-mon-key.json"
    disableVsphereResourceMetrics: true
```

服务账号密钥文件必须具有适当的 Cloud Monitoring 和 Cloud Logging 角色。如果未使用 VPC，请将 `enableVPC: false`。

### Cloud Audit Logging

要启用 Cloud Audit Logging，请配置 `cloudAuditLogging` 部分：

```yaml
cloudAuditLogging:
    projectID: "my-audit-project"
    clusterLocation: "us-central1"
    serviceAccountKeyPath: "my-key-folder/audit-log-key.json"
```

审计日志配置跟踪集群生命周期变更，并需要具有适当权限的服务账号。

### 节点自动修复

`autoRepair` 控制是否自动修复不健康节点。启用后，`cluster-health-controller` 会检查节点健康状况并排空并重新创建不健康节点，如图 9.5 所示。

```yaml
autoRepair:
    enabled: true/false
```

**图 9.5. 节点自动修复流程**

![](img/fb3bebce7b5f593bca1a7f8a1371be06_265_0.png)

> **NOTE**：自动修复由 `cluster-health-controller` 部署实现。它监控管理集群和用户集群中的节点并修复不健康节点。

### 私有仓库

要使用私有容器仓库，请指定包含仓库地址、凭据和可选 CA 证书的 `privateRegistry` 部分：

```yaml
privateRegistry:
  address: "{{Private_Registry_IP_address}}"
  credentials:
    fileRef:
      path: "{{my-config-folder}}/admin-creds.yaml"
      entry: "private-registry-creds"
    caCertPath: "my-cert-folder/registry-ca.crt"
```

### 密钥加密

Anthos on VMware 支持静态加密 Kubernetes 密钥。要启用此功能，请设置 `secretsEncryption` 部分：

```yaml
secretsEncryption:
  mode: GeneratedKey
  generatedKey:
    keyVersion: 1
```

> **TIP**：使用 `gcloud` 命令或适当的更新命令轮换生成的加密密钥，然后在配置中更新密钥版本。

![](img/fb3bebce7b5f593bca1a7f8a1371be06_268_0.png)

> **IMPORTANT**：部署后，**不能**在不完全重新创建集群的情况下更改主节点数量。

### 用户集群命名空间

每个用户集群都在管理集群中的专用命名空间中部署。命名空间包含用户集群服务、部署和其他 Kubernetes 资源。

命名空间名称与集群名称相关联，因此您可以使用以下命令查找用户集群中部署的所有资源：

```
kubectl get all -n {{ clusterName }}
```

用户集群的命名空间在用户集群创建时自动创建，并由用户集群生命周期控制器管理。不得手动修改。

**图 9.7. Anthos on VMware 用户集群命名空间**

| Admin Cluster | User Cluster name | User Cluster |
| --- | --- | --- |
| config-management-system | kube-public | default |
| kube-system | gke-connect | knative-serving |
| gke-system | kube-node-lease |  |

与管理集群类似，用户集群是从 YAML 配置文件部署的。基本资源定义为：

```yaml
apiVersion: v1
kind: UserCluster
```

> **TIP**：您可以使用以下命令转换旧版本的配置文件：
>
> ```
> gkectl create-config cluster --config $MyAwesomeClusterConfigFile
> ```

### 网络配置

要定义集群网络，请使用 DHCP 或静态 IP 模式配置 `network` 部分。

DHCP 模式：

```yaml
network:
  ipMode:
    type: dhcp
  serviceCIDR: 10.96.232.0/24
  podCIDR: 192.168.0.0/16
  vCenter:
    networkName: "My Anthos on VMware user network"
  additionalNodeInterfaces:
  - networkName: "My additional network"
    type: dhcp
```

静态 IP 模式：

```yaml
network:
  ipMode:
    type: "static"
    ipBlockFilePath: "myNodeHostConfFile.yaml"
  additionalNodeInterfaces:
  - networkName: "My additional network"
    type: "static"
    ipBlockFilePath: "mySecondNodeHostConfFile.yaml"
```

在开始集群创建之前，确定主节点将使用 3 个还是 1 个副本。3 副本控制平面提供高可用性；1 副本控制平面不提供。

以下示例定义主节点资源：

```yaml
masterNode:
  cpus: 4
  memoryMB: 8192
  replicas: 3
```

> **TIP**：配置文件为 YAML。枚举值应作为字符串引用；否则 YAML 可能会错误解释它们。诸如 `cpus`、`memoryMB` 和 `replicas` 之类的数字字段应使用整数值。

