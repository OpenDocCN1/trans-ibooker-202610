traffic rules.

**Note:**

This is different from the native Kubernetes Service object, which might be confusing to new users at first.

Below we can see an example of Service definition that deploys a simple hello world workload to Knative Serving. This will automatically create other resources: Revision, Configurations and Route.

# Listing 11.1

```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
    name: helloworld
    namespace: default
spec:
    template:
        spec:
            containers:
                - image: docker.io/{username}/helloworld
                  env:
                    - name: TARGET
                      value: "Python Sample v1"
```

+   - **Revision** (API path *revision.serving.knative.dev*): This is essentially an immutable snapshot of the container version and its configuration. It defines what is actually served to the user..
+   - **Configuration** (API path *configuration.serving.knative.dev*): This is the configuration part of your application that enforces the desired state of your workload. It allows you to separate your code (container) from the configuration piece. Modification of the configuration results in new revision creation.
+   - **Route** (API path *route.serving.knative.dev*): This maps the endpoints to one or more revisions.

When you use Knative you no longer need to worry about the native Kubernetes and Service Mesh resources like: *Deployments*, *Services*,