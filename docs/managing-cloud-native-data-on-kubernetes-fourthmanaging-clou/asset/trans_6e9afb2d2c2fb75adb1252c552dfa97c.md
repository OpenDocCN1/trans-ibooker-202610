

```
deployment.apps/wordpress-mysql
REVISION  CHANGE-CAUSE
1         <none>
```

正如你所见，Kubernetes Deployment 为管理一组克隆 Pod 的生命周期提供了一些复杂的特性。你可以通过修改 Deployment 的 YAML 规范并重新应用它来测试这些生命周期操作（回滚除外）。尝试将副本数缩放至 2 再缩回，或使用不同的 MySQL 镜像。更新 Deployment 后，你可以使用类似 `kubectl describe deployment wordpress-mysql` 的命令来观察 Kubernetes 为使 Deployment 达到期望状态而发起的事件。

Deployment 还有其他可用的选项，这里没有空间展开，例如如何指定当尝试的更新失败时 Kubernetes 的行为。有关 Deployment 行为的更深入解释，请参阅 Kubernetes 文档。

## Services

在上述步骤中，你已创建了一个 PVC 来指定数据库的存储需求，一个 Secret 来提供管理员凭据，以及一个 Deployment 来管理单个 MySQL Pod 的生命周期。现在你有了运行中的数据库，你希望使其可供应用程序访问。在我们于第 1 章介绍的计算、网络和存储方案中，这属于网络部分。

Kubernetes Service 是我们需要使用的原语，以便将对数据库的访问作为网络服务暴露出来。Service 为其背后运行的一组 Pod 提供了抽象。在本例中只有一个 MySQL 节点，你可能会想知道为什么要创建这种抽象。一个 Service 支持的关键特性是提供一个一致命名的端点，且不会改变。你不希望在数据库 Pod 重启并获得新的

IP地址时，不得不更新你的客户端。你可以使用如下的 YAML 配置为访问 MySQL 创建一个 Service（源代码）：

```
apiVersion: v1
kind: Service
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  ports:
    - port: 3306
  selector:
    app: wordpress
    tier: mysql
clusterIP: None
```

以下是关于此配置需要注意的几点：

- 首先，此配置指定了在 Service 上暴露的端口：3306。在定义 Service 时实际上涉及两个端口：暴露给 Service 客户端的端口，以及 Service 所代理的底层 Pod 暴露的 `targetPort`。由于你未指定 `targetPort`，它默认为 port 的值。
- 其次，选择器定义了 Service 将流量导向哪些 Pod。在此配置中，只会有一个由 Deployment 管理的 MySQL Pod，这完全没问题。
- 最后，如果你之前使用过 Kubernetes Service，你可能会注意到此 Service 未定义 `serviceType`，这意味着它是默认类型，即 `ClusterIP`。此外，由于 `clusterIP` 属性被设为 None，这就是所谓的 *无头服务*，即 Service 的 DNS 名称直接映射到所选 Pod 的 IP 地址。

Kubernetes 支持多种类型的服务以满足不同的用例，如图 3-3 所示。我们将在此简要介绍它们，以突出它们对数据基础设施的适用性：

### ClusterIP Service

此类 Service 暴露在只能从 Kubernetes 集群内部访问的 IP 地址上。这是在 Kubernetes 中用于数据基础设施（如数据库）的最常见的服务类型，尤其是无头服务，因为此类基础设施通常与使用它的应用程序一起部署在 Kubernetes 中。

### NodePort Service

NodePort Service 在每个工作节点的 IP 地址上对集群外部暴露。还会内部创建一个 ClusterIP 服务，NodePort 将流量路由到该服务。你可以让 Kubernetes 选择使用的外部端口，或使用 NodePort 属性指定所需端口。NodePort 服务最适合开发环境，当你需要调试数据基础设施应用程序特定实例上发生的情况时。

### LoadBalancer

LoadBalancer 服务代表 Kubernetes 运行时请求设置由底层云提供商提供的负载均衡器。例如，在 Amazon 的 Elastic Kubernetes Service（EKS）上，请求一个 LoadBalancer 服务会导致创建一个 Elastic Load Balancer（ELB）实例。在多节点数据基础设施部署前面使用 LoadBalancer 通常不是必需的，因为这些数据技术通常有自己的负载分发方法。例如，Apache Cassandra 驱动程序了解 Cassandra 集群的拓扑结构，并为客户端应用程序提供负载均衡功能，从而消除了对负载均衡器的需求。

### ExternalName Service

ExternalName Service 通常用于表示对集群外部服务的访问，例如在 Kubernetes 外部运行的数据库。ExternalName 服务不

具有选择器，因为它不映射到任何 Pod。相反，它将 Service 名称映射到 CNAME 记录。例如，如果你创建一个 `my-external-database` 服务，其 `externalName` 为 `database.mydomain.com`，则应用程序 Pod 中对 `my-external-database` 的引用将被映射到 `database.mydomain.com`。

![](img/cb55ccac39ceccbc2806f8db5c8fad69_89_0.png)

图 3-3. Kubernetes 服务类型

还请注意图中包含 Ingress。虽然 Kubernetes Ingress 不是一种 Service 类型，但它相关。Ingress 用于从集群外部提供对 Kubernetes 服务的访问，通常通过 HTTP。存在多种 Ingress 实现，包括 Nginx、Traefik、Ambassador（基于 Envoy）等。Ingress 实现通常提供包括 SSL 终止和跨多个不同 Kubernetes 服务的负载均衡在内的功能。与 LoadBalancer 服务一样，Ingress 更常用于应用层。

## Accessing MySQL

现在你已部署了数据库，可以部署使用它的应用程序了——Wordpress 服务器。

首先，服务器需要自己的 PVC。这有助于说明存在直接利用存储的应用程序的情况，也许用于存储文件，以及使用数据基础设施的应用程序，以及两者兼具的应用程序。你可以提出一个较小的请求，因为这只是演示目的（[source code]()）：

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wp-pv-claim
  labels:
    app: wordpress
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

接下来，创建一个单个 Wordpress 节点的 Deployment（[source code]()）：

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
  labels:
    app: wordpress
spec:
  selector:
    matchLabels:
      app: wordpress
      tier: frontend
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: wordpress
        tier: frontend
```

```
spec:
  containers:
  - image: wordpress:4.8-apache
    name: wordpress
    env:
    - name: WORDPRESS_DB_HOST
      value: wordpress-mysql
    - name: WORDPRESS_DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: mysql-root-password
          key: password
    ports:
    - containerPort: 80
      name: wordpress
    volumeMounts:
    - name: wordpress-persistent-storage
      mountPath: /var/www/html
  volumes:
  - name: wordpress-persistent-storage
    persistentVolumeClaim:
      claimName: wp-pv-claim
```

请注意，访问 MySQL 的数据库主机和密码作为环境变量传递给 Wordpress。主机值是你上面为 MySQL 创建的服务名称。这就是数据库连接被路由到你的 MySQL 实例所需的一切。密码值从 Secret 中提取，与上面 MySQL 部署的配置类似。

你还会注意到 Wordpress 在 80 端口暴露 HTTP 接口，因此让我们创建一个服务来暴露 Wordpress 服务器（source code）：

```
apiVersion: v1
kind: Service
metadata:
  name: wordpress
  labels:
    app: wordpress
spec:
  ports:
  - port: 80
  selector:
    app: wordpress
```

