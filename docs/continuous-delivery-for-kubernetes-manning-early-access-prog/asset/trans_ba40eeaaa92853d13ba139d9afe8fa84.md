

![](img/2ab98a5ff1ec073700d902ba95287fd6_46_0.png)

Figure 2.X Describing a deployment to see its details

以这种方式描述部署在部署出现问题时将会很有帮助。例如，如果未满足所需副本数，描述该资源将有助于了解问题可能出在哪里。务必始终查看底部的事件，以获取有关资源状态的更多信息，在此案例中，部署在 12 分钟前已扩缩至 1 个副本。

如前所述，部署还负责协调版本或配置的升级和回滚。部署更新策略默认设置为“Rolling Update”，这意味着部署会逐个逐步升级 Pod，以最大限度地减少停机时间。也可以设置另一种策略，称为“Recreate”，它会关闭所有 Pod 并创建新的 Pod。

与 Pod 不同，部署不是临时的；因此，如果你创建了一个 `Deployment`，无论其底层的容器是否失败，你都可以随时查询它。默认情况下，当你创建部署资源时，Kubernetes 会为处理和检查部署请求的副本创建一个中间资源。

## ReplicaSets

拥有容器的多个副本是一项重要的功能，可以让你扩展应用程序。如果你的应用程序正承受来自用户的流量负载，你可以轻松地扩展服务的副本数以处理所有传入请求。同样，如果你的应用程序没有经历大量请求，这些副本可以缩减以节省资源。Kubernetes 创建的对象称为 `ReplicaSet`，可以通过运行以下命令进行查询：

```
kubectl get replicaset
```

输出应如下所示：

| NAME | DESIRED | CURRENT | READY | AGE |
|---|---|---|---|---|
| app-fmtok8s-agenda-rest-7cfc4b5b4c | 1 | 1 | 1 | 3h18m |
| app-fmtok8s-api-gateway-8b4b6d56b | 1 | 1 | 1 | 3h18m |
| app-fmtok8s-c4p-rest-558b8d7bbd | 1 | 1 | 1 | 3h18m |
| app-fmtok8s-email-rest-7795df6885 | 1 | 1 | 1 | 3h18m |

Figure 2.X Listing Deployment's Replica Sets

这些 `ReplicaSet` 对象完全由 Deployment` 资源管理，通常你不需要处理它们。

ReplicaSets 在处理滚动更新时也很重要，你可以在这里找到有关此主题的更多信息：https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/。在后面的章节中，你将使用 Helm 对应用程序进行更新，此时这些机制将被触发。

如果你想更改部署的副本数，你可以再次使用 `kubectl` 来执行：

```
kubectl scale --replicas=2 deployments/<DEPLOYMENT_ID>
```

你可以尝试对 API Gateway 部署执行此操作：

```
kubectl scale --replicas=2 deployments/app-fmtok8s-api-gateway
```

此命令会更改 Kubernetes 中的部署资源，并触发为 API Gateway 部署创建第二个副本。增加面向用户的服务的副本数是很常见的，因为所有用户在访问会议页面时都会访问该服务。

如果你再次在浏览器中访问应用程序，屏幕顶部在 API Gateway 服务版本号旁边，你可以看到 Pod 名称以及 Pod 运行的节点：

Figure 2.X First replica answering your request

如果你现在刷新页面，第二个副本应该会响应你的请求：

Figure 2.X Second replica is now answering; this replica might be on a different node

默认情况下，Kubernetes 会在副本之间进行负载均衡。通过仅更改副本数即可扩展，无需部署任何新内容，Kubernetes 将负责配置新的 Pod（其中包含新的容器）来处理更多流量。Kubernetes 还会确保始终保持所需数量的副本。你可以通过删除一个 Pod 并观察 Kubernetes 如何自动重新创建它来测试这一点。

## 2.2.2 Connecting services together

我们已经了解了部署，它负责启动我们的容器并保持其运行，但到目前为止，这些容器只能在 Kubernetes 集群内部访问。如果我们希望其他服务与这些容器交互，我们需要查看另一个 Kubernetes 资源，称为“Service”。Kubernetes 提供了高级服务发现机制，允许服务仅通过知道彼此的名称即可相互通信。这对于在容器 IP 地址可能随时间变化的情况下连接大量服务至关重要，而无需使用这些 IP 地址。

## EXPLORING SERVICES

要将容器公开给其他服务，你需要使用 `Kubernetes Service` 资源。每个应用程序服务都定义此 `Service` 资源，以便其他服务和客户端可以连接。 在 Kubernetes 中，服务将负责将流量路由到应用程序容器。这些 `Service` 代表一个逻辑名称，你可以使用它来抽象容器的运行位置。如果你有多个容器副本，服务资源将负责在所有副本之间进行负载均衡。

你可以通过运行以下命令列出所有服务：

```
kubectl get services
```

运行命令后，你应该会看到如下图所示的内容：

| NAME | TYPE | CLUSTER-IP | EXTERNAL-IP | PORT(S) | AGE |
|---|---|---|---|---|---|
| fmtok8s-agenda | ClusterIP | 10.96.22.133 | <none> | 80/TCP | 3h22m |
| fmtok8s-api-gateway | ClusterIP | 10.96.2.222 | <none> | 80/TCP | 3h22m |
| fmtok8s-c4p | ClusterIP | 10.96.114.4 | <none> | 80/TCP | 3h22m |
| fmtok8s-email | ClusterIP | 10.96.215.48 | <none> | 80/TCP | 3h22m |
| kubernetes | ClusterIP | 10.96.0.1 | <none> | 443/TCP | 7h20m |

Figure 2.X Listing application's services

你也可以通过以下命令描述服务以查看更多信息：

```
kubectl describe service fmtok8s-api-gateway
```

这应该会给出类似图 2.X 的结果。服务和部署通过选择器属性链接，在以下图像中突出显示。换句话说，服务将流量路由到由包含标签 app=app-fmtok8s-api-gateway 的部署创建的所有 Pod。

![](img/2ab98a5ff1ec073700d902ba95287fd6_49_0.png)

Figure 2.X Describing the API Gateway Service

## SERVICE DISCOVERY IN KUBERNETES

使用服务后，如果你的应用程序服务需要向任何其他服务发送请求，它可以使用 Kubernetes 服务名称和端口，在大多数情况下，如果你使用 HTTP 请求，可以使用端口 80，这样只需要使用服务名称即可。

如果你查看服务的源代码（你可以在这里找到：https://github.com/salaboy/fmtok8s-c4p-rest/blob/main/src/main/java/com/salaboy/conferences/c4p/rest/services/EmailService.java#L14），你会看到 HTTP 请求是针对服务名称创建的，无需 IP 地址或端口。

最后，如果你想将服务公开到 Kubernetes 集群之外，你需要一个 Ingress 资源。正如名称所示，此 Kubernetes 资源负责将集群外部的流量路由到集群内部的服务。通常，你不会公开多个服务，从而限制应用程序的入口点。
你可以通过运行以下命令获取所有可用的 Ingress 资源：

```
kubectl get ingress
```

输出应如下所示：

```
NAME       CLASS    HOSTS   ADDRESS     PORTS   AGE
frontend   <none>   *       localhost   80      4m32s
```

Figure 2.X Listing application's Ingress Resources

然后你可以用与处理其他资源类型相同的方式描述 Ingress 资源，以获取有关它的更多信息：

```
kubectl describe ingress frontend
```

你应该期望输出如下所示：

```
Name:                     frontend
Namespace:                default
Address:                  localhost
Default backend:          default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /     fmtok8s-api-gateway:80 (10.244.1.8:8080)
Annotations:  kubernetes.io/ingress.class: nginx
              meta.helm.sh/release-name: app
              meta.helm.sh/release-namespace: default
              nginx.ingress.kubernetes.io/rewrite-target: /
Events:
  Type    Reason  Age   From                      Message
  ----    ------  ----  ----                      -------
  Normal  Sync    7m58s (x2 over 7m59s)  nginx-ingress-controller  Scheduled for sync
```

Figure 2.X Describing Ingress Resource

正如你所见，Ingress 也使用服务名称来路由流量。此外，要使其正常工作，你需要一个 Ingress Controller，就像我们在创建 KinD 集群时安装的那样。如果你运行在云提供商上，你可能需要安装一个 Ingress 控制器。

