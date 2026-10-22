this is one of the best things you can do for your developers and data engineers. Default secure!

# Kubernetes Operator for Apache Spark

If Spark can run in Kubernetes via `spark-submit`, why do we need an operator? As you learned in previous chapters, Kubernetes operators give you more flexibility in managing applications and a more cloud native experience overall. Using `spark-submit` to run your Spark applications requires your production systems to be set up with a local installation of Spark, including all dependencies. The Spark on Kubernetes Operator allows SREs and developers to manage park applications declaratively using Kubernetes tools such as Helm and `kubectl`. It also allows better observability on running jobs and exporting metrics to external systems like Prometheus. Finally, using the operator provides an experience much closer to running other applications in Kubernetes.

The first step is to install the operator into your Kubernetes cluster using Helm.

```
$ helm repo add spark-operator \
  https://googlecloudplatform.github.io/spark-on-k8s-operator

$ helm install my-release spark-operator/spark-operator \
  --namespace spark-operator --create-namespace
```

Once completed, you will have a SparkApplication controller running and looking for SparkApplication objects. This is the first big departure from `spark-submit`. Instead of a long list of command line arguments, you use the SparkApplication CRD to define the Spark job in a YAML file. Let’s look at a config file from the official documentation.

```
apiVersion: "sparkoperator.k8s.io/v1beta2"
kind: SparkApplication
metadata:
  name: spark-pi
  namespace: default
spec:
  type: Scala
```