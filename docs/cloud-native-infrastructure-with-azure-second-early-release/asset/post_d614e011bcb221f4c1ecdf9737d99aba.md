
Now that we have our network configured, we are ready to apply some security policy. The Calico network policy has a larger set of capabilities than Kubernetes, including:

- Policy ordering
- Deny rules
- Greater flexibility on match rules

Calico network policy supports securing applications using OSI Layers 5 through 7 criteria, as well as cryptographic identity. You will find examples on the Calico network policy configuration page.

Network policy can be enforced in a number of ways:

- Globally (applies to all Pods in all namespaces), known as GlobalNetworkPolicy
- Per network or host, known as HostEndpoint
- Per namespace, known as NetworkPolicy

## GlobalNetworkPolicy: Allowing ICMP traffic

The first policy we will create is a global network policy that allows ICMP traffic for ping and traceroute from all hosts within the cluster. Create the file global-policy.yaml as follows:

```
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: allow-ping-in-cluster
spec:
  selector: all()
  types:
  - Ingress
  ingress:
  - action: Allow
    protocol: ICMP
    source:
      selector: all()
    icmp:
```

```
type: 8 # Ping request
- action: Allow
  protocol: ICMPv6
  source:
    selector: all()
  icmp:
    type: 128 # Ping request
```

Apply the policy by running:

```
$ calicoctl create global-policy.yaml
```

## NetworkPolicy: Allowing traffic within a namespace between two labels

In this example, we will allow traffic from the production namespace with selector green to a Pod in the same namespace with selector blue on port TCP/1234. Create a new file called network-policy.yaml:

```
apiVersion: projectcalico.org/v3
kind: NetworkPolicy
metadata:
  name: allow-tcp-1234
  namespace: production
spec:
  selector: color == 'blue'
  ingress:
  - action: Allow
    protocol: TCP
    source:
      selector: color == 'green'
    destination:
      ports:
      - 1234
```

Apply the policy by running:

```
$ calicoctl create network-policy.yaml
```

Once this policy is applied, traffic will be able to flow to port TCP 1234 to any green Pod.

## Cilium

One of the most innovative networking tools to be introduced over the past five years is the Cilium project, which heavily utilizes eBPF to provide a suite of networking, observability, and security features that are oriented toward cloud native infrastructure.

Cilium utilizes the eBPF Linux kernel technology that enables low-overhead, high-powered visibility, and control logic for the kernel and the applications running on top of it. The use of eBPF makes Cilium a popular choice for infrastructure architects, as the platform provides features around three key pillars:

### Networking

Cilium implements its own CNI which utilizes Linux eBPF. This makes Cilium highly performant and allows it to perform load balancing instead of relying on kube-proxy. It also allows for multicluster connectivity, which is important for larger installations.

### Observability

Cilium’s use of eBPF makes it a unique piece of observability software. Cilium can perform deep-packet inspection on all layers of network connectivity and data.

### Security

One of the best features of Cilium is that it can perform transparent encryption between hosts via IPSec. This means you get end-to-end encryption via a highly efficient mechanism without having to make changes to your application. While Cilium provides its own label+CIDR matching security policy, it also does (OSI) Layer 7 filtering (e.g., filter outbound DNS queries or filter incoming HTTP requests).

Finally, Cilium allows you to perform Layer 7 introspection for policy-making decisions (see this blog post for more information).

Cilium’s use of eBPF as its core enables the software to deeply introspect and manipulate (where needed) all network traffic without affecting system performance.

The Cilium agent (cilium-agent) runs on every node (see Figure 8-5) and accepts the configuration via the Kubernetes API. The agent will turn that configuration into an eBPF program that runs against the container’s network interface, the host’s network, or one of the host’s network cards.

## Orchestration systems

### Cilium layer

Cilium CLI

Policy repository

Plug-ins

Cilium monitor

Cilium daemon
Code generation

Bytecode injection

BPF program

BPF program

BPF program

BPF program

eth0

Container

Container

Container

### Kernel

Figure 8-5. The Cilium architecture

## Deploying Cilium

Cilium can be installed on a self-managed Kubernetes cluster or on a managed AKS cluster. In both cases, Cilium can be deployed via Helm, which makes installation easy. Furthermore, Cilium provides a container to perform connectivity checks to verify that network traffic can flow freely between Pods on your cluster.

For more information, see the specialized documentation for deploying Cilium on Azure.

### Self-managed Cilium installation

We will install Cilium using its quick-install file:

```
$ kubectl apply -f
https://raw.githubusercontent.com/cilium/cilium/v1.9/install/kubernetes/quick-install.yaml
```

The following code will install the connectivity check between hosts:

```
$ kubectl apply -f
https://raw.githubusercontent.com/cilium/cilium/v1.9/examples/kubernetes/connectivity-check/connectivity-check.yaml
```

This will deploy a series of Pods which will use various network paths to connect to one another in order to verify connectivity. Connectivity paths include with and without service load balancing and various network policy combinations.

### AKS Cilium installation

To set up Cilium on AKS, we will use a Helm chart.

- First we will create a new AKS cluster:

```
$ export RESOURCE_GROUP_NAME=aks-test
$ export CLUSTER_NAME=aks-test
$ export LOCATION=westus
```

```
$ az group create --name $RESOURCE_GROUP_NAME --location $LOCATION
$ az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --location $LOCATION \
    --node-count 2 \
    --network-plugin azure
```

- Now we will create a service principal to interact with the Azure APIs and populate environment variables to pass to Helm:

```
$ az ad sp create-for-rbac --name cilium-operator > azure-sp.json

$ AZURE_SUBSCRIPTION_ID="$(az account show | jq -r .id)"
$ AZURE_CLIENT_ID="$(jq -r .appId < azure-sp.json)"
$ AZURE_CLIENT_SECRET="$(jq -r .password < azure-sp.json)"
$ AZURE_TENANT_ID="$(jq -r .tenant < azure-sp.json)"
$ AZURE_NODE_RESOURCE_GROUP="$(az aks show --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME | jq -r .nodeResourceGroup)"
```

- We will install the Cilium repository locally and then install Cilium on our Kubernetes cluster:

```
$ helm repo add cilium https://helm.cilium.io/
$ helm install cilium cilium/cilium --version 1.9.9 \
    --namespace kube-system \
    --set azure.enabled=true \
    --set azure.resourceGroup=$AZURE_NODE_RESOURCE_GROUP \
    --set azure.subscriptionID=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantID=$AZURE_TENANT_ID \
```

```
--set azure.clientID=$AZURE_CLIENT_ID \
--set azure.clientSecret=$AZURE_CLIENT_SECRET \
--set tunnelDisabled \
--set ipam.mode=azure \
--set masquerade=false \
--set nodeinit.enabled=true
--set kubeProxyReplacement=strict \
--set hostFirewall=true \
--set loadBalancer.algorithm=maglev \
--set k8sServiceHost=REPLACE_WITH_API_SERVER_IP \
--set k8sServicePort=REPLACE_WITH_API_SERVER_PORT
```

- We can verify the installation by checking that the four initial Pods have been created and that both a `cilium` and a `cilium-operator` Pod are running:

```
$ kubectl -n kube-system get pods --watch
cilium-operator-ad4375ds5-2x2q3        1/1     Running
0               4m18s
cilium-s5x8xk                    1/1     Running
0               4m19s
```

## Installing Hubble

You will also want to install Hubble, a UI that allows you to see a service map which depicts dependencies between services and statistics on network usage and performance. Hubble’s live-updating, graph-like interface (see Figure 8-6) allows you to present a lot of information about the layout and flow of data between containers. Furthermore, it provides an interface to model Cilium policy.
