```
Extensions,
    Knativearrivaltime: 2021-11-09T06:21:26Z
    knativehistory: default-kn2-trigger-kn-channel.knative-samples.svc.cluster.local
    traceparent: 00-971d4644229653483d38c46e92a959c7-92c66312e4bb39be-00
```

```
Hello World Message "My data"
Responded with event Validation: valid
Context Attributes,
    specversion: 0.2
    type: dev.knative.samples.hifromknative
    source: knative/eventing/samples/hello-world
    id: 37458d77-01f5-411e-a243-a459bbf79682
Data,
    My data
```

# KEDA

KEDA stands for *Kubernetes Event-Driven Autoscaling*. KEDA allows you to scale any container in Kubernetes based on the number of incoming events. KEDA is an add-on to Kubernetes and utilizes the Horizontal Pod Autoscaler (HPA) to scale based on a variety of event sources, including Apache Kafka, Azure Event Hubs, and NATS streaming.

# KEDA Architecture

A typical KEDA architecture is depicted in Figure 11-3, where KEDA interacts directly with the Kubernetes API server, HPA, and the configured trigger source.

The KEDA component performs the following roles:

*Controller and scaler*