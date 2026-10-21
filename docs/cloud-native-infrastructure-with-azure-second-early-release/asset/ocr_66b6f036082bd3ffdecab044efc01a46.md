+   - Test your function by calling the URL:

```
$ curl -X POST <URL found from above> -d "My Data"
Hello World: My Data
```

Now that a basic serving function is running, let’s look at Knative eventing.

# Installing and Running Knative Eventing on Kubernetes

In this section, we will install Knative eventing components and write a simple eventing system.

+   - Start by installing the custom resources:

```
kubectl apply -f
https://github.com/knative/eventing/releases/download/v0.26.0/eventing-crds.yaml
```

+   - Now install the serving components:

```
kubectl apply -f
https://github.com/knative/eventing/releases/download/v0.26.0/eventing-core.yaml
```

At this time, you will have the eventing components installed. We are going to reuse our Docker image from the preceding section. However, we will configure Knative so that we can send an HTTP message to a message bus within Knative instead of posting to an HTTP application server. When the message reaches the message bus, it will trigger our Python application server. In a more complicated example, you can set a Kafka topic as a source of events.

+   - Create a *knative-eventing.yaml* file: