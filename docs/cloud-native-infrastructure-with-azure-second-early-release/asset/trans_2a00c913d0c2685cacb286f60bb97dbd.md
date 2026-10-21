

```
forward . 8.8.8.8
log
errors
cache
}
```

在这个示例中，我们为 cloudnativeazure.io 和根（.）服务器块配置了两个服务器块。两者都监听 53 端口。此外，还定义了多个插件：log，用于记录 CoreDNS 接收到的每个查询；errors，用于记录错误；cache，用于启用前端缓存（即除区域传输和元数据外的所有记录将被缓存，最长可达 3600 秒）。你只需在 Corefile 所在的同一目录下输入 coredns 即可运行前面的 Corefile，或执行 coredns -conf Corefile：

```
$ coredns -conf Corefile
.:53
cloudnativeazure.io.:53
CoreDNS-1.8.1
darwin/amd64, go1.15.7, 95622f4
```

如果在另一个终端中对 cloudnativeazure.io 执行 dig，你会看到以下内容：

```
$ ~ dig @127.0.0.1 cloudnativeazure.io:53
; <<>> DiG 9.10.6 <<>> @127.0.0.1 cloudnativeazure.io:53
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 48995
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags;; udp: 4096
;; QUESTION SECTION:
;cloudnativeazure.io:53.      IN      A

;; AUTHORITY SECTION:
.                       1800    IN      SOA     a.root-servers.net.
nstld.verisign-grs.com. 2021021001 1800 900 604800 86400
```

```
;; Query time: 82 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Wed Feb 10 23:45:28 IST 2021
;; MSG SIZE  rcvd: 126
```

你可以在 CoreDNS 终端中看到如下日志：

```
$ coredns -conf Corefile
.:53
cloudnativeazure.io.:53
CoreDNS-1.8.1
darwin/amd64, go1.15.7, 95622f4

[INFO] 127.0.0.1:59741 - 48995 "A IN cloudnativeazure.io:53. udp 51 false 4096" NXDOMAIN qr,rd,ra,ad 115 0.05160016s
```

我们在这里只介绍了 CoreDNS 的基础知识。如需了解更多信息，建议阅读 John Belamaric 和 Cricket Liu 合著的 *Learning CoreDNS*（O’Reilly，2019）。

现在让我们讨论 CoreDNS 在 Kubernetes 中的服务发现用法。

## Kubernetes 服务发现与 CoreDNS

如前所述，CoreDNS 是 Kubernetes v1.13 及更高版本中的默认服务发现机制，你可以从一开始就使用它。如以下代码块所示，运行在 kube-system 命名空间中的 Pod 类型为 core-dns：

```
$ kubectl get pods --namespace=kube-system -o wide
NAME                          READY   STATUS    RESTARTS   AGE   IP           NODE                                      NOMINATED NODE   READINESS GATES
coredns-748cdb7bf4-9xggk      1/1     Running   0          4h31m 10.244.0.3    aks-agentpool-13363041-vmss000002         <none>
coredns-748cdb7bf4-f6glb      1/1     Running   0          4h30m 10.244.0.5    aks-agentpool-13363041-vmss000002         <none>
coredns-autoscaler-868b684fd4-7ck6z 1/1 Running 0 4h30m 10.244.0.4 aks-agentpool-13363041-vmss000002 <none>
```

```
<none>

$ kubectl get deployments --namespace=kube-system
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
coredns             2/2     2            2           4h31m
coredns-autoscaler  1/1     1            1           4h31m
```

CoreDNS Kubernetes 插件会监视 Kubernetes 服务和端点资源，并缓存该数据。当请求发出时，CoreDNS 插件会检查是否存在与请求名称对应的资源，然后返回适当的数据。响应记录是根据传入请求即时构建的，利用 Kubernetes API 服务器的 watch 功能。

让我们看看 CoreDNS 如何在 Kubernetes 集群中协助服务发现。假设你按如下方式在 Kubernetes 集群中运行一个简单的 Nginx Pod：

```
$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          3m20s
```

```
$ kubectl get services
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
kubernetes    ClusterIP   10.0.0.1        <none>        443/TCP   41m
nginx         ClusterIP   10.0.148.122    <none>        80/TCP    3m26s
```

要访问此 Nginx 服务器，你需要运行一个包含 dig 和 curl 等 DNS 工具的 infoblox/dnstools Pod。要运行此 Pod，可以使用以下命令：

```
$ kubectl run --restart=Never -it --image infoblox/dnstools
dnstools
If you don't see a command prompt, try pressing enter.
dnstools#
```

前面的命令会将你登录到新启动的容器中并提供一个命令提示符。

在 Kubernetes 中，基于 DNS 的服务发现定义了一种规范，或 DNS 模式，定义了用于定位集群中运行的服务的 DNS 名称。Kubernetes 集群中的所有 DNS 记录都属于一个单一域，称为 *集群域*，在大多数 Kubernetes 集群中默认通常设置为 `cluster.local`。DNS 规范定义了为分配给服务的每个 IP（即集群 IP）存在一个 A 记录，该记录包含集群 IP，其名称由服务名称和命名空间派生为 `service.namespace.svc.cluster-domain`。因此，要访问我们创建的 Nginx Pod，你可以从 `infoblox/dnstools` 容器中发出以下命令：

```
dnstools# curl nginx.default.svc.cluster.local
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and working. Further configuration is required.</p>

<p>For online documentation and support please refer to <a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at <a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
```

```
</html>
dnstools#
```

你也可以发出 dig 命令来查看服务的 A 记录：

```
dnstools# dig nginx.default.svc.cluster.local

; <<>> DiG 9.11.3 <<>> nginx.default.svc.cluster.local
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 52488
;; flags: qr aa rd; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags; udp: 4096
; COOKIE: 5086e790e261bf20 (echoed)
;; QUESTION SECTION:
;nginx.default.svc.cluster.local. IN      A

;; ANSWER SECTION:
nginx.default.svc.cluster.local. 5 IN    A    10.0.148.122

;; Query time: 1 msec
;; SERVER: 10.0.0.10#53(10.0.0.10)
;; WHEN: Sat Feb 27 07:37:10 UTC 2021
;; MSG SIZE  rcvd: 119
```

Microsoft Azure 还提供 DNS 即服务，通过名为 Azure DNS 的托管服务。我们接下来将介绍它。

## Azure DNS

Azure DNS 主要提供 Microsoft Azure 基础设施来处理 DNS 域的名称解析。你可以使用 Azure DNS 来托管你的 DNS 域并管理你的 DNS 记录。通过在 Azure 中托管你的域，你可以使用与其它 Azure 服务相同的凭据、API、工具和计费来管理你的 DNS 记录。

Azure DNS 可用于解析公共和私有域中的主机名。公共 DNS 功能可用于通过 Azure 托管你预先购买的域名，而私有 DNS 让你能够在虚拟网络内管理和解析域名。

CoreDNS 还提供了一个 Azure 特定插件，用于从 Azure DNS 提供区域。你可以按如下方式启用 Azure 插件：

```
azure AZURE_RESOURCE_GROUP:ZONE... {
    tenant <TENANT_ID>
    client <CLIENT_ID>
    secret <CLIENT_SECRET>
    subscription <SUBSCRIPTION_ID>
    environment <ENVIRONMENT>
    fallthrough [ZONES...]
    access private
}
```

在前面的代码中：

- AZURE_RESOURCE_GROUP:ZONE 是 Azure 上托管区域所属的资源组，ZONE 是包含数据的区域。
- CLIENT_ID 和 CLIENT_SECRET 是 Azure 的凭据，tenant 指定要使用的 TENANT_ID。SUBSCRIPTION_ID 是订阅 ID。
- environment 指定 Azure ENVIRONMENT。
- fallthrough 指定如果区域匹配但无法生成记录，则请求应传递给下一个插件。如果省略 ZONES，则对插件具有权威性的所有区域都会发生回退。
- access 指定 DNS 区域是公共还是私有。默认值为 public。

我们已经介绍了服务发现在分布式云原生环境中的作用和意义，那么让我们讨论一下服务网格带来的
