

- **Docker,** 按照其网站上提供的文档进行安装：https://docs.docker.com/get-docker/
- **Kubernetes KinD**（Kubernetes in Docker），按照其网站上提供的文档在你的笔记本电脑上安装 KinD：https://kind.sigs.k8s.io/docs/user/quick-start/#installation
- `kubectl`，按照官方 Kubernetes 网站上提供的文档安装 `kubectl` https://kubernetes.io/docs/tasks/tools/
- **Helm,** 你可以在其网站上找到安装 Helm 的说明：https://helm.sh/docs/intro/install/

安装完所有内容后，我们就可以开始使用 KIND，它是一个项目，可让你使用 Docker 容器“节点”在本地运行 Kubernetes 集群。

在本节中，你将在笔记本电脑/PC 上创建一个本地 Kubernetes 集群并进行设置，以便你可以访问在其中运行的应用程序。

通过使用 KIND，你可以快速为运行和测试你的应用程序 provision 一个 Kubernetes 集群；因此，当处理由多个服务组成的应用程序时，使用此类工具来作为持续集成管道的一部分运行集成测试是有意义的。

一旦在你的环境中安装了 `kind`，你就可以通过在终端中运行单行命令来创建集群。

你将要创建的集群将被称为 `dev`，并且将有四个节点，三个 worker 节点和一个 master 节点（控制平面），如图 2.2 所示。我们希望能够看到我们的应用程序服务放置在 Kubernetes 集群中的哪些节点上。

![](img/2ab98a5ff1ec073700d902ba95287fd6_30_0.png)

图 2.2 Kubernetes 集群拓扑

KinD 将模拟一个由一组机器或虚拟机构成的真实集群，在本例中，每个节点都将是一个 Docker 容器。当你在这些节点之上部署应用程序时，Kubernetes 将根据集群整体利用率决定应用程序容器的运行位置。Kubernetes 还将处理这些节点的故障，以最大限度地减少你的应用程序停机时间。由于你在本地运行 Kubernetes 集群，因此存在局限性，例如你的笔记本电脑/PC 可用的 CPU 和内存。在真实集群中，这些节点中的每一个都是不同的物理或虚拟机，可以在不同的位置运行以最大化弹性。

你可以通过在终端中运行以下命令来创建集群：

```
cat <<EOF | kind create cluster --name dev --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
- role: worker
- role: worker
- role: worker
EOF
```

你可以从 GitHub 复制之前的命令和以下命令：https://github.com/salaboy/from-monolith-to-k8s/blob/master/kind/README.md

请注意，除了创建集群之外，你还需要设置一个 Ingress Controller（因此控制平面节点中的标签：`node-labels: "ingress-ready=true"` 以及一些端口映射以将流量从你的笔记本电脑路由到集群内运行的服务。

在运行前一个命令后，你应该会看到类似于图 2.3 的内容：

Creating cluster "dev" ...
✓ Ensuring node image (kindest/node:v1.21.1)
✓ Preparing nodes
✓ Writing configuration
✓ Starting control-plane
✓ Installing CNI
✓ Installing StorageClass
✓ Joining worker nodes
Set kubectl context to "kind-dev"
You can now use your cluster with:

kubectl cluster-info --context kind-dev

Have a nice day!

图 2.3 KIND 集群已创建

要将你的 `kubectl` CLI 工具与此新创建的集群连接，你可能需要运行：

```
kubectl cluster-info --context kind-dev
```

你应该会看到类似于以下内容：

Kubernetes control plane is running at https://127.0.0.1:60714
CoreDNS is running at https://127.0.0.1:60714/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

图 2.4 为 `kubectl` 设置上下文

一旦你已连接到集群，你就可以开始与其交互。例如，你可以通过运行以下命令来检查集群节点：

```
kubectl get nodes -owide
```

运行该命令的输出应类似于：

| NAME | STATUS | ROLES | AGE | VERSION | INTERNAL-IP | EXTERNAL-IP | OS-IMAGE | KERNEL-VERSION | CONTAINER-RUNTIME |
| dev-control-plane | Ready | control-plane,master | 6m37s | v1.21.1 | 172.18.0.6 | <none> | Ubuntu 21.04 | 5.10.25-linuxkit | containerd://1.5.2 |
| dev-worker | Ready | <none> | 6m7s | v1.21.1 | 172.18.0.2 | <none> | Ubuntu 21.04 | 5.10.25-linuxkit | containerd://1.5.2 |
| dev-worker2 | Ready | <none> | 6m7s | v1.21.1 | 172.18.0.5 | <none> | Ubuntu 21.04 | 5.10.25-linuxkit | containerd://1.5.2 |
| dev-worker3 | Ready | <none> | 6m7s | v1.21.1 | 172.18.0.4 | <none> | Ubuntu 21.04 | 5.10.25-linuxkit | containerd://1.5.2 |

![](img/2ab98a5ff1ec073700d902ba95287fd6_33_0.png)

图 2.5 列出所有 Kubernetes 节点

正如你所见，你的 Kubernetes 集群由四个节点组成，其中一个是控制平面。请注意，你正在使用 `-owide` 标志来获取有关节点的更多信息。

最后，你将使用 NGINX Ingress Controller（更详细的说明可以在这里找到：(https://kind.sigs.k8s.io/docs/user/ingress/)）将流量从 Kubernetes 集群外部路由到在集群内部运行的应用程序。有许多 Ingress Controller 实现可以安装来进行此路由，但 NGINX Ingress Controller 被广泛采用，是最受欢迎的选择。有关可用选项的非详尽列表，你可以查看 Kubernetes 网站：https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/。要安装 NGINX Ingress Controller，你需要运行以下命令：

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml
```

此命令在名为 `ingress-nginx` 的新 Kubernetes 命名空间中创建运行 NGINX Ingress Controller 所需的一组资源：

```
namespace/ingress-nginx created
serviceaccount/ingress-nginx created
configmap/ingress-nginx-controller created
clusterrole.rbac.authorization.k8s.io/ingress-nginx created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created
role.rbac.authorization.k8s.io/ingress-nginx created
rolebinding.rbac.authorization.k8s.io/ingress-nginx created
service/ingress-nginx-controller-admission created
service/ingress-nginx-controller created
deployment.apps/ingress-nginx-controller created
ingressclass.networking.k8s.io/ingress created
validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
serviceaccount/ingress-nginx-admission created
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
role.rbac.authorization.k8s.io/ingress-nginx-admission created
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created
job.batch/ingress-nginx-admission-create created
job.batch/ingress-nginx-admission-patch created
```

图 2.6 安装 NGINX Ingress Controller

顺便提一下，你可以通过运行以下命令来检查此 Ingress Controller 在你的集群中运行的位置：

```
kubectl get pods -n ingress-nginx -owide
```

此命令的输出应如下所示：

```
NAME                                    READY   STATUS      RESTARTS   AGE     IP           NODE             NOMINATED NODE   READINESS GATES
ingress-nginx-admission-create-c8ptk    0/1     Completed   0          8m1s    10.244.2.2   dev-worker        <none>           <none>
ingress-nginx-admission-patch-nxsd5    0/1     Completed   0          8m1s    10.244.3.2   dev-worker2       <none>           <none>
ingress-nginx-controller-c9459f755-6k2r5 1/1   Running     0          8m2s    10.244.0.5   dev-control-plane <none>           <none>
```

![](img/2ab98a5ff1ec073700d902ba95287fd6_34_0.png)

图 2.7 Ingress Controller 运行在 `control-plane` 节点上

这里可以看到 Ingress Controller 运行在控制平面节点上。
好了；你的集群已启动并运行，你的 `kubectl` 命令行界面已配置为针对你的新集群工作！现在你可以在新创建的集群中安装应用程序了。

