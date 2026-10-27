Labeling the namespace to ignore Gatekeeper will cause the admission controller to ignore every policy for any object that is created in the namespace. Rather than exempting a namespace from every policy, you may want to consider exempting the namespace from individual policies, allowing you to enforce some policies, while exempting only the required policies for object creation in the namespace.

# Exempting a Namespace from a Constraint Template

If you find yourself in a situation where you need to exempt a certain namespace from a policy, but you cannot exempt the namespace from all policies, you can add an exemption to the constraint itself. For example, we have a policy that requires all namespaces to have a billing code assigned to them. However, you want to exempt the kube-system namespace and a new namespace that will be created called web-frontend. We can do this by adding the excludedNamespaces field to our constraint:

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