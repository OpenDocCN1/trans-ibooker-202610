Now, this should work correctly; however, if you are getting any issues, a good place to start diagnosing these is inside the dashboard. Selecting the button highlighted in the following screenshot will display the log file for the container, including any crash reports:

| Name | Node | Status | Restarts | Age | CPU (cores) | Memory (bytes) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| salesorderservice-666bb859f-5nc64 | aks-nodepool1-40464275-1 | Running | 0 | a minute | 0 | 21.145 Mi |
| salesorderservice-666bb859f-bcnrz | aks-nodepool1-40464275-2 | Running | 0 | 24 minutes | 0 | 50.477 Mi |

If you do discover that you have any errors, you can fix them and recreate the image (jump to the Building a Docker image section to learn more). Then, you can follow along by tagging the image; however, this time, tag the image as v2. Finally, update and reapply the deployment.yaml file. Now, your new YAML file may look like this:

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: salesorderservice
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: salesorderservice
    spec:
      containers:
        - name: salesorder-process
          image: salesorderregistry.azurecr.io/salesorderprocess:v2
          ports:
            - containerPort: 80
```

When your pods are running correctly, you should be able to see the following in the pod status:

```
kubectl describe pod
```