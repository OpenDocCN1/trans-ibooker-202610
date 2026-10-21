
```
---
apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
spec:
  type: ClusterIP
  selector:
    app: hello-world
  ports:
  - port: 8080
    targetPort: 8080
```

端口表示服务可用的端口，而 targetPort 是服务将被转发到的实际容器端口。在此例中，我们将 hello-world 应用的 8080 端口在 Pod IP 上暴露到了 targetPort 8080：

```
$ kubectl get svc
NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP
PORT(S)              AGE
hello-world-service  ClusterIP   10.32.0.40     <none>
8080/TCP             6s
```

显示的 IP 地址是集群 IP，而不是实际的机器 IP 地址。如果你在 worker 节点上通过 SSH 登录，可以通过简单执行如下 curl 来检查服务是否已在 8080 端口暴露：

```
ubuntu@worker0:~$ curl http://10.32.0.40:8080
Hello World
```

如果你尝试从集群外部访问此 IP 地址（即除 worker 之外的任何其他节点），你将无法连接到它。因此，你可以使用 NodePort，它会在每个节点的 IP 上以定义的端口暴露服务，如下所示：

```
---
apiVersion: v1
kind: Service
metadata:
  name: hello-world-node-service
spec:
  type: NodePort
  selector:
    app: hello-world
  ports:
  - port: 8080
    targetPort: 8080
    nodePort: 30767
```

在服务清单中，我们将 Pod 端口 8080 映射到了 NodePort（即物理实例端口）30767。这样，你可以直接暴露 IP 或放置你选择的负载均衡器。现在执行 get svc，你可以看到端口的映射如下：

```
$ kubectl get svc
NAME                 TYPE       CLUSTER-IP     EXTERNAL-IP
PORT(S)              AGE
hello-world-node-service   NodePort   10.32.0.208    <none>
8080:30767/TCP   8s
hello-world-service        ClusterIP  10.32.0.40     <none>
8080/TCP         41m
```

现在我们可以在节点的 30767 端口访问服务：

```
ubuntu@controller0:~$ curl http://10.240.0.21:30767
Hello World
```

curl 命令中的 IP 是 worker 节点的物理 IP 地址（不是集群 IP），而被暴露的端口是 30767。你甚至可以直接访问节点的公网 IP 的 30767 端口。图 4-5 展示了集群 IP、节点端口和负载均衡器之间的关系。

![](img/8d47d1b5dbca55e784ae602145a894c7_140_0.png)

图 4-5. Kubernetes 节点中的集群 IP、节点端口和负载均衡器

其他类型的 Service 包括 LoadBalancer 和 ExternalName。LoadBalancer 使用云提供商的负载均衡器将 Service 对外暴露。NodePort 和 ClusterIP Service 会自动创建，外部负载均衡器会路由到它们，而 ExternalName 将服务映射到 DNS 名称，如 my.redisdb.internal.com。在图 4-6 中，你可以看到不同服务类型之间的关系。

![](img/8d47d1b5dbca55e784ae602145a894c7_141_0.png)

图 4-6. 云环境中的外部服务和 LoadBalancer

## Ingress

Service 对象有助于在集群内部和外部暴露应用程序，但在生产系统中，我们无法为使用 NodePort 部署的所有服务不断开放新的唯一端口，也无法每次将服务类型选择为 LoadBalancer 时都创建一个新的负载均衡器。有时我们需要部署基于 HTTP 的服务并执行 SSL 卸载，而 Service 对象在这些情况下并不能真正帮助我们。在 Kubernetes 中，HTTP 负载均衡（或正式的第 7 层负载均衡）由 Ingress 对象执行。

要使用 Ingress，我们首先需要配置一个 ingress controller。7 我们将配置 Azure Kubernetes Service (AKS) Application Gateway ingress controller 以更好地理解它并观察其行为，但总的来说，了解它的最简单方法之一是查看以下内容：

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-wildcard-host
spec:
  rules:
  - host: "foo.bar.com"
    http:
      paths:
      - pathType: Prefix
        path: "/bar"
        backend:
          service:
            name: hello-world-node-service
            port:
              number: 8080
  - host: "*.foo.com"
    http:
      paths:
      - pathType: Prefix
        path: "/foo"
        backend:
          service:
            name: service2
            port:
              number: 80
```

在 ingress 清单中，我们创建了两条规则，并将 *foo.bar.com* 作为主机映射到一个子域 /bar，该子域路由到我们之前的服务 `hello-world-node-service`。

同样，我们可以为一个域名定义多个路径并将它们路由到其他服务。Ingress 的配置方式有很多种，可以根据你的需求进行配置，可能从单个域名路由到多个服务，或多个域名路由到多个域名（参见图 4-7）。

![](img/8d47d1b5dbca55e784ae602145a894c7_143_0.png)

图 4-7. 具有多个路径的域的 Ingress

最后，你可以通过创建 Secret 对象并在 ingress spec 中使用该 secret 来指定 TLS 支持，如下所示：

```
apiVersion: v1
kind: Secret
metadata:
  name: my_tls_secret
  namespace: default
data:
  tls.crt: base64 encoded cert
  tls.key: base64 encoded key
type: kubernetes.io/tls
```

你可以通过指定 Base64 编码的 TLS 证书和密钥来保护 ingress。当你在 ingress 清单中引用此 secret 时，它将如下所示：

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: tls-example-ingress
spec:
  tls:
  - hosts:
    - https.mywebsite.example.come
secretName: my_tls_secret
rules:
- host: https.mywebsite.example.come
  http:
    paths:
    - path: /
      pathType: Prefix
      backend:
        service:
          name: service1
          port:
            number: 80
```

Ingress controller 处理许多功能和复杂性，而 Azure 网关 ingress controller 处理了其中很多；例如，利用 Azure 原生的第 7 层应用网关负载均衡器将你的服务暴露到互联网。我们将在第 5 章介绍 AKS 时更详细地讨论这一点。

## DaemonSet

正如我们之前讨论的，DaemonSets 通常用于在 Kubernetes 集群中的多个节点上运行代理。代理在由 Pod 抽象的容器内运行。大多数时候，SRE 和 DevOps 工程师更喜欢在每个节点上运行日志代理或监控代理以获取应用遥测和事件。默认情况下，DaemonSet 会在每个节点上创建 Pod 的副本，尽管这也可以使用节点选择器进行限制。ReplicaSets 和 DaemonSets 之间有很多相似之处，但它们之间的关键区别在于 DaemonSets 要求在所有节点上运行单个代理（即 Pod）应用程序。

运行日志容器的其中一种方式是使用 DaemonSet 在每个节点上部署 Pod。Fluentd 是一个开源日志解决方案，广泛用于从系统收集日志。以下示例展示了使用 DaemonSet 部署 Fluentd 的一种方法：

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
name: fluentd-elasticsearch
namespace: kube-system
labels:
  central-log-k8s: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
```

在前面的 DaemonSet 配置中，我们创建了一个 DaemonSet，它将在每个节点上部署 Fluentd 容器。我们还创建了一个容忍，该容忍不会将 Fluentd 节点调度到 master（控制平面）节点上。

