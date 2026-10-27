```
selector:
  app: foo
ports:
- name: web
  protocol: TCP
  port: 80
  targetPort: 80
clusters:                      #A
- link: "europe-west1-c/gke-eu"      #B
- link: "asia-northeast1-a/gke-asia-1"      #B
```

# # MulticlusterIngress

```
apiVersion: networkin.g.gke.io/v1alpha1
kind: MultiClusterIngress
metadata:
  name: foobar-ingress
  namespace: blue
spec:
  template:
    spec:
      backend:
        serviceName: default-backend
        servicePort: 80
      rules:
      - host: foo.example.com
        backend:
          serviceName: foo      #A
          servicePort: 80
      - host: bar.example.com
        backend:
          serviceName: bar      #A
          servicePort: 80
```

Note that the MulticlusterService includes a cluster selector stanza at the bottom. Removing this sends Ingress traffic to all member clusters. Adding a cluster selector may be useful if you want to remove MCI traffic from a specific cluster (or clusters), for example, if you are performing upgrades or maintenance to a cluster. If the clusters stanza is present in the MulticlusterService resource, Ingress traffic is sent to only the clusters available in the list. Clusters are explicitly referenced by <region | zone>/<name>. Member clusters within the same environ and region should