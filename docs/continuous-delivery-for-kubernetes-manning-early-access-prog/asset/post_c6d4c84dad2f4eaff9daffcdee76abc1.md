
![](img/2ab98a5ff1ec073700d902ba95287fd6_179_0.png)

Functions accept one input event, do some processing, that can include to access external resources and when they are done processing emit an output event for other functions to do more processing.

Figure 7.1 Function's inputs and outputs

Functions can be chained together by just tapping into previous function output events. The idea here is that developers shouldn’t worry about where the events are coming from or where the output event is placed, this is all handled by the platform as well. This reduces the cognitive load from developers and enables the platform to provide a simplified user experience.

![](img/2ab98a5ff1ec073700d902ba95287fd6_179_1.png)

Multiple functions can be interested in a specific event, and the platform is responsible of routing these events to the right functions. This allows application developers to just write their functions and not worry about how to consume an event or where to place the output events.

Figure 7.2 Chaining multiple functions based on events

Functions by themselves are really useful constructs, but they cannot work on their own. To be able to work with Functions you need to have a platform that takes care of running these functions, scaling them and routing events to the right functions so they can perform their tasks.

## THE NEED FOR FUNCTION-BASED PLATFORMS

If you are a developer tasked to write a set of functions to perform some very domain-specific tasks, you don’t want to worry about how these functions will be built and deployed, how they will be scaled up or down, or how to route events from one function to the next.

Functions can be elastically scaled up and down based on demand, but the scaling will be performed automatically by the platform without any user intervention, reducing the amount of maintenance of your runtime environments. Less maintenance also means a more complex platform that will take care of things like scaling for us. It also means that the behaviour that we can include inside our functions is in some way limited to respect the function boundaries.

In FaaS platforms it is quite common to talk about cold starts, as functions can be downscaled to zero when a new event arrives, the platform and the end-user will need to wait until the function bootstrap to be able to process the incoming event. If you have dozens or hundreds of functions, this latency can add up. Also because we are dealing with events, we are in a very asynchronous environment.

But as a function developer, you shouldn’t worry about:

- How to build the function code into a runnable function
- How to consume an event, the platform will give you a way to register interest in a certain type of event.
- How to produce an event, you just produce it and the platform will be in charge of routing the event to the interested parties
- Being restricted by a single programming language, you can write functions in any language you want as soon as the platform supports it
- Knowing which other functions are running at a given time, since each function is independent of others
- How many replicas you need to support ever-changing demands, as the platform will take care of it

A very prominent FaaS (Serverless) and a very good example of such a platform is AWS Lambda, which provides a polyglot environment to write functions in different languages. But this obviously doesn’t run inside Kubernetes and is vendor-specific.

There are some preconceptions as to how functions are different from Services the following table goes into these differences, but as we will see later on we can blur these differences in the Kubernetes world

| Functions | Services |
|---|---|
| New replicas are created when they are needed, based on demand | A Service is started and it runs until it is manually downscaled. If you need to scale up the Service you need to do it manually or configure an autoscaler to perform this scaling when certain criteria are met. |
| Functions need to start fast, as there will be requests waiting for a new instance to start | A Service can take a bit longer to start, because it is manually started by an operator. The operator can decide when the Service is up and ready to be used. |
| Functions downscale to zero if there is nobody using them. The FaaS platform will have a way to cache requests until a new instance of the target function is scaled up and ready to process the request | Services needs to be manually downscaled if there is no demand. By default, a Service cannot be downscaled to zero, as incoming requests will fail if there is no instance to process them. |
| Functions are focused on solving a very specific problem, usually receiving an event as input and producing an event as output | Services can host multiple endpoints to perform different tasks. Services are considered larger in scope |

So how can we get closer to Kubernetes and also rip all the benefits from a FaaS Platform?
Let’s take a quick look at a new breed of Platforms called Containers as a Service (CaaS).

## 7.1.2 Containers as a Service Platforms

With the rise of containers, it makes a lot of sense to build platforms that can take any container as input and run it, manage it and scale it for you. These platforms aim to leverage the FaaS platform approach to enable developers to worry about their code and not how to run or scale their containers.

A prominent example of this platform is Google Cloud Run, which will take care of building and deploying your containers for you:

- Java: https://cloud.google.com/run/docs/quickstarts/build-and-deploy/java
- Go: https://cloud.google.com/run/docs/quickstarts/build-and-deploy/go

AWS Lambda is also supporting Containers. Both Google Cloud Run and AWS Lambdas are vendor-specific tools, but at least both offerings accept pre-built containers.

If we are talking about containers, then running and orchestrating them with Kubernetes makes a lot of sense. With Knative we can build a Container as a Service Platform for our developers which leverage autoscaling, scaling to zero and the Knative Eventing model.

The main missing bits, even with Knative, we still need to create a Knative Service resource using YAML files and we need to make sure that Knative can access a container image that someone needs to build, hence developers will need to create some kind of Dockerfile to containerize their applications.

Knative is providing all the building blocks to build these developers' experiences and that's why the Knative community has been working on a project called `func` which focuses on enabling a FaaS and CaaS experience on top of your Kubernetes Clusters. We'll consider this in more detail in the next section.

## 7.2 Getting Closer to Developers with Knative `func`

In this section, we will be looking at Knative Functions (`func`) a project that was created to enable developers to use a function-based approach to build applications on top of Knative. One of the main design objectives behind Knative Functions is to simplify the developer experience by enabling developers to write functions without the need of thinking about containers or even Kubernetes.

The main idea behind looking at how `func` was designed and implemented is to highlight the need for building specialized developer experiences that hide the complexity of the technology stack used to run the software that they are writing. `func` is focused on allowing developers to choose their own technology stack to write functions and use CloudEvents as the input and output for these functions.

By using functions as the main building block, `func` allows you to quickly scaffold a project with an empty function template that consumes a CloudEvent and produce a CloudEvent as result.

In the following sections, we will be:

- Creating a project using the `func` CLI
- Building the project
- Running the project locally
- Deploying the function into a Kubernetes Cluster

Let’s get started.

### 7.2.1 Creating a Project with `func`

Once you created a `func` project, `func` takes care of the lifecycle of the project by providing commands to build, run and deploy the project into a Kubernetes cluster that needs to have Knative installed.

You can install `func` and create a project with the following command in an empty directory, let’s call it `fmtok8s-java-function`:

```
func create fmtok8s-java-function -l springboot -t cloudevents
```

The `-l` parameter specifies the tech stack that you want to use, in this case, is Java using the Spring Boot framework and Maven as the build tool. The `-t` parameters specifies which function template we want to use, in this case, a function that works with CloudEvents.

I encourage you to try `-l go` to create a similar function using Golang.
