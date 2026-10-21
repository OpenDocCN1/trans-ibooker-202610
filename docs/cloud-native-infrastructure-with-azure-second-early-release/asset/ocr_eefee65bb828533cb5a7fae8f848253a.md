```
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

+   - Now you can apply the two files to Kubernetes using: