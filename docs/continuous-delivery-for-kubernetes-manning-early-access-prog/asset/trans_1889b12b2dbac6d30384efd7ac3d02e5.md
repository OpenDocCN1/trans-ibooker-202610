

- 长期维护这些组件，进行备份并确保故障转移机制按预期工作。

![](img/2ab98a5ff1ec073700d902ba95287fd6_103_0.png)

图 4.2 使用 PostgreSQL Helm Chart 部署新的 PostgreSQL 实例

如果你正在使用 Helm Charts，有一些注意事项和技巧你需要了解：

- 如果 Chart 不允许你配置你感兴趣更改的参数，你始终可以使用 `helm template`，然后修改输出以添加或更改你需要的参数，最后使用 `kubectl apply -f` 安装组件。或者，你可以向 Chart 仓库提交 Pull Request。不公开所有可能参数是一种常见做法，并等待社区成员建议让 Chart 暴露更多参数。如果是这种情况，不要害羞并联系维护者。无论你进行何种修改，Chart 内容都需要被维护和记录。通过使用 `helm template` 你将失去 Helm 发布管理功能，该功能允许你在有新 Chart 版本时升级 Chart。
- 大多数 Chart 都带有默认配置，该配置旨在实现扩展，这意味着默认部署将针对高可用性场景。这种情况会导致安装 Chart 时消耗大量资源，如果你使用的是笔记本电脑上的 Kubernetes KIND 或 Minikube，可能无法提供这些资源。同样，Chart 文档通常包含针对开发和资源受限环境的特殊配置。
- 如果你在 Kubernetes 集群内部安装数据库，每个数据库容器（Pod）都需要能够访问底层 Kubernetes 节点的存储。对于数据库，你可能需要一种特殊的存储类型以使数据库能够弹性扩展，这可能需要 Kubernetes 之外的高级配置。

如果你的 Conference Platform 已在 Kubernetes 集群中运行，你可以通过使用各自的 Chart 继续安装前面提到的组件。首先，你需要添加 Bitnami Helm Chart 仓库：

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

然后使用以下两行安装 Chart：

```
helm install postgresql bitnami/postgresql

helm install redis bitnami/redis
```

这两个命令的输出将为你提供如何访问用于连接实例的密码的说明，该密码存储在 Kubernetes Secret 中。你还会看到用于连接应用程序所需的 Kubernetes Service 名称。这是我们在部署其他应用程序将连接的新组件时在 Kubernetes 中的常见模式。找到服务的正确 URL、用户和密码对于你的应用程序连接至关重要。

如果你使用 `kubectl` 列出正在运行的 Pod，你应该会得到类似以下内容：

```
> kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
app-fmtok8s-agenda-rest-68bd9c8bcb-dxbdt   1/1     Running   0          12m
app-fmtok8s-api-gateway-58d49588b4-vw7pc   1/1     Running   0          12m
app-fmtok8s-c4p-rest-7c8b0c4485-scj7r      1/1     Running   0          12m
app-fmtok8s-email-rest-8f954fbbd-p8hld    1/1     Running   0          12m
postgresql-0                      1/1     Running   0          6m30s
redis-master-0                    1/1     Running   0          3m47s
redis-replicas-0                  1/1     Running   0          3m47s
redis-replicas-1                  1/1     Running   0          3m13s
redis-replicas-2                  1/1     Running   0          2m36s
```

我们可以看到 PostgreSQL Chart 默认创建单个副本（Pod），而 Redis 则创建了一个主节点和 3 个副本。不同数据库具有不同的可扩展性机制和默认配置是很常见的。

## 连接我们的服务到新部署的基础设施

注意：Agenda 服务和征集提案（C4P）服务都已特别准备以支持内存存储和外部持久化。在你的项目中，除非你正在构建概念验证，否则不应依赖内存方法。

![](img/2ab98a5ff1ec073700d902ba95287fd6_105_0.png)

#1 创建一个 Kubernetes Deployment 来运行你的其中一个服务，Pod 模板包含用于配置此 Deployment 将创建的 Pod 的环境变量。
#2 Pod 是使用 Deployment 资源中指定的模板创建的，该模板指向包含连接 db 实例详细信息的 Secret。
#3 运行在 Pod 内部的容器需要准备好使用环境变量来连接 db 实例。

图 4.3 使用 Secrets 将服务连接到已部署的资源

让我们先连接征集提案（C4P）服务，你可以通过编辑名为 `app-fmtok8s-c4p-rest` 的 Kubernetes Deployment 并添加一些环境变量来指定数据库连接属性来实现。查找 `spec.template.spec.containers[0].env` 部分并添加以下新变量：

```
- name: SPRING_DATASOURCE_DRIVERCLASSNAME
  value: org.postgresql.Driver
- name: SPRING_DATASOURCE_PLATFORM
  value: org.hibernate.dialect.PostgreSQLDialect
- name: SPRING_DATASOURCE_URL
  value: jdbc:postgresql://${DB_ENDPOINT}:${DB_PORT}/postgres
- name: SPRING_DATASOURCE_USERNAME
  value: postgres
- name: SPRING_DATASOURCE_PASSWORD
  valueFrom:
    secretKeyRef:
      key: postgresql-password
      name: postgresql
- name: DB_ENDPOINT
  value: postgresql
- name: DB_PORT
  value: "5432"
```

加粗部分突出显示了我们如何使用安装 Chart 时动态生成的密码，以及 DB 端点 URL，在本例中为 PostgreSQL Kubernetes Service，也是由 Chart 安装的。如果你使用了不同的 Chart 发布名称，DB 端点将会不同。一旦你保存此部署配置，将启动一个新的 Pod，如果 Pod 成功连接到 PostgreSQL 实例，旧版本将由 Kubernetes 自动移除。

你现在可以增加征集提案服务的副本数量，因为服务现在是无状态的。服务的每个副本都将连接到同一个数据库实例以获取和存储数据。

Agenda 服务和 Redis 也可以这样做。

```
- name: SPRING_REDIS_IN_MEMORY
  value: "false"
- name: SPRING_REDIS_HOST
  value: redis-master
- name: SPRING_REDIS_PASSWORD
  valueFrom:
    secretKeyRef:
      key: redis-password
      name: redis
```

与之前一样，我们从名为 `redis` 的 Secret 中提取密码。REDIS_HOST 来自 Chart 创建的 Kubernetes Service 名称，这取决于你使用的 `helm release` 名称。与 C4P 服务一样，你现在可以为 Agenda 服务添加更多副本以支持更多流量。请记住，你的服务现在也可能崩溃，并且一旦恢复，它们将重新连接到数据库并获取可用数据。

如果你想使用 Helm 在 Kubernetes 集群中安装 PostgreSQL 和 Redis，你可以按照位于此处的逐步教程：https://github.com/salaboy/from-monolith-to-k8s/tree/master/helm-app-infra

我强烈建议你阅读该教程，以便亲身体验 Helm Charts 和 Kubernetes Deployments。

## 4.1.2 安装和监控应用程序基础设施组件的不同方法

根据服务如何使用这些数据库，你可能会发现数据库成为瓶颈。想象一下，如果 Agenda 服务的请求过多，你决定将 Agenda Deployment 的副本数量扩展到 200。此时，Redis 需要有足够的资源来处理 200 个 Pod 连接到 Redis 集群。使用 Redis 的优势在于，在会议进行期间我们可能会有大量读取操作时，Redis 集群允许我们从副本读取数据，从而可以分发负载。

![](img/2ab98a5ff1ec073700d902ba95287fd6_107_0.png)

#1 如果你注意到某个服务的需求激增，你可能会想增加副本数量，而使用 ReplicaSet 的 Deployment 不会对此提出异议。如果集群有足够的资源，副本将被创建。
#2 如果应用程序基础设施配置不正确，你可能会遇到很多问题，例如耗尽数据库连接池，或使数据库 Pod 过载，因为在你扩展 Deployment 时它们不会随之扩展。

图 4.4 应用程序基础设施需要根据我们的服务将如何扩展进行配置

