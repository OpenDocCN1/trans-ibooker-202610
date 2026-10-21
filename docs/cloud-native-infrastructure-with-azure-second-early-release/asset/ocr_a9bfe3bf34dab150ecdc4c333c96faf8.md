```
kind: ConfigMap
apiVersion: v1
metadata:
  name: kubernetes-services-endpoint
  namespace: tigera-operator
data:
  KUBERNETES_SERVICE_HOST: "<API server host>"
  KUBERNETES_SERVICE_PORT: "<API server port>"
```

+   - Restart the operator to pick up the change:

```
$ kubectl delete pod -n tigera-operator -l k8s-app=tigera-operator
```

+   - Disable the deployment of kube-proxy (to save resources):

```
$ kubectl patch networks.operator.openshift.io cluster --type merge -p '{"spec":{"deployKubeProxy": false}}'
```

+   - Using calicoctl, enable eBPF:

```
$ calicoctl patch felixconfiguration default --patch='{"spec": {"bpfEnabled": true}}'
```

If you have issues running eBPF, Calico provides a troubleshooting guide.

NOTE
If you are using eBPF and have a kernel version later than 4.16, Calico will automatically attempt to use XDP to process packets, which will provide performance improvements. This is useful for high-throughput services or services that are being DOSed.

# Implementing Calico Security Policy