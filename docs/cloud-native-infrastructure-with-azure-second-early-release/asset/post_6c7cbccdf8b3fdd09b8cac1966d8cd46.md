
```
$ kubectl create -n istio-system secret tls telemetry-gw-cert --key=${CERT_DIR}/tls.key --cert=${CERT_DIR}/tls.crt
```

At this point, we can expose Kiali using the following configurations, which will create a gateway, a virtual service, and a destination rule:

- `Gateway` represents a data plane that routes traffic into a Kubernetes cluster by configuring a load balancer for HTTP/TCP traffic, regardless of where it will be running.
- `VirtualService` defines a set of traffic routing rules to apply when a host is addressed. A routing rule can match a certain type of traffic, and when a match for a rule occurs, the traffic is sent to a named destination service.
- `DestinationRule` configures the set of policies to be applied while forwarding traffic to a service.

Here is the code:

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

Now you can access the Kiali UI at https://kiali.${INGRESS_DOMAIN} (in our case, it is https://kiali.40.118.246.227.nip.io). When you go to this URL you will be presented with the Kiali login screen, where you need to enter the token for the Kiali operator. You can get the token by doing the following:

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

You can now copy the token and log in with it. You also need to install Prometheus with Istio to record metrics that track the health of Istio and of applications within the service mesh (refer back to Chapter 6 if you need a refresher on how to do that). Use the following manifest to install and configure Prometheus:

```
$ kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.9/samples/addons/prometheus.yaml
```

Figure 7-4 shows the resultant Kiali dashboard. You’ll notice that the Istio ingress gateway sends traffic to Kiali along with other deployments in the namespace istio-system. The dashboard presents a variety of Istio configurations and features which can be used directly from the UI. For example, if you check the default namespace in the Overview tab, you can see it already has the sidecar proxy auto-injection-enabled for all the deployments in this namespace (see Figure 7-5). You can use this feature from the UI to enable or disable automatic injection of the sidecar proxy.

![](img/8d47d1b5dbca55e784ae602145a894c7_259_0.png)

Figure 7-4. Kiali dashboard showcasing simple inbound traffic

![](img/8d47d1b5dbca55e784ae602145a894c7_260_0.png)

Figure 7-5. Kiali dashboard showing the status of the auto-injection sidecar proxy in the default namespace

## Exploring the Kiali dashboard

In this section, we’ll explain how you can use the Istio features right from the Kiali dashboard. For this demonstration, we’re using an example of a small travel application service. Though we will not focus much on the application, we will highlight Kiali’s features and how efficiently it provides every detail about your cloud native application. We will also provide a high-level overview of the business logic of the demo application. If you want to learn more, please refer to the official Kiali documentation for the travel demo.⁴

This application simulates two business domains organized in different namespaces. The first namespace is called travel-portal and it has several travel shops, where users can search for and book flights, hotels, cars, and travel insurance. The shop applications can behave differently based on request characteristics such as channel (web or mobile) and user (new or existing). All the portals consume a service called travels that is deployed in the travel-agency namespace.

The second namespace is called travel-agency and it hosts a set of services created to provide quotes for travel. A main travel service will be the business entry point for the travel agency. It receives a destination city and a user as parameters and it calculates all the elements that compose a travel budget: airfare, lodging, car reservation, and travel insurance.

Each service can provide an independent quote and the travel service must then aggregate them into a single response.

There is a third namespace as well, called `travel-control`, which runs the main business dashboard with different functions.

We have deployed these three namespaces in the services as follows:

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

The first thing you need to do after you have deployed the travel demo application is enable sidecar proxy support. Start by enabling auto injection for all the newly created namespaces by clicking on the three dots in the highlighted box in Figure 7-6.

![](img/8d47d1b5dbca55e784ae602145a894c7_261_0.png)

Figure 7-6. Enabling auto injection for the `travel-portal` namespace
