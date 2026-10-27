# Kustomize Feature List

Kustomize comes with all the features that are needed to customize your Kubernetes deployment. These include:

+   - Generating Resources (Config Maps and Secrets)
+   - Setting cross-cutting fields
+   - Composing and Customizing Resources

Figure 14.33 Kustomize features

![](bbox=[0.185, 0.34, 0.795, 0.62])

Let's have a look at each of them in details:

# Composing

You can use resources attribute to define which resources definitions you want to customize.

If you don’t include any other customization feature in the file the resources will be simply composed into one definition. In the below example we compose deployment.yaml and service.yaml definitions.

```
# kustomization.yaml
```