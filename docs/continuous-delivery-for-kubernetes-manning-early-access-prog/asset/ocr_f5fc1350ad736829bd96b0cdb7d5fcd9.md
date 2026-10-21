Both Knative Eventing Brokers and Triggers are Kubernetes resources that you can create using `kubectl` or any tool that you want to use to sync Kubernetes resources into your clusters.

```
A Broker resource will look like this:
apiVersion: eventing.knative.dev/v1
kind: Broker
metadata:
  name: default
  namespace: default
```

You can create as many brokers as you want for your application to use. As soon as you create a Broker you can check that the Broker is ready to accept events and that it provides an URL for producers to send events to. Use `kubectl get brokers` to check the available Brokers:

```
salaboy> kubectl get broker
NAME    URL                                     AGE   READY   REASON
default    http://broker-ingress.knative-eventing.svc.cluster.local/default/default   2s
          True
```

If we now configure our producer (application-a) to send events to the Broker URL, the events will be ready to be consumed from the Broker and we can wire consumers to the broker by creating Knative Eventing Triggers which will look as follow:

```
apiVersion: eventing.knative.dev/v1
kind: Trigger
metadata:
  name: app-b-trigger
  namespace: default
spec:
  broker: default
  subscriber:
    ref:
      apiVersion: v1
      kind: Service
      name: application-b-service
```

A Trigger responsibility is to register interest in events that are published in a Broker. This example trigger is registering interest in all events that are published by producers to the Broker’s URL. All the events that are published to the broker by producers will be forward to the `application-b-service` Kubernetes Service as specified in the `subscriber.ref` field.

If we are not interested in all events that are going to the Broker we can specify filters, for example: