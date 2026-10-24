package the application as a container and deploy it on the cluster as a scale-to-zero serverless app.

Popular public cloud serverless solutions such as AWS Lambda, Azure Functions, or Google Cloud Run provide a convenient SDK to start developing functions written in the most popular programming languages, to be packaged and deployed in the FaaS model. There are also available open-source solutions like [Apache OpenWhisk](Apache OpenWhisk) or [Fn project](Fn project) implementing FaaS with Docker. In the following sections, we will focus on Knative and Kubernetes, as we have discussed throughout the book how Kubernetes provides a complete ecosystem for easing the migration of Java enterprise applications to the cloud-native paradigm.

# Functions Deployment for Java Applications

Functions are a piece of code delivered according to the serverless model and portable between different infrastructure configurations. The lifecycle of a function is described in [Figure 7-6](Figure 7-6) starting with code writing, as well as specification and metadata. The building phase automatically happens afterward, and the deployment publishes the function in the platform. This enables a mechanism of updates that will trigger a new build and a new publish when a new change is needed.

![](bbox=[0.16, 0.62, 0.8, 0.76])

Figure 7-6. Functions deployment model

# Boson Function CLI (func)