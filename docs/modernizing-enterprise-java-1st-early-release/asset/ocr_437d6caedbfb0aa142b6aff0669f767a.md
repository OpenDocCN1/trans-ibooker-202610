Deploying function to the cluster
Function deployed at URI: http://quarkus-faas.coolstore.192.168.39.69.nip.io

# Finally start your Quarkus function on Kubernetes!

```
```bash
$ curl \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"message":"Hello FaaS on Kubernetes!"}' \
  http://quarkus-faas.coolstore.192.168.39.69.nip.io
```
```

You should get an output similar to this:

```
```json
{"message":"Hello FaaS on Kubernetes!"}
```
```

You can verify that a new pod has started in your Kubernetes cluster inside
the `coolstore` namespace:

```
```text
kubectl get pods -n coolstore
NAME                                      READY   STATUS    RESTARTS   AGE
curl                                      1/1     Running   3          9d
quarkus-faas-00001-deployment-5b789c84b5-kc2jb  2/2     Running   0          80s
```
```

And, you should see that a new Knative Service has been created:

```
```text
kubectl get ksvc quarkus-faas -n coolstore
NAME           URL
LATESTCREATED  LATESTREADY       READY  REASON
... 
quarkus-faas   http://quarkus-faas.coolstore.192.168.39.69.nip.io
quarkus-faas-00001  quarkus-faas-00001  True
```
```

You can now see all the details of your newly deployed function with the
following command:

```
```bash
kubectl describe ksvc quarkus-faas -n coolstore
```
```