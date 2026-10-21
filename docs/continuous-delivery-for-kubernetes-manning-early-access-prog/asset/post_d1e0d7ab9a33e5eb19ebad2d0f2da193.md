
![](img/2ab98a5ff1ec073700d902ba95287fd6_121_0.png)

# Figure 4.12 Call for Proposals Service using PostgreSQL provisioned by Crossplane

To summarize what we have achieved so far we can say that:

- We abstracted how to provision a Cloud specific component such as a PostgreSQL database and all the configurations needed to access the new instances,
- We have exposed a simplified interface for the App Ops team which is Cloud Provider independent.
- Finally, we have connected our application service to the newly provisioned instance by relying on a Kubernetes Secret that is created by Crossplane containing all the details required to connect to the newly created instance.

The same can be achieved for the Agenda Service and Redis. You can follow along a step by step tutorial that covers all the steps described in this section here: https://github.com/salaboy/from-monolith-to-k8s/tree/master/crossplane

In this section, we have used a configuration package that I’ve created to provision a PostgreSQL and a Redis database on top of Google Cloud without requiring developers to know where the resources are going to be provisioned or all the cloud provider-specific configurations required to create these resources.

If you create more complex composite resources and configurations, what you are defining is basically a higher-level abstraction on how you want to expose the resources that matter for your applications. In one way or another, you will end up building a domain-specific platform on top of Kubernetes and using Crossplane to define and manage these higher-level constructs. This approach not only simplifies the creation and management of such platforms but it will also allow these constructs to be shared across different cloud providers and split the responsibility of defining and using these abstractions to specialized teams.

The following section quickly reviews the Crossplane Composite Resources used for the walking skeleton application, focusing on the definition of the configuration package that we installed previously.

## 4.4 Building Cloud-Native platforms on top of Kubernetes

Most of the time, the configurations installed in our package will be in charge of the Infra Ops team, who understand more about the Cloud Provider capabilities and configurations.

The source code of the configuration package used can be found here: https://github.com/salaboy/from-monolith-to-k8s/tree/master/crossplane/pkg

To define provider or configuration packages you need to first create a new configuration type describing what your package is going to be about and its dependencies. The package that we installed for the Conference Platform had defined as dependency the "provider-gcp" package as it depends on GCP specific types. You can consider this package as an extension on top of GCP types, which is exactly how we used it in the example.

### 4.4.1 Creating our own Crossplane Configuration Package

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

```
composition.yaml:
apiVersion: apiextensions.crossplane.io/v1
kind: Composition
metadata:
  name: compositepostgresqlinstances.gcp.db.fmtok8s.salaboy.com
  labels:
    provider: gcp
    app: fmtok8s-app
spec:
  writeConnectionSecretsToNamespace: crossplane-system
  compositeTypeRef:
    apiVersion: db.fmtok8s.salaboy.com/v1alpha1
    kind: CompositePostgreSQLInstance
  resources:
    - name: cloudsqlinstance
      base:
        apiVersion: database.gcp.crossplane.io/v1beta1
        kind: CloudSQLInstance
        spec:
          forProvider:
            databaseVersion: POSTGRES_9_6
            region: europe-west2
            settings:
              tier: db-custom-1-3840
              dataDiskType: PD_SSD
              ipConfiguration:
                ipv4Enabled: true
                authorizedNetworks:
                  - value: "0.0.0.0/0"
          writeConnectionSecretToRef:
            namespace: crossplane-system
  patches:
    - fromFieldPath: "metadata.uid"
      toFieldPath: "spec.writeConnectionSecretToRef.name"
      transforms:
        - type: string
          string:
            fmt: "%s-postgresql"
    - fromFieldPath: "spec.parameters.storageGB"
      toFieldPath: "spec.forProvider.settings.dataDiskSizeGb"
  connectionDetails:
    - fromConnectionSecretKey: username
    - fromConnectionSecretKey: password
    - fromConnectionSecretKey: endpoint
    - type: FromValue
      name: port
      value: "5432"
```

Notice that inside the composition we can set sensible defaults, so our users (App Ops teams) don't need to worry about which version of PostgreSQL they need to select, in which region the database needs to be provisioned or even which kind of storage the DB will need.

Your users will not consume the composition straight away, they will rely on a simplified interface that you can define by creating CompositeResourceDefinition which defines the

parameters that a user can set (the shape of the interface) to be propagated to the composition that we defined previously.

With the following `definition.yaml` file we create a schema for our PostgreSQLInstance type which defines which parameters can be set and which parameters are required. For this simple example, the only parameter that you can set and is required is storageGB.

```
definition.yaml:
apiVersion: apiextensions.crossplane.io/v1
kind: CompositeResourceDefinition
metadata:
  name: compositepostgresqlinstances.db.fmtok8s.salaboy.com
spec:
  group: db.fmtok8s.salaboy.com
  names:
    kind: CompositePostgreSQLInstance
    plural: compositepostgresqlinstances
  claimNames:
    kind: PostgreSQLInstance
    plural: postgresqlinstances
  connectionSecretKeys:
  - username
  - password
  - endpoint
  - port
  versions:
  - name: v1alpha1
    served: true
    referenceable: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              parameters:
                type: object
                properties:
                  storageGB:
                    type: integer
                required:
                - storageGB
        required:
        - parameters
```

Notice that these simplified interfaces that you are going to expose to the App Ops team can and should be versioned as with any other resource type.

### 4.4.2 Building and distributing our Configuration Package

Once you have these 3 files defined you can package your configuration package using the Crossplane `kubectl` plugin, by running:

```
kubectl crossplane build configuration
```

This will generate an OCI container that you can push to Docker Hub or any other container registry. The name of the OCI container will be defined by the name of the Configuration resource that you have defined in the crossplane.yaml file. To push the configuration package to the registry you can also use the plugin:

```
kubectl crossplane push configuration salaboy/crossplane-fmtok8s-gcp:0.0.8
```

Notice that you need to have the right credentials to push to the target registry. If not specified it will use Docker Hub, where "salaboy" is my Docker Hub username.

If you really want to create a self-service platform, as the Google Cloud user interface but on top of Kubernetes you will need to build a User Interface that reads from the installed Configuration Packages the **CompositeResourceDefinitions** type Schema definitions. You can build parameterizable user screens where users can select the types of resources they want, enter the required parameters and create resources without the need of having `kubectl` installed in their environments.

### 4.4.3 Extending Crossplane with our custom Providers

Most of the examples and scenarios discussed in this chapter are targeting Cloud Providers, but Crossplane can be also used for On-Premises setups and extended to support other managed services besides the ones provided by AWS, Azure and Google. The abstractions and mechanisms provided by Crossplane are valuable in a wide range of scenarios where the resources that we want to provision and monitor are outside of the Kubernetes Cluster where our services are running.

If you want to extend Crossplane with your own Providers I recommend you to check the Provider Template (https://github.com/crossplane/provider-template) project that shows a simple example of how a Provider can be created and distributed. A Crossplane Provider consists of a Kubernetes Controller which will use credentials to connect to the remote Managed Service and then expose the resources that can be provisioned remotely as Kubernetes Resources.
