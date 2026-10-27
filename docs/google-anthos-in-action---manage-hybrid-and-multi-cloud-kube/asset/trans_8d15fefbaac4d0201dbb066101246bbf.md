
![](img/fb3bebce7b5f593bca1a7f8a1371be06_635_0.png)

截至2022年1月，已有较小的一组（45个解决方案）在GKE on-prem环境中进行了测试。我们可以通过向搜索选项添加适当的过滤器“*deployment-env: gke-on-prem*”（如图2所示）来查看on-prem的解决方案。截至2022年1月，已有78个解决方案在Anthos环境中进行了测试，如图3所示。

**图17.3：可用于Anthos GKE-on-prem环境的解决方案。**

## Marketplace

返回Marketplace页面

您的解决方案

您的订单

筛选类型以进行过滤

## 类别

- 大数据 (4)
- 分析 (3)
- 数据库 (14)
- 机器学习 (3)
- 开发工具 (12)
- 计算 (4)
- 博客与CMS (4)
- 安全 (4)
- 开发栈 (2)
- 网络 (3)

## 类型

Kubernetes应用

## 部署环境

GKE on-prem

45个结果

搜索Marketplace

aqua
Aqua Enterprise Security Platform
Aqua Security Software Inc.
Aqua在Kubernetes上提供云原生安全平台
Anthos

Bitpoke App
Bitpoke
Kubernetes上的WordPress
Anthos

Cassandra Cluster
Google Click to Deploy containers
具有无主复制以实现高可用性的NoSQL数据库
Anthos

paloalto
CN Series NGFW
Palo Alto Networks, Inc.
容器防火墙保护Kubernetes环境
Anthos

Consul
Google Click to Deploy containers
Consul是一个分布式服务网格，用于连接、保护和服务配置
Anthos

适用于GKE的Crunchy PostgreSQL
Crunchy Data
在GKE上可信的开源PostgreSQL即服务
Anthos

etcd
etcd
Google Click to Deploy containers
etcd集群高可用安装
Anthos

Flink Operator
Google Click to Deploy containers
闪电般快速的统一分析引擎
Anthos

Gitlab
Google Click to Deploy containers
GitLab是完整DevOps生命周期的单一应用程序
Anthos

在浏览市场中的解决方案时，您可以通过查找列表上附带的“Works with Anthos”徽标来识别与Anthos兼容的第三方解决方案。在图4中，您可以看到一些解决方案

带有一个小型的Anthos按钮——该按钮用于展示已经过认证可与Anthos配合使用的解决方案。

这些列表符合 **Anthos Ready Program** ([https://cloud.google.com/anthos/docs/resources/anthos-ready-partners](https://cloud.google.com/anthos/docs/resources/anthos-ready-partners)) 的要求，该计划标识了符合Google Cloud互操作性要求并已经过验证可与Anthos平台配合使用的合作伙伴解决方案，以满足企业客户的基础设施和应用程序开发需求。要获得资格，合作伙伴提供的解决方案必须完成、通过并保持集成要求，才能获得“Works with Anthos”徽章。

**图17.4：可用于Anthos环境的解决方案。**

您可以通过Cloud console导航菜单中的“工具”部分访问Private Catalog（见图6）。从那里可以创建新的私有市场、添加应用程序以及配置访问权限。每个Private Catalog应由一个GCP项目托管，并且可以在文件夹和项目级别添加Catalog IAM权限。与GCP组织、文件夹或项目共享目录允许客户与其最终用户共享他们的解决方案。这些步骤非常直观，感兴趣的读者可以上网查找更多信息[^1]。

图17.6：访问Private Catalog。

![](img/fb3bebce7b5f593bca1a7f8a1371be06_640_0.png)

### 17.1.3 在GKE on-prem集群上部署

如果您的意图是向Anthos GKE on-prem集群部署Marketplace中的解决方案，那么您需要在目标集群上定义一个或多个命名空间，并使用一个secret对其进行注释，该secret将允许您部署选定的解决方案。需要执行以下步骤：

- 1. 如果您的集群运行Istio，则默认情况下，与第三方服务的任何外部连接都会被阻止，因此配置Istio出口流量以允许连接到外部操作系统软件包存储库非常重要（请参阅第5章，Anthos Service Mesh：可扩展的安全性和可观测性）。
- 2. 您需要允许从Google Container Registry下载镜像，方法是创建允许访问marketplace.gcr.io的防火墙或代理规则。
- 3. 在您的GKE on-prem集群中，您可能需要创建一个Google Cloud服务账号。这可以通过类似这样的cloud shell完成：

```
gcloud iam service-accounts create sa-name \
  --description="sa-description" \
  --display-name="sa-display-name"
```

- 4. 您需要使用具有Kubernetes“cluster-admin”角色的Kubernetes服务账号（KSA）的令牌或凭据登录到您的Anthos GKE on-prem集群。（在第4章“Anthos，基于Kubernetes构建的计算环境”中讨论了角色。）这将允许您拥有超级用户访问权限，以对任何资源执行任何操作。从控制台，您可以通过运行以下命令生成一对新的公钥/私钥对并下载到您的机器上：

```
gcloud iam service-accounts keys create ~/key.json \
  --iam-account sa-name@project-id.iam.gserviceaccount.com
```

- 5. 如果您是首次从Cloud Marketplace部署，则需要在集群中通过运行以下命令创建application-system命名空间：

```
kubectl create namespace application-system
```

- 6. 然后，您需要使用以下命令为application-system创建一个包含imagePullSecret的Kubernetes Secret：

```
JSON_KEY_FILENAME=path_to/service_account_key.json
IMAGEPULLSECRET_NAME=gcr-json-key
kubectl create secret docker-registry $IMAGEPULLSECRET_NAME \
  --namespace="application-system" \
  --docker-server=gcr.io \
  --docker-username=_json_key \
  --docker-password="$(cat $JSON_KEY_FILENAME)"
```

- 7. 下一步是通过运行以下命令将imagePullSecret应用到application-system命名空间中的默认Service Account：

```
kubectl patch sa default -n application-system -p ""imagePullSecr
```

8. 最后，对于您要部署应用程序的每个命名空间，您必须使用以下命令创建一个新的Secret，并使用该Secret注释该命名空间：

```
JSON_KEY_FILENAME=path_to/service_account_key.json
IMAGEPULLSECRET_NAME=gcr-json-key
kubectl create secret docker-registry $IMAGEPULLSECRET_NAME \
  --namespace=$NAMESPACE_NAME \
  --docker-server=gcr.io \
  --docker-username=_json_key \
  --docker-password="$(cat ~/$JSON_KEY_FILENAME)"
```

```
kubectl annotate namespace $NAMESPACE_NAME marketplace.cloud.goog
```

9. 一旦按照上一步所述定义了$NAMESPACE_NAME，该命名空间就可以用于在您的本地集群上进行安装。

换句话说，无论是在GCP上还是在本地部署Anthos，只需在目标Kubernetes集群上定义适当的命名空间，并使用拉取解决方案镜像所需的密钥注释该命名空间即可。

在下一节中，我们将介绍如何安装一些预定义的解决方案，这些解决方案属于不同类别，包括现代化工具、数据库、监控和日志记录、CI/CD、生产力和机器学习。

### 17.2 真实场景

在本节中，我们将简要介绍如何部署适用于Anthos的Marketplace解决方案。我们的示例有意采用异构方式，并且属于不同类别。请注意，我们已经在第18章“Migrate for Anthos”中从Cloud Marketplace部署了Migrate for Anthos。因此，我们在这里跳过它，但它是一个非常可靠的用例。让我们再看几个额外的示例。

### 17.2.1 示例1：ElasticSearch

实例名称elasticsearch-1和2个副本。

# 图17.8：部署ElasticSearch解决方案

![](img/fb3bebce7b5f593bca1a7f8a1371be06_644_0.png)

创建集群可能需要几分钟时间（见图9），这始终是喝杯咖啡休息一下的好理由，让Marketplace替我们完成所有工作。

# 图17.9：为新的ElasticSearch解决方案创建集群
