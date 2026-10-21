The following section quickly reviews the Crossplane Composite Resources used for the walking skeleton application, focusing on the definition of the configuration package that we installed previously.

# 4.4 Building Cloud-Native platforms on top of Kubernetes

Most of the time, the configurations installed in our package will be in charge of the Infra Ops team, who understand more about the Cloud Provider capabilities and configurations.

The source code of the configuration package used can be found here: https://github.com/salaboy/from-monolith-to-k8s/tree/master/crossplane/pkg

To define provider or configuration packages you need to first create a new configuration type describing what your package is going to be about and its dependencies. The package that we installed for the Conference Platform had defined as dependency the "provider-gcp" package as it depends on GCP specific types. You can consider this package as an extension on top of GCP types, which is exactly how we used it in the example.

# 4.4.1 Creating our own Crossplane Configuration Package

As with every component that we install in our Kubernetes Cluster, we will need to maintain it and upgrade it if we need to update the configurations or if Crossplane component changes, hence defining which Crossplane version we are targeting is important.

```
crossplane.yaml:

apiVersion: meta.pkg.crossplane.io/v1
kind: Configuration
metadata:
  name: fmtok8s-gcp
  annotations:
    app: fmtok8s-app
    provider: gcp
spec:
  crossplane:
    version: ">=v1.0.0-0"
  dependsOn:
  - provider: crossplane/provider-gcp
    version: ">=v0.16.0"
```

The next step is to create a Composition and for the example, I have kept it as simple as possible, but here is where the power of Crossplane really shines, as you can create complex compositions implementing common patterns and best practices that the users of your platform will need. The following yaml file defines the CompositePostgreSQLInstance type, which for the sake of simplicity just involves a CloudSQLInstance type coming from the Crossplane GCP provider. It is important to notice that a configuration package can contain as many compositions as you want.