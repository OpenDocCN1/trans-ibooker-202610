```
containers:
  - name: shopping-cart
    image: shopping-cart
    imagePullPolicy: IfNotPresent
    ports:
      - containerPort: 80
---
kind: Service
apiVersion: v1
metadata:
  name: shopping-cart
spec:
  type: LoadBalancer
  ports:
    - name: shopping-cart
      port: 5000
      targetPort: 80
  selector:
    app: shopping-cart
```

The container image to deploy

The port the container listens for requests on

Start of the section specifying a service

Specifies this is a load balancer service

Listens on port 5000 externally

Maps to port 80 on the container

Routes requests to the shopping cart

Using this manifest to deploy and run the shopping cart in Kubernetes is as simple as running this from the command line:

```
> kubectl apply -f shopping-cart.yaml
```

This command tells Kubernetes to run everything described in the manifest, so Kubernetes will start the load balancer and the shopping cart. To check if both of these are running, we can use the command kubectl get all, which will list everything running in the Kubernetes cluster. If the deployment went well, the output from kubectl get all should be similar to this:

```
NAME                         READY   STATUS    RESTARTS   AGE
pod/shopping-cart-f4c8f4b94-4j48v   1/1     Running   0          15h

NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes       ClusterIP      10.96.0.1      <none>        443/TCP           36d
service/shopping-cart    LoadBalancer   10.103.8.64    localhost     5000:31593/TCP    6d20h

NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/shopping-cart        1/1     1            1           6d20h

NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/shopping-cart-f4c8f4b94         1         1         1       15h
```

If you prefer to have a UI, you can install and start the Kubernetes dashboard. First install the Kubernetes dashboard using this command:

```
> kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
```

At the time of writing, the Kubernetes dashboard in version 2.2.0, which is part of this command. Please consult the Kubernetes dashboard project documentation on GitHub at https://github.com/kubernetes/dashboard for the current version.