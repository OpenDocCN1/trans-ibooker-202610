
## 11 Knative serverless extension

by Konrad Clapa

### This chapter covers:
- Introduction to serverless
- Knative Serving and Eventing Components
- Knative on Anthos

Before we get into the details let us set up the scene. What we are going to talk about in this chapter is Google Cloud Platform’s managed service based on an open source project called Knative. The project was started to allow for higher development velocity of Kubernetes applications without need to understand the complex Kubernetes concepts it uses. With this service, Google installs and manages Knative serving inside your Anthos GKE cluster. One of the benefits of using Knative with Anthos instead of open source Knative is that Google's automation and Site Reliability Engineers all handle installation and maintenance. Anthos integrates with numerous GCP services like Cloud Load Balancing[1], Cloud Armor[2], Cloud CDN[3] and many others making an enterprise-ready Knative a reality.

### 11.1 What is the problem we are trying to solve

We have already discussed Kubernetes in Chapter 4, Anthos, the computing environment based on Kubernetes so we have a feeling how complex the installation and maintenance of it can be. This problem is being solved for us with Google Kubernetes Engine. On top of Kubernetes we still need to know how to run and operate cloud native applications. What Knative does is abstract those implementation details and allow you to serve your serverless container based workloads on any Kubernetes cluster.

In this chapter we will look at what serverless is, introduce you to Knative and finally discuss how Anthos delivers an enterprise grade container based serverless platform.

### 11.2 Introduction to Serverless

There is a lot of discussion about what serverless is. The most common thing to see is comparisons between serverless and Function as a Service (FaaS). This is almost an ideological dispute. To keep this simple let's have a look at the Cloud Native Computing Foundation definition:

> *Serverless computing refers to a new model of cloud native computing, enabled by architectures that do not require server management to build and run applications.*

Google's fully managed Cloud Run service perfectly fits into this definition as it abstracts the compute layer from the developer and operator. It allows you to deploy the containers that will be serving http(s) requests. The scaling of the application is handled by the platform itself.

While Google Cloud Functions deliver similar capabilities, it is more opinionated about the runtime languages you can use. With Cloud Run you can use any language that can run a service able to answer HTTPS calls. Cloud Run does not require Anthos to be used.

When we think about Cloud Run we can think about the following set of serverless features:

- No server - developers don’t need to worry about underlying compute infrastructure
- Multi-Language - the application can be written in any language
- Event-driven - the container/function is triggered by an external event
- Autoscaling - the container can automatically scale based on requests.
- Portability - your container/application should be able to run on any Kubernetes platform.

### 11.3 Knative

As you already know Kubernetes is a platform for building platforms. Then why not use it for building serverless platforms based on containers? Knative runs on top of Kubernetes like any other Kubernetes application. You can even see some statements from the Knative contributors that it should not be called serve less so, let's have a look at Knative as a platform to deliver serverless anywhere where you run Kubernetes. Sounds fair?

#### 11.3.1 Introduction

Say you would like to build your own Kubernetes-based serverless platform you might come up with the following diagram showing all the required components. Clearly there is some duplication of effort related to build of the primitives like autoscalability, observability, rollouts and many others. What Knative is doing is providing all these primitives for you so there is a common experience of running serverless workload on Kubernetes.

Figure 11.1. Kubernetes Serverless stack architecture

### Kubernetes Serverless stack architecture

Products

Developer Experience

Event Sources

Runtimes

...

Primitives

Build

Observe

Invoke

Rollout

Route

Bind Events

Autoscale

...

Common effort

Platform

Kubernetes

Now think of it from the developers perspective. All they have to do is to define the dependencies, write their code and put it in a container. Then they deploy the application to Knative. The details of how it is served is not of their concern. This however does not mean that they lose the capability to finetune the service. There are multiple parameters that can be set like concurrency (how many requests can be served per container), min/max instances (minimum/maximum container instances that can be provisioned for the Knative Service) and many others. Knative hides away all the complexities of Kubernetes involved with scaling and traffic management and provides a means to observe the workloads. That is what you call an easy start with development of Kubernetes applications, right?

### Knative vs CaaS, FaaS and PaaS

In the second section of this chapter we learned what problems Knative is trying to solve: Enable serverless workloads to run anywhere with the flexibility of Kubernetes but hiding the complexity away. The below table presents a comparison of Knative against platform as a service (PaaS)[4], container as a service (CaaS)[5] and function as a service (FaaS)[6]. With various features being supported or not. Do it yourself (DIY) means you need to put some development to be able to use that feature.

### Table 11.1. Knative vs PaaS, CaaS and FaaS

| Feature | Knative | PaaS | CaaS | FaaS |
|---|---|---|---|---|
| Simple UX/DX | Yes | Yes |  | Yes |
| Event-driven | Yes | Yes |  | Yes |
| Container based | Yes | Yes | Yes | Yes |
| Autoscaling | Yes | Yes | DIY | Yes |
| Scale resources to 0 | Yes | Yes |  | Yes |
| Load Balancing | Yes | Yes | DIY | Yes |
| Unrestricted execution time | Yes* | Yes | Yes |  |
| Unrestricted compute/memory limits | Yes** | Yes** | Yes** |  |
| Variety of programming languages support | Yes | Yes | Yes | Limited |

*might be restricted for Managed services like Cloud Run.

** depends on the platform

As we can see, the Knative gives you all the advantages of function as a service but also gives you the ability to run your application in almost any language. The limits on the execution time are much higher compared to FaaS. In many cases where there is a need for longer request processing time Knative is a solution. You finally have access to advanced features like volumes and networking so you can tweak your workload if needed. Like with all compute services with higher flexibility the responsibility demarcation line shifts more toward you. You need to build your own container and make sure you are able to make use of all the advanced features but let's be honest, who does not like to be in more control of your application until you get all the benefits of FaaS?

#### 11.3.2 Knative History

Google started Knative but now has multiple companies contributing to it like IBM, RedHat, Pivotal, SAP and many others. The full documentation and source code can be found here: https://github.com/knative. Knative started as a set of components that allows the build and run of stateless workloads together with subscription to events. There are currently two active projects in progress on Github those are:

- **Knative Serving** - allows to serve serverless containerised workloads

#### 11.3.3 Knative Architecture

Let’s have a look at Knative architecture presented in the below diagram. As you can already notice there are multiple layers with some of the components being plug and play or optional.

### Figure 11.3. Knative Architecture

![](img/fb3bebce7b5f593bca1a7f8a1371be06_338_0.png)

Knative can run on any compute platform that has the ability to run Kubernetes. It can be either based on virtual machines or bare-metal servers. For traffic routing, Service Mesh Gateway is used. Obviously the most popular is Istio but alternative solutions are also supported like: Gloo, Ambassador, Contour and Kourier with more to come. To learn more about Istio refer to Chapter 5 Anthos Service Mesh: security and observability at scale. On top of that we have Knative components installed as a Kubernetes application. Note that each of those components can be installed and operated
