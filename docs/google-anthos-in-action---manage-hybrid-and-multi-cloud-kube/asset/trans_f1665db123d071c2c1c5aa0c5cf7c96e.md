

```yaml
labels:
  app: guestbook
name: frontend
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: guestbook
type: LoadBalancer
loadBalancerIP: [SERVICE VIP]    #A
```

除了 Service VIP 之外，负载均衡器还需要一个用于 Kubernetes API 服务器的控制平面 VIP。最后，每个 Anthos on Prem 集群中都运行着一个 ingress 控制器。ingress 控制器服务也具有一个名为 Ingress VIP 的 VIP。通过 ingress 暴露的服务使用 Ingress VIP 来访问 Kubernetes 服务。

Anthos on Prem 高级负载均衡架构如下所示。

## Figure 12.10 Anthos on prem: Load balancer network architecture

![](img/fb3bebce7b5f593bca1a7f8a1371be06_398_0.png)

下表总结了在各种模式下为负载均衡做准备时必须执行的操作。

|  | Integrated / Bundled mode | Manual mode |
|---|---|---|
| Choose VIPs before you create your clusters. | Yes | Yes |
| Choose node IP addresses before you create your clusters. | No, if using DHCP. Yes, if using static IP | Yes |
| Choose nodePort values before you create your clusters. | No | Yes |
| Manually configure your load balancer | No | Yes |

## Ingress

Anthos on Prem 包含一个 L7 负载均衡器，具有基于 Envoy 的 ingress 控制器，用于处理集群内部署的 ClusterIP 服务的 Ingress 对象规则。ingress 控制器本身在集群中作为 NodePort 服务公开。ingress NodePort 服务可以通过 L3/L4 F5 负载均衡器访问。安装会在负载均衡器上配置一个虚拟 IP 地址（Ingress VIP）（端口 80 和 443）。VIP 指向 Ingress 控制器 NodePort 服务中的端口。这就是外部客户端如何访问集群中服务的方式。

为了通过 Ingress 暴露服务，您必须创建一条 DNS A 记录，将 DNS 名称指向 Ingress VIP。然后您可以为该服务创建 Service 和 Ingress 资源。例如，假设您想暴露一个示例 guestbook 应用的前端服务。

首先，为 guestbook 应用创建指向 Ingress VIP 的 DNS A 记录。

```
*.guestbook.com    A    [INGRESS_VIP]
```

接下来，为 frontend Deployment 创建一个 Service。请注意，Service 的类型为 ClusterIP。

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: guestbook
name: frontend
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: guestbook
type: ClusterIP        #A
```

最后，创建 Ingress 规则。

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: frontend
  labels:
    app: guestbook
spec:
  rules:
    - host: www.guestbook.com
      http:
        paths:
          - backend:
              serviceName: frontend    #A
              servicePort: 80      #A
```

## Anthos on Bare Metal

Anthos on bare metal 是 GCP 支持的 Anthos GKE 实现，部署在裸金属服务器上。Anthos on bare metal 消除了对虚拟化层或 hypervisor 的需求。所有集群节点和 API 服务器都直接在裸金属服务器上运行

Anthos on Bare Metal 部署模型和网络架构在专门介绍它的章节中有详细描述。

## Anthos on AWS

Anthos 集群在 AWS 上（GKE on AWS）是一种混合云软件，将 Google Kubernetes Engine（GKE）扩展到 Amazon Web Services（AWS）。Anthos on AWS 使用 AWS 资源，例如 [Elastic Compute Cloud (EC2)](https://aws.amazon.com/ec2/)、

[Elastic Block Storage (EBS)](#)，和 [Elastic Load Balancer (ELB)](#)。

Anthos 集群在 AWS 上有两个组件。

- 1. **Management service** - 可以安装和更新您的用户集群的环境，使用 AWS API 来配置资源。
- 2. **User clusters** - 您运行容器化应用程序的 Anthos on AWS 集群。

## Networking requirements

管理服务和用户集群都部署在 AWS VPC 内的 EC2 实例中。您可以在 [dedicated AWS VPC](#) 或 [existing AWS VPC](#) 中创建管理服务。您需要在运行 Anthos on AWS 用户集群的每个 AWS 虚拟私有云（VPC）中拥有一个管理服务。管理服务安装在一个 AWS 可用区中。每个 VPC 只需要一个管理服务；一个管理服务可以管理多个用户集群。

用户集群由两个组件组成：控制平面或 Kubernetes API 服务器，以及运行应用程序的节点池。管理服务的主要组件是 Cluster Operator。Cluster Operator 是一个 [Kubernetes Operator](#)，它创建和管理您的 AWSClusters 和 AWSNodePools。Cluster Operator 将配置存储在 etcd 数据库中，存储持久化在 AWS EBS 卷上。

AWSClusters 资源创建和管理用户集群的控制平面，AWSNodePools 资源创建和管理用户集群的节点池。

当您将管理集群安装到 [Dedicated VPC](#) 时，Anthos on AWS 会在 dedicatedVPC.availabilityZones 中指定的每个区域创建控制平面副本。当您将管理集群安装到 [existing infrastructure](#) 时，Anthos on AWS 会创建具有三个控制平面副本的 AWSCluster，每个副本都在相同的可用区中。每个副本都属于自己的 [AWS Auto Scaling group](#)，在实例终止时重启实例。管理服务将控制平面放置在 AWS Network Load Balancer（NLB）后面的私有子网中。管理服务使用 NLB 与控制平面交互。

每个控制平面都存储在本地 etcd 数据库中的配置。这些数据库被复制并设置在 [stacked high availability topology](stacked high availability topology) 中。

一个控制平面管理一个或多个 AWSNodePools。

## Figure 12.11 Anthos on AWS architecture

![](img/fb3bebce7b5f593bca1a7f8a1371be06_402_0.png)

在专用 VPC 中创建 Anthos on AWS 集群时，需要以下 VPC 资源。

- 1. **VPC CIDR Range** - anthos-gke 创建的 AWS VPC 的 IP 地址总 [CIDR](https://aws.amazon.com/what-is/cidr/) 范围。例如，10.0.0.0/16。
- 2. **Availability Zones** - 您想要创建节点和控制平面的 AWS EC2 [availability zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)。
- 3. **Private CIDR Blocks** - 私有子网的 CIDR 块。Anthos on AWS 组件例如管理服务在私有子网中运行。该子网必须在 `vpcCIDRBlock` 中指定的 VPC CIDR 范围内。您需要为每个可用区一个子网。
- 4. **Public CIDR Block** - 公共子网的 CIDR 块。您需要为每个可用区一个子网。公共子网通过 AWS [network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html) 和 [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) 中指定的安全组和地址范围，将集群服务例如负载均衡器暴露给外部。
- 5. **SSH CIDR Block** - 允许对您的 [bastion host](https://cloud.google.com/anthos/clusters/docs/aws/how-to/bastion-host) 进行入站 SSH 的 CIDR 块。您可以使用 IP 范围，例如 203.0.113.0/24。如果您想允许从任何 IP 地址进行 SSH，请使用 0.0.0.0/0。当您使用默认设置创建管理服务时，控制平面具有私有 IP 地址。该 IP 地址无法从 AWS VPC 外部访问。您可以使用 [bastion](https://cloud.google.com/anthos/clusters/docs/aws/how-to/bastion-host) 主机或通过 VPN 或 [AWS Direct Connect](https://aws.amazon.com/directconnect/) 等其他连接访问 AWS VPC 来访问管理服务。

在现有 VPC 中创建 Anthos on AWS 集群时，需要以下 VPC 资源。

- 1. 至少一个公共子网。
- 2. 至少一个私有子网。
- 3. 一个具有通往公共子网路由的互联网网关。
- 4. 一个具有通往私有子网路由的 NAT 网关。
- 5. [DNS hostnames](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html) 已启用。
- 6. DHCP 选项集中 `domain-name` 的自定义值。Anthos on AWS 不支持除 [default EC2 domain names](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-hostnames) 之外的其他值。
- 7. 选择或创建一个 AWS [security group](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)，允许从您将管理 Anthos 集群 on AWS 安装的安全组或 IP 范围入站 SSH（端口 22）。

## Anthos on AWS cluster IP allocation

