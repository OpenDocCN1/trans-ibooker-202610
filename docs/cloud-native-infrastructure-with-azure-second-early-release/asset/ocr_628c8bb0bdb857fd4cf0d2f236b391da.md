```
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
```