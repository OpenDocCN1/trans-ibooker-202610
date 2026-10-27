```
```yaml
- name: example-configmap
  literals:
  - Foo=Bar
```
```

and run:

```
```bash
kubectl kustomize ./
```
```

# Using variables

Variables feature is very useful when you want to capture an attribute of one resource and pass it to other resources. As an example you might want to use the service name to pass it to a container command to be executed. Note that this currently supports string types. Please check the following link to see example usage

[https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/vars/](https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/vars/)

# Bases and Overlays

Now that we have a good understanding of what features Kustomize brings we can have a look at how we can use it to prepare Kubernetes for different environments in your CI/CD pipeline.

Kustomize comes with concepts of **Bases** and **Overlays**. The Base is a root directory with Kubernetes resources definitions and main kustomization.yaml file. This performs a first layer of customization. Note that you can also set your base in a git repository. Overlay is a directory that stores kustomization.yaml that customizes resources already customized in the base layer. You can create multiple overlays to represent each of your environments.

**Figure 14.34** Kustomize base and overlays