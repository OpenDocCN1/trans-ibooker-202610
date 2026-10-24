+   ❶ This is the file containing configuration information for your function project
+   ❷ The POM file for this Quarkus project
+   ❸ The Java class containing annotations and code to run the function

Let’s add some content for the `func.yaml` function’s configuration file to transform your function into a runnable container image on Kubernetes.

```
name: quarkus-faas
namespace: "coolstore"
runtime: quarkus
image: "docker.io/modernizingjavaappsbook/quarkus-faas:latest"
imageDigest: ""
trigger: http
builder: quay.io/boson/faas-quarkus-jvm-builder
builderMap:
  default: quay.io/boson/faas-quarkus-jvm-builder
  jvm: quay.io/boson/faas-quarkus-jvm-builder
  native: quay.io/boson/faas-quarkus-native-builder
env: {}
annotations: {}
```

+   ❶ Name of the function.
+   ❷ The Kubernetes namespace where your function will be deployed.
+   ❸ The language runtime for your function declared at creation time.
+   ❹ This is the image name for your function after it has been built.
+   ❺ The invocation event that triggers your function. For example, `http` for plain HTTP requests such as in this case, or `event` for CloudEvent triggered functions.
+   ❻ Specifies the buildpack builder image to use when building the function