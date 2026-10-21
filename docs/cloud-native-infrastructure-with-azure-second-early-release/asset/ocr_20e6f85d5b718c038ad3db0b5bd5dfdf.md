+   - Now create a file called *secret.yaml*. Replace the `servicebus-queue` and `servicebus-connectionstring` values with the Base64 strings generated in the previous step.

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

+   - Create a second file, called *keda.yaml*, which will create your scaled job. It will be running a docker image (`docker.pkg.github.com/prabdeb/sample-python-keda-service-bus-scaler/consumer:latest`) when triggered:

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
```