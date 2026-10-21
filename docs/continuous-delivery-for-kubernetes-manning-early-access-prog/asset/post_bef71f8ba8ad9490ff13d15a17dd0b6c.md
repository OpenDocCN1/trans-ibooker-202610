
you can define an Ingress resource with two rules. Each rule will be in charge of defining the external path or subdomain to access each service.

![](img/2ab98a5ff1ec073700d902ba95287fd6_137_0.png)

Figure 5.8 Using two Ingress rules for pointing to A and B versions

If you have your application hosted under the www.example.com domain, the Ingress resource defined in figure 5.8 will direct traffic to Service A, allowing users to point their browsers to www.example.com/b to access Service B. Alternatively, and depending on how you have configured your Ingress Controller, you can also use subdomains instead of path-based routing meaning that to access the default version you can keep using www.example.com but to access Service B you can use a subdomain such as test.example.com

I can hear you saying, what a pain, look at all the Kubernetes resources that I need to define and maintain to just achieve something that feels basic and needed for everyday operations. Let’s quickly summarize the limitations and challenges that we found so far, so we can take a look at Knative Serving and how can it help us to implement these strategies in a more streamlined way.

## 5.1.5 Limitations and complexities of using Kubernetes built-in building blocks

Canary Releases, Blue/Green deployments and A/B testing can be implemented using built-in Kubernetes resources. But as you have seen in the previous sections, creating different deployments, changing labels and calculating the number of replicas needed to achieve percentage-based distribution of the requests is quite a major task and very error-prone. Even if you use a GitOps approach as shown with Jenkins X or with other tools in Chapter 3, creating the required resources with the right configurations is quite hard and it takes a lot of effort.

We can summarise the drawbacks of implementing these patterns using Kubernetes building blocks as follows:

- Manual creation of more Kubernetes resources, such as Deployments, Services and Ingress Rules to implement these different strategies can be error-prone and cumbersome. The team in charge of implementing the release strategies need to understand deeply how Kubernetes behaves to achieve the desired configuration.
- No automated mechanisms provided out-of-the-box to coordinate and implement the resources required by each release strategy.
- Error-prone, as multiple changes need to be applied at the same time in different resources for everything to work as expected
- If we notice a demand increase or decrease in our services, we need to manually change the number of replicas for our Deployments or install and configure a custom autoscaler (more on this at the end of this chapter). Unfortunately, if you set the number of replicas to 0, there will not be any instance to answer requests, requiring you to have at least one replica running all the time.

Out of the box, Kubernetes doesn’t include any mechanism to automate or facilitate these release strategies, and that becomes a problem quite quickly if you are dealing with a large number of services that depend on each other.

One thing is clear, your teams need to be aware of the implicit contracts imposed by Kubernetes regarding 12-factor apps and how their services APIs evolve to avoid downtime. Your developers need to know how Kubernetes built-in mechanisms work in order to have more control over how your applications are upgraded.

If we want to reduce the risk of releasing new versions, we want to empower our developers to have these release strategies available for their daily experimentation. In the next sections, we will look at Knative Serving, a set of tools and mechanisms built on top of Kubernetes to simplify all the manual work described in the previous sections.

## 5.2 Reducing releases risk to improve delivery speed

We want to ensure that our operations teams have the right tools to implement the introduced strategies for running our applications. This section covers Knative Serving ( http://knative.dev ), a set of tools and mechanisms that facilitate a progressive delivery ( https://redmonk.com/jgovernor/2018/08/06/towards-progressive-delivery/ ) approach.

While Knative Serving has been associated with Serverless Platforms, we will see in the following sections that a Serverless approach can be achieved but by no means is the only feature that Knative Serving provides. In the following sections, we will see how to implement the previously introduced release strategies with Knative Serving applied to our Conference Platform application.

### 5.2.1 Introduction to Knative Serving

Knative is one of these technologies that it is hard to go back and not use when you learn about what it can do for you. After working with the project for almost 3 years and observing the evolution of some of its components, I can confidently say that every Kubernetes Cluster should have Knative installed; your developers will appreciate it. Knative provides higher-
level abstractions on top of Kubernetes built-in resources to implement good practices and common patterns that enable your teams to go faster, have more control over their services and facilitate the implementation of Event-Driven applications.

As the title of this section specifies, the following sections focus on a subset of functionality provided by Knative, called Knative Serving. Knative Serving allows you to define "Knative Services", which dramatically simplifies implementing the release strategies exemplified in the previous sections. Knative Services will take care of creating Kubernetes built-in resources for you and keep track of their changes and versions, enabling scenarios that require multiple versions to be present at the same time. Knative Services also provides advanced traffic handling and autoscaling to scale down to zero replicas for a serverless approach.

To install Knative Serving on your cluster, I highly recommend you to check the official Knative documentation that you can find here: https://knative.dev/docs/admin/install/serving/install-serving-with-yaml/

If you want to install Knative locally, you should the getting started guide for local development(https://knative.dev/docs/getting-started/#install-the-knative-quickstart-environment). Optionally, you can install `kn`, a small binary that allows you to interact with Knative in a more natural way. You can always use `kubectl` to interact with Knative resources as well.

When installing Knative Serving, you are installing a set of components in the `knative-serving` namespace that understand how to work Knative Serving custom resource definitions.

![](img/2ab98a5ff1ec073700d902ba95287fd6_139_0.png)

Figure 5.9 Knative Serving components in your Kubernetes Cluster

It is outside of the scope of this book to explain how Knative Serving components and resources work; my recommendation is that if I manage to get your attention with the examples in the following sections, you should check out the Knative In Action book by Jacques Chester (https://www.manning.com/books/knative-in-action).

### 5.2.2 Knative Services

Once you have Knative Serving installed, you can create Knative Services. I can hear you thinking: "But we already have Kubernetes Services, why do we need Knative Services?" believe me, I had the same feeling when I saw the same name but follow along, it does make sense.

When you were deploying each Service in the Conference Application before, you needed to create at least two Kubernetes resources, a Deployment and a Service. We have covered in a previous section that using ReplicaSets internally, a Deployment can perform rolling updates by keeping track of the configuration changes in the Deployment resources and creating new ReplicaSets for each change that we made. We also discussed in Chapter 2 the need for creating an Ingress resource, if we want to route traffic from outside the cluster. Usually, you will only create an Ingress resource to map the publicly available services, such as the User Interface of the Conference Platform.

Knative Services build on top of these resources and simplify how we define these resources that you need to create for every application service. While it simplifies the task and reduces the amount of YAML that we need to maintain, it also adds some exciting features, but before jumping into the features, let’s take a look at how a Knative Service looks in action. Let’s start simple and use the Email Service from the Conference Platform to demonstrate how Knative Services work:

```
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: email-service #A
spec:
  template:
    spec:
      containers:
        - image: salaboy/fmtok8s-email-rest:0.1.0 #B
```

#A You need to specify a name for the resource, as with any other Kubernetes resource

#B You need to specify which container image you want to run

In the same way, as a Deployment will pick the `spec.template.spec` field to cookie-cut Pods, a Knative Service defines the configuration for creating other resources using the same field.

Nothing too strange so far, but how is this different from a Kubernetes Service?

If you create this resource using `kubectl apply -f` you can start exploring the differences.

You can also list all Knative Services using `kubectl get ksvc` (ksvc stand for Knative Service) and you should see your newly created Knative Service in there:

```
NAME          URL                                           LATEST CREATED          READY
email-service http://email-service.default.X.X.X.X.sslip.io email-service-00001    True
```
