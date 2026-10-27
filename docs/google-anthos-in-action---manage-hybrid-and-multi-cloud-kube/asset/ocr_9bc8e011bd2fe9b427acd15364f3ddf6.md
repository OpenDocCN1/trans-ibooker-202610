traffic to a service running a sidecar makes it insecure - nullifying the advantages of Istio and the sidecar. Once you are comfortable with Istio, you may want to consider changing the permissive policy to the more secure strict setting.

You can force strict mTLS for the entire mesh, or just certain namespaces by creating a Kubernetes object, called PeerAuthentication. Deciding on the correct scope for mTLS is different for each organization and cluster. You should always test any mTLS policy changes in a development environment before implementing them in production, to avoid any unexpected application failures.

Since this is an important policy, it’s a perfect example to demonstrate the importance of using ACM as a configuration management tool. Remember that once an object is managed by ACM, the configuration manager will control it. This means that the manager will re-create any managed object that is edited or deleted for any reason. For the mTLS use case, you should see the importance of using ACM to make sure that the policy is set, and if edited, remediated to the configured strict value.

To enable a strict mTLS mesh wide policy, you need to create a new PeerAuthentication object that sets the mTLS mode to strict. An example manifest is shown below.

```
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: istio-system
spec:
  mtls:
    mode: STRICT
```

The manifest assumes that Istio has been installed in the istio-system namespace. SInce the namespace selector is the istio-system namespace, it will enforce a strict mTLS policy for all namespaces in the cluster.

# Note:

To enforce a strict mTLS policy for every namespace in the cluster, the