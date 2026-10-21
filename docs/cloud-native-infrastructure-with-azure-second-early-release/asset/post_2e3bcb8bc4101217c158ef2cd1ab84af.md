
# Namespace for sample application with eventing enabled
apiVersion: v1
kind: Namespace
metadata:
    name: knative-samples
    labels:
        eventing.knative.dev/injection: enabled
---
# A default broker
apiVersion: eventing.knative.dev/v1
kind: Broker
metadata:
    name: default
    namespace: knative-samples
    annotations:
        # Note: you can set the
eventing.knative.dev/broker.class annotation to change
the class of the broker.
        # The default broker class is MTChannelBasedBroker,
but Knative also supports use of the other class.
        eventing.knative.dev/broker.class:
MTChannelBasedBroker
spec: {}
---
# hello-world app deployment
apiVersion: apps/v1
kind: Deployment
metadata:
    name: hello-world
    namespace: knative-samples
spec:
    replicas: 1
    selector:
        matchLabels: &labels
app: hello-world
template:
  metadata:
    labels: *labels
  spec:
    containers:
      - name: hello-world
        image:
https://myharborinstallation.com/helloworld:v1
        imagePullPolicy: IfNotPresent
---
# Service that exposes helloworld-python app.
# This will be the subscriber for the Trigger
apiVersion: v1
kind: Service
metadata:
  name: hello-world
  namespace: knative-samples
spec:
  selector:
    app: hello-world
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
---
# Knative Eventing Trigger to trigger the helloworld-python service
apiVersion: eventing.knative.dev/v1
kind: Trigger
metadata:
  name: hello-world
  namespace: knative-samples
spec:
broker: default
filter:
  attributes:
    type: dev.knative.samples.hello-world
    source: dev.knative.samples/helloworldsource
subscriber:
  ref:
    apiVersion: v1
    kind: Service
    name: hello-world

- Apply the *knative-eventing.yaml* configuration on Kubernetes:

```
$ kubectl apply -f knative-eventing.yaml
```

This will create a new Kubernetes namespace called `knative-samples` that allows eventing, and a message broker that will pass messages to our Python application server (on events).

- We will now send an event to our Knative message broker. Here is the response:

```
curl -v "broker-ingress.knative-eventing.svc.cluster.local/knative-samples/default" \
-X POST \
-d 'My data'
```

```
Event received. Context: Context Attributes,
  specversion: 0.3
  type: dev.knative.samples.hello-world
  source: dev.knative.samples/helloworldsource
  id: 536808d3-88be-4077-9d7a-6a3f162705f79
  time: 2021-11-09T06:21:26.09471798Z
  datacontenttype: application/json
```

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

## KEDA

KEDA stands for *Kubernetes Event-Driven Autoscaling*. KEDA allows you to scale any container in Kubernetes based on the number of incoming events. KEDA is an add-on to Kubernetes and utilizes the Horizontal Pod Autoscaler (HPA) to scale based on a variety of event sources, including Apache Kafka, Azure Event Hubs, and NATS streaming.

## KEDA Architecture

A typical KEDA architecture is depicted in Figure 11-3, where KEDA interacts directly with the Kubernetes API server, HPA, and the configured trigger source.

The KEDA component performs the following roles:

*Controller and scaler*

The controller and scaler will scale the number of Pods up or down depending on the signal from the metrics adapter.

## Metrics adapter

The metrics adapter acts as a Kubernetes metrics server that provides queue length or stream lag information to the agent. The scaler uses this information to scale containers up and down.

You can see all of the KEDA scalers in the Scalers documentation.

![](img/8d47d1b5dbca55e784ae602145a894c7_371_0.png)

Figure 11-3. The KEDA reference architecture

## Installing KEDA on Kubernetes

KEDA is best installed using the Helm operator on Kubernetes. We will walk through this now.

- Add the KEDA Helm repository:
  
  $ helm repo add kedacore
  https://kedacore.github.io/charts
  
- Install the Helm chart:
  
  $ kubectl create namespace keda
  $ helm install keda kedacore/keda --namespace keda

After this, your Kubernetes cluster will be ready for function deployment.

In the next example, we will build a proof-of-concept event system with an Azure Service Bus queue. We will produce messages to the Azure Service Bus queue using a Python script and then use KEDA to scale a Python consumption job.

- Grab your Service Bus connection string using either the manual method described in the Azure documentation or by running:
  
  az servicebus namespace authorization-rule keys list \
    -n RootManageSharedAccessKey -g <group> --namespace-name <namespace> \
    --query primaryConnectionString -o tsv
  
- You will need to get the Base64-encoded values of the connection string and the queue name by running:
  
  $ echo -n "<connection string>" | base64
  $ echo -n "<queue name>" | base64

- Now create a file called *secret.yaml*. Replace the `servicebus-queue` and `servicebus-connectionstring` values with the Base64 strings generated in the previous step.

```
apiVersion: v1
kind: Secret
metadata:
  name: sample-job-secret
  namespace: default
data:
  servicebus-queue:
    <base64_encoded_servicebus_queue_name>
  servicebus-connectionstring:
    <base64_encoded_servicebus_connection_string>
```

- Create a second file, called *keda.yaml*, which will create your scaled job. It will be running a docker image (`docker.pkg.github.com/prabdeb/sample-python-keda-service-bus-scaler/consumer:latest`) when triggered:

```
apiVersion: keda.sh/v1alpha1
kind: TriggerAuthentication
metadata:
  name: auth-service-bus-sample-job
spec:
  secretTargetRef:
    - parameter: connection
      name: sample-job-secret
      key: servicebus-connectionstring
---
apiVersion: keda.sh/v1alpha1
kind: ScaledJob
metadata:
name: servicebus-queue-so-sample-job
namespace: default
spec:
  jobTargetRef:
    parallelism: 1 # max number of desired pods
    completions: 1 # desired number of successfully
finished pods
    activeDeadlineSeconds: 600 # Specifies the duration
in seconds relative to the startTime that the job may be
active before the system tries to terminate it; value
must be positive integer
    backoffLimit: 6 # Specifies the number of retries
before marking this job failed. Defaults to 6
    template:
      metadata:
        labels:
          app: sample-keda-job
      spec:
        containers:
        - name: sample-keda-job
          image: docker.pkg.github.com/prabdeb/sample-python-keda-service-bus-scaler/consumer:latest
          env:
          - name: SERVICE_BUS_CONNECTION_STR
            valueFrom:
              secretKeyRef:
                name: sample-job-secret
                key: servicebus-connectionstring
          - name: SERVICE_BUS_QUEUE_NAME
            valueFrom:
              secretKeyRef:
                name: sample-job-secret
                key: servicebus-queue
        restartPolicy: Never
pollingInterval: 30                                      # Optional.
Default: 30 seconds
    successfulJobsHistoryLimit: 100                      # Optional.
Default: 100. How many completed jobs should be kept.
    failedJobsHistoryLimit: 100                         # Optional.
Default: 100. How many failed jobs should be kept.
    #envSourceContainerName: {container-name}    # Optional.
Default: .spec.JobTargetRef.template.spec.containers[0]
    maxReplicaCount: 100                                # Optional.
Default: 100
    scalingStrategy:
        strategy: "custom"                             # Optional.
Default: default. Which Scaling Strategy to use.
        customScalingQueueLengthDeduction: 1            # Optional.
A parameter to optimize custom ScalingStrategy.
        customScalingRunningJobPercentage: "0.5"  # Optional.
A parameter to optimize custom ScalingStrategy.
    triggers:
    - type: azure-servicebus
        metadata:
            # Required: queueName OR topicName and
subscriptionName
            queueName: <servicebus_queue_name>
            # Required: Define what Azure Service Bus to
authenticate to with Managed Identity
            namespace: <servicebus_namespace>
            messageCount: "10" # default 5
        authenticationRef:
            name: auth-service-bus-sample-job #
authenticationRef would need either podIdentity or define
a connection parameter
```

- Now you can apply the two files to Kubernetes using:
