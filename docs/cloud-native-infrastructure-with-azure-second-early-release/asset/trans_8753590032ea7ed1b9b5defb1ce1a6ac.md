

```
$ kubectl create -n istio-system secret tls telemetry-gw-cert --key=${CERT_DIR}/tls.key --cert=${CERT_DIR}/tls.crt
```

此时，我们可以使用以下配置公开 Kiali，这些配置将创建一个网关、一个虚拟服务和一个目标规则：

- `Gateway` 表示一个数据平面，通过为 HTTP/TCP 流量配置负载均衡器将流量路由到 Kubernetes 集群，而不管它将在哪里运行。
- `VirtualService` 定义了一组在访问主机时要应用的流量路由规则。路由规则可以匹配特定类型的流量，当规则匹配时，流量将被发送到命名的目标服务。
- `DestinationRule` 配置在将流量转发到服务时要应用的一组策略。

以下是代码：

```
$ cat <<EOF | kubectl apply -f -
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: kiali-gateway
  namespace: istio-system
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 443
      name: https-kiali
      protocol: HTTPS
    tls:
      mode: SIMPLE
      credentialName: telemetry-gw-cert
    hosts:
    - "kiali.${INGRESS_DOMAIN}"
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
name: kiali-vs
namespace: istio-system
spec:
  hosts:
  - "kiali.${INGRESS_DOMAIN}"
  gateways:
  - kiali-gateway
  http:
  - route:
    - destination:
        host: kiali
        port:
          number: 20001
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: kiali
  namespace: istio-system
spec:
  host: kiali
  trafficPolicy:
    tls:
      mode: DISABLE
---
EOF
```

现在你可以在 https://kiali.${INGRESS_DOMAIN} 访问 Kiali UI（在我们的例子中是 https://kiali.40.118.246.227.nip.io）。当你访问此 URL 时，你会看到 Kiali 登录屏幕，需要输入 Kiali 操作员的令牌。你可以通过以下方式获取令牌：

```
$kubectl get secrets --namespace=kiali-operator -o wide
NAME                                 TYPE                                  DATA   AGE
default-token-7ng81                  kubernetes.io/service-account-token   3      142m
kiali-operator-token-jj88t           kubernetes.io/service-account-token   3      141m
sh.helm.release.v1.kiali-operator.v1 helm.sh/release.v1                    1      141m

$ ~ kubectl describe secret kiali-operator-token-jj88t --
```

namespace=kiali-operator
Name:               kiali-operator-token-jj88t
Namespace:          kiali-operator
Labels:             <none>
Annotations:        kubernetes.io/service-account.name: kiali-operator
                    kubernetes.io/service-account.uid: 9a30ba7a-7d21-
484d-9e7a-0a38458690a4

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:          1765 bytes
namespace:       14 bytes
token:
eyJhbGciOiJSUzI1NiIsImtpZCI6InVvQXNpV01QRG1fRjhmal0RRZjVIWXBNVHRnZ
DhJdHhoOXI2RmQ2eGx
DZDQifQ.eyJpc3MiOiJrdWJlcms1ldGVzL3N1cnZpY2VhY2NvdW50Iiwia3ViZXJuZ
XRlcys5pby9zZXJ2aWN
1YWNjb3VudC9uYW1lc3BhY2UiOiJraWFsaS1vcGVyYXrvciIsImt1YmVybV0ZXMu
aW8vc2VydmljZWfJY2
91bnQvc2VjcmV0Lm5hbWUiOiJraWFsaS1vcGVyYXrvci10b2tlbi1qajg4dCIsImt
1YmVybV0ZXMu
aW8vc2VydmljZWfJY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJraWFsaS1vcGVyYX
RvciIsImt1YmVybV0
ZXMu
aW8vc2VydmljZWfJY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjlhMzBiY
TdhLTdkMjEtNDg0ZC0
5ZTdhLTBhMzq0NTg2OTBhNCIsInN1YiI6InN5c3RlbTpzZXJ2aWN1YWNjb3VudDpr
aWFsaS1vcGVyYXrvcj
praWFsaS1vcGVyYXrvciJ9.uJ6056HMQGvc0OQIK1OOMj1PCSrWMpYZL0jKiG27EB
urhUcZ73ngr1S0Ifzc
NnRwJLuwUkMPrzzKOzVvrUUMf6biPb0MieWn001HX6mfHOzNgXRAxdP-
0ApehmEiCyENQS4DEQL2Eg8mUuaFsExuwzpx8LAibdLRpbI7sQa4P5B7he2Huol5F
zJsoySjaKcP6eJGOaI
bscIz-qtDcoM311EXhZr6xx8G7b5O7VEYuzs-
LNKNyqCJL_iDyxV73WGgaPA2KHUjM-ESpBF-
qkoWZDMY6oLqbe24Kcv-
Mzmji_WlTMc28mTttjiMgVP1lUeQfFTK4wYNvpnFEQCJ6ogYC4JJoRLSUuPs1SC2J
X_Bi40aiGus1BH3JtF
QsxxmL3f7ZnPa-
XY66Zk_ZyKZF1rqLKnzXCa85KkZZnCA83kpKT4ksM9MnqTBGBLNMW7OV4gTDT19zU
vQm-
-VgaoN2lnHZ-ogHPzeNym769OYHzSAA_C_g1un1gtc8R1kUDM8u4F-
DUSDGnJTBqjUQLiwXDyJS_epCpLuuA-
6xCtx9DLkWTcGy886SPXlu3OLVeZUfRRlumjteEdId8Z884Iqx1T4V8Qo6AomdI0m
LjbyFbpKoDLTfaTA98
SuaEbko0oF5dzxo2xBgYXy-bliE4K09r_PnBZ-BLVQ0zfO_1Bjym9JYW0

你现在可以复制令牌并使用它登录。你还需要安装 Prometheus 与 Istio 配合使用，以记录跟踪 Istio 健康状况以及服务网格中应用程序的指标（如果需要复习如何操作，请参考第 6 章）。使用以下清单安装和配置 Prometheus：

```
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.9/samples/addons/prometheus.yaml
```

图 7-4 显示了生成的 Kiali 仪表板。你会注意到 Istio 入口网关将流量发送到 Kiali 以及 istio-system 命名空间中的其他部署。仪表板展示了多种 Istio 配置和功能，可以直接从 UI 使用。例如，如果你检查概览选项卡中的默认命名空间，你可以看到它已经为该命名空间中的所有部署启用了 sidecar 代理自动注入（见图 7-5）。你可以使用此功能从 UI 启用或禁用 sidecar 代理的自动注入。

![](img/8d47d1b5dbca55e784ae602145a894c7_259_0.png)

图 7-4. Kiali 仪表板展示简单的入站流量

![](img/8d47d1b5dbca55e784ae602145a894c7_260_0.png)

图 7-5. Kiali 仪表板显示默认命名空间中自动注入 sidecar 代理的状态

## 探索 Kiali 仪表板

在本节中，我们将解释如何直接从 Kiali 仪表板使用 Istio 功能。对于本次演示，我们使用一个小型旅行应用程序服务的示例。虽然我们不会过多关注应用程序，但我们将重点介绍 Kiali 的功能以及它如何高效地提供有关你的云原生应用程序的每个细节。我们还将提供演示应用程序业务逻辑的高级概述。如果你想了解更多，请参考旅行演示的官方 Kiali 文档。⁴

该应用程序模拟了在不同命名空间中组织的两个业务域。第一个命名空间称为 travel-portal，它有多个旅行商店，用户可以在其中搜索和预订航班、酒店、汽车和旅行保险。商店应用程序可以根据请求特征（如渠道（网页或移动）和用户（新用户或现有用户））表现出不同的行为。所有门户都使用部署在 travel-agency 命名空间中的名为 travels 的服务。

第二个命名空间称为 travel-agency，它托管了一组用于提供旅行报价的服务。主要的旅行服务将是旅行机构的业务入口点。它接收目的地城市和用户作为参数，并计算构成旅行预算的所有元素：机票、住宿、汽车预订和旅行保险。

每个服务都可以提供独立的报价，旅行服务必须将其聚合为单个响应。

还有第三个命名空间，称为 `travel-control`，它运行具有不同功能的主业务仪表板。

我们按如下方式在服务中部署了这三个命名空间：

```
$ kubectl create namespace travel-agency
$ kubectl create namespace travel-portal
$ kubectl create namespace travel-control
```

```
$ kubectl apply -f <(curl -L https://raw.githubusercontent.com/kiali/demos/master/travels/travel_agency.yaml) -n travel-agency
$ kubectl apply -f <(curl -L https://raw.githubusercontent.com/kiali/demos/master/travels/travel_portal.yaml) -n travel-portal
$ kubectl apply -f <(curl -L https://raw.githubusercontent.com/kiali/demos/master/travels/travel_control.yaml) -n travel-control
```

部署旅行演示应用程序后，你需要做的第一件事是启用 sidecar 代理支持。首先通过点击图 7-6 中突出显示的框中的三个点，为所有新创建的命名空间启用自动注入。

![](img/8d47d1b5dbca55e784ae602145a894c7_261_0.png)

图 7-6. 为 `travel-portal` 命名空间启用自动注入

