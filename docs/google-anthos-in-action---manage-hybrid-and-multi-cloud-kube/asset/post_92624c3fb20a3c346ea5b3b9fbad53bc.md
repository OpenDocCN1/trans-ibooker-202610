
```
name: anetd-4nldm
namespace: kube-system
- enforcementAction: dryrun
  kind: Pod
  message: 'Privileged container is not allowed: kube-proxy, se true}'
  name: kube-proxy-4z8qh
  namespace: kube-system
```

The output shows that if we were to enforce this policy, the kube-proxy and Cilium pods would be denied startup. This presents a catch-22, we want to deny privileged containers from running, but we need to have privileged containers to allow the system container to run. It is not uncommon for certain containers to go against a policy that would deny the container starting. Since this is a common scenario, the policy engine allows you to exempt namespaces from being evaluated from either all policies or only certain policies.

## Creating a Namespace Exemption

Once a constraint is created, it will affect every container that is started, without any regard for the type of pod that is being started. Many common system containers like networking or logging agents require privileges that may be denied by a cluster policy. Since policies secure a cluster, they are enforced at a cluster, across all namespaces. This may work for certain policies, but others may block a legitimate container from being scheduled. To allow exceptions, the admission controller includes controls that allow namespaces to be exempt from either all policies or just select policies.

### Exempting a Namespace from Specific Processing

To exempt namespaces from all Gatekeeper processes, you can create a config object that contains the namespace(s) that you want to exempt. You can exempt each namespace from all Gatekeeper functions or only certain processes, like auditing, by adding one or more processing options. The table below shows the four processing options that can be set:

**Table 16.2 Namespace exemption options**

| Process Option | Results of Exception |
|---|---|
| audit | Namespace(s) will not report audit results, but will still be part of the webhook and sync processes. |
| webhook | Namespace(s) will be exempt from the admission controller, but will still be part of the audit and sync processes. |
| sync | Namespace(s) resources will not be reported into Gatekeeper, but will still be part of the audit and webhook processes. |
| * | Exempts the namespace(s) from all Gatekeeper processes. |

For example, we may want to exempt a few namespaces from all Gatekeeper processing. To exempt all processes, we can create a new Config that contains the namespaces, with a * in the processes field. The manifest shown below will create a config that exempts the kube-system and gatekeeper-system namespaces from all Gatekeeper processes.

```
apiVersion: config.gatekeeper.sh/v1alpha1
kind: Config
metadata:
  name: config
  namespace: "gatekeeper-system"
spec:
  match:
    - excludedNamespaces: ["kube-system", "gatekeeper-system"]
      processes: ["*"]
```

You can add different process exemptions for different namespaces by adding additional matches. For example, you can create a match that only exempts a namespace from the webhook process, while another namespace may only be exempt from Gatekeeper's auditing process.

### Exempting a Namespace from All Policies

In some organizations, creating an exception for all Gatekeeper policies might violate a security standard. Creating a namespace exemption is quick and easy, but it will exempt every deployment in that namespace from all Gatekeeper policies, without exception.

To exclude a namespace from all policies is a two-step process. The first step is to add a list of namespaces that the policy engine will allow to ignore policies, and the second is to label the namespace(s) that you want to exempt with `admission.gatekeeper.sh/ignore=true`.

If you attempt to skip the first step and you only label the namespace to exempt it, you will receive an error from the API server that only exempt namespaces can have the ignore label:

```
Error from server (Only exempt namespace can have the admission.g
```

Before labeling a namespace, you must first add the namespaces by editing the installed configManagement object and adding a list of namespaces in the `exemptableNamespaces` field, which is added using the GCP console.

To allow a namespace to be added as an exemption using the Anthos console, you need to edit the config management settings for the cluster. In the GCP console, open Anthos > Config Management, to see the list of clusters that are available. Select the radio button next to the cluster you want to configure and click the configure button at the top of the GCP console page. If you expand the settings and scroll to the bottom, you will see the policy controller section, then click on the ACM settings for your clusters which contains a list of exempt namespaces.

Figure 16.3 Adding an exemption using the GCP console

Labeling the namespace to ignore Gatekeeper will cause the admission controller to ignore every policy for any object that is created in the namespace. Rather than exempting a namespace from every policy, you may want to consider exempting the namespace from individual policies, allowing you to enforce some policies, while exempting only the required policies for object creation in the namespace.

### Exempting a Namespace from a Constraint Template

If you find yourself in a situation where you need to exempt a certain namespace from a policy, but you cannot exempt the namespace from all policies, you can add an exemption to the constraint itself. For example, we have a policy that requires all namespaces to have a billing code assigned to them. However, you want to exempt the kube-system namespace and a new namespace that will be created called web-frontend. We can do this by adding the `excludedNamespaces` field to our constraint:

```
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: ns-billing
spec:
  match:
    kinds:
    - apiGroups: [""]
      kinds: ["Namespace"]
    excludedNamespaces:
    - kube-system
    - web-frontend
  parameters:
    labels:
    - key: "billing"
```

If we attempt to create a new namespace called test-fail without a label, Gatekeeper will deny the request with the error below:

```
Error from server ([denied by ns-billing] you must provide labels
```

However, if we try to create the web-frontend namespace without a label, Gatekeeper will allow it, since it is included in the excludedNamespaces for the constraint.

```
[root@localhost gke-bm]# kubectl create ns web-frontend
namespace/web-frontend created
```

ACM provides granular controls to exempt namespaces by providing the ability to exempt a namespace from either all policies or only certain policies. You can use a mix of each type for different namespaces, exempting one namespace for all policies, while other namespaces may only be exempt from certain policies. Try to avoid exempting namespaces from all policies, unless there is strong justification to do so. Once exempt, no policies will ever be enforced for a fully exempted namespace.

While Anthos includes a number of constraint templates, there may be scenarios where you need to create a custom constraint. In the next section, we will explain how to create a custom constraint template, allowing you to extend the included set of policies.

## Creating a Constraint Template

The default template library included with ACM has grown from a handful of policies in the early releases, to 32+ with the most current release. Google and the community will continue to add policies to the default library, but you may have a unique policy requirement for your clusters that Google does not provide.

If you find yourself needing to create a policy, you can create a custom constraint template by creating your own policy using a language called Rego. Covering Rego in depth is beyond the scope of this book, but you can read more about Rego and how to use it to create policies at https://www.openpolicyagent.org/docs/latest/policy-reference/.

To create a new template, you need to create a new ConstraintTemplate object, which will contain the Rego code to evaluate the policy. Google has documentation to assist you in creating a template at https://cloud.google.com/anthos-config-management/docs/how-to/write-a-constraint-template#example_constraint.

The below example will create a new template that will check for an image digest when a new container is created.
