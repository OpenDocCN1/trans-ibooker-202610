Eventing Broker is an Event Mesh that pulls or receives the events while Knative Trigger filters and routes events to the targets. Event sources are the control plane of Knative Eventing that makes sure events are sent to the broker.

Now let's have a closer look at the resources.

+   - **Broker API path broker.eventing.knative.dev** is essentially an addressable Event Delivery System that you install in your namespace. This gets done by setting a label on your namespace. Similar as what you do with Istio when you want to do sidecar injections into the Pods. Events are received by the Broker and then sent to subscribers. The messages are stored in a channel that is managed by the Broker.

The channel can be a simple In-memory channel or can use persistent storage for reliability purposes. Examples of those are Pub/Sub and Kafka. The configuration of the channels is stored in ConfigMaps. You can install Broker into multiple namespaces. The use case for this is if you want to have different types of messages. You can also filter which events are accepted by the Broker.

Bellow we can see an example definition of a Knatvie Broker:

```
apiVersion: eventing.knative.dev/v1
kind: Broker
metadata:
  annotations:
    eventing.knative.dev/broker.class: MTChannelBasedBroker
  name: default
  namespace: default
spec:
  config:
    apiVersion: v1
    kind: ConfigMap
    name: config-br-default-channel
    namespace: knative-eventing
```

**Triggers API path trigger.eventing.knative.dev** matches the event with a Service. So it is defined for which type of event (e.g Cloud Storage object created sent the event to that Service). Triggers can filter the events based on one or more attributes. If there are multiple attributes all of the attributes