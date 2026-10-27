
The image tag that is being pulled is from docker.io, which is not in the approved registry list. It also maps the host's root filesystem into the container at mount /host and it is starting as a privileged container.

Since the container started successfully, we can document that the cluster can pull images from registries that are not in the accepted list. A successful start also shows that the pod started as a privileged container and that the mount to hostPath also succeeded. To document that the container does have access to the host filesystem, we can exec into the container and list the /host directory. The image below shows that we can successfully list the host's root filesystem.

Fig. 16.16 Accessing the Host filesystem in a container

![](img/fb3bebce7b5f593bca1a7f8a1371be06_625_0.png)

## Evermore Industries Solution - Adding Privileged Constraints

### Meets requirements: 3 and 4

We need to address the security requirements for Evermore’s cluster. To secure the cluster from running privileged pods in the cluster, but not affecting pods in any Anthos system namespaces, we need to enable a constraint with exemptions. However, before enabling a constraint, Evermore has required that all constraints be tested and the output of affected pods be supplied as part of the documentation.

The first step is to create a manifest to create the constraint. The manifest shown below will create a constraint called privileged-containers in auditing mode only. It will also exclude all of the system namespaces that Evermore has supplied in the requirements document.

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sPSPPrivilegedContainer
metadata:
  name: privileged-containers
spec:
  enforcementAction: dryrun
  excludedNamespaces:
  - kube-system
  - gke-system
  - config-management-system
  - gatekeeper-system
  - gke-connect
  match:
    kinds:
    - apiGroups: [""]
      kinds: ["Pod"]
```

To add the audit output to the documentation, you must describe the constraint and direct the output to a file, by executing the kubectl command below:

```bash
kubectl get K8sPSPPrivilegedContainer psp-privileged-container -o yaml > privtest
```

This will create a file called privtest in the current folder, containing the audit results for the psp-privileged-container constraint. You should check the file to verify that it contains the expected audit results under the violations section. Here is an abbreviated output from our audit:

```
violations:
  - enforcementAction: dryrun
    kind: Pod
    message: 'Privileged container is not allowed: cilium-agent,
      {"capabilities": {"add": ["NET_ADMIN", "SYS_MODULE"]}, "pri
    name: anetd-4qbw5
    namespace: kube-system
  - enforcementAction: dryrun
    kind: Pod
    message: 'Privileged container is not allowed: clean-cilium-s
      {"capabilities": {"add": ["NET_ADMIN"]}, "privileged": true
    name: anetd-4qbw5
```

You may have noticed that the audit output contains pods running in namespaces that were added as an exclusion. Remember that when you exclude a namespace in a constraint, the namespace will still be audited - the exclusion only stops the policy from being enforced.

Since the output looks correct, we can enforce the policy to meet the security requirements to deny privileged containers. To remove the existing constraint, delete it using the manifest file using *kubectl delete -f <manifest file>*

Next, update the manifest file and remove the *enforcementAction: dryrun* line from the manifest and redeploy the constraint.

## Evermore Industries Solution - Adding Host Constraints

### Meets requirements: 3 and 5

The 5th requirement from Evermore is to deny hostPID, hostNetwork, and hostIPC in all namespaces, except kube-system. We also need to test the policy before implementation, as stated in the requirements.

To meet the set requirements, we need to implement two new policies. The first, k8spsphostnamespace, will block access to host namespaces including hostPID and hostIPC. Finally, to address blocking hostNetwork, we need to

Just like the previous constraint, we have included the dryrun option to test the constraint before being enforced. Once tested and deployed, any pod that attempts to set hostNetwork to true, will be denied by the admission controller with an error stating that only hostNetwork=false is allowed.

```
Error creating: admission webhook "validation.gatekeeper.sh" deni
```

Congratulations! By deploying the last two constraints, we have met all of Evermore’s requirements.

## 16.10 Summary

In this chapter you were introduced to some basic security concepts like how root and privileged containers can be used to take over an unprotected host. You also learned how Anthos can be used to secure a cluster using policies and how to deploy policies using Anthos Configuration Management, including:

- Discussing the notion that virtual machines provide better security over containers, which has been a long running discussion in the industry. Both patterns have their own unique security concerns, and must be deployed correctly, keeping security in mind from the beginning of their deployments.
- A Kubernetes security overview and features included with Anthos to help remediate security concerns in clusters including ACM and ASM.
- Understanding security issues with containers running as root or as a privileged container and how each can be used to compromise a host.
- Using ACM features to secure a cluster by using Gatekeeper and the included Constraint Template libraries provided by Anthos.
- Understanding Google’s Container Vulnerability Scanning engine to identify container vulnerabilities.

> [1] Kubernetes.io does provide an initial set of recommendations: https://kubernetes.io/docs/concepts/security/pod-security-standards/

[2] http://cve.mitre.org/cve

## 17 Google Cloud Marketplace

by Antonio Gulli

### This chapter covers:

- The Public Google marketplace
- The Private Google marketplace
- Deploying a Marketplace solution
- Real-world scenarios

Google Cloud Marketplace is a one-stop solution to try, buy, configure, manage and deploy software products. Frequently, there are multiple vendors offering the same package, providing an array of options to select from for your specific use case and industry in terms of operating systems, VMs, containers, storage costs, execution environment and SaaS services. Google Cloud offers new users an initial credit that can also be used on the Marketplace. As of Jan 2021, this credit is $300 but it might change in the future. In this chapter, we will discuss how Google Cloud Marketplace can be used to deploy packages automatically in different Kubernetes environments, including Anthos, GKE and GKE on-prem.

### 17.1 The Google Marketplace

The GCP Marketplace website offers a single place for GCP customers to find free and paid applications, either provided by Google or by our third party partners, who extend what we offer in the platform. Deployments can use either default configurations or be specialized in case of specific needs such as increased memory, storage or more computational power with larger vCPUs. Each package has specific instructions for getting assistance after installation. Note that the marketplace team keeps each image updated to fix critical issues and bugs. However, it is your responsibility to update the solutions that are already deployed in your environments.

Kubernetes. All the user has to do is click on the Kubernetes Apps category in the Cloud Marketplace Home page. (https://cloud.google.com/marketplace/browse)

Currently there are about 100 solutions available for GKE in different areas such as Networking, Databases, Analytics, Machine Learning, Monitoring, Storage and more, as shown at the bottom of Figure 17.1.

Solutions are categorized according to the license model: either open source, or Paid or “Bring Your Own License” (BYOL). BYOL is a licensing model that allows enterprises to use their licenses flexibly, whether on-premise, or in the cloud.

Figure 17.2: Solutions available for Anthos GKE environment.
