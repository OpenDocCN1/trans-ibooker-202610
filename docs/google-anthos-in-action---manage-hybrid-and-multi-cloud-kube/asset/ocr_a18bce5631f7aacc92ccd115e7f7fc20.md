The next section contains the solution to address Evermore’s requirements. You can follow along with the solution, or if you are comfortable, configure your cluster to address the requirements and use the solution to verify your results.

# Evermore Industries Solution - Testing the Current Security

# Meets requirements: 1

The first requirement requires you to document any security concerns with the current cluster. To test the first three security requirements, you can deploy a manifest that attempts to elevate the privileges of a container. The test manifest should pull an image from a registry that is not on the approved list and set the fields to elevate privileges and the various host values. We have provided an example manifest below.

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: hack-example
  name: hack-example
spec:
  hostPID: true
  hostIPC: true
  hostNetwork: true
  volumes:
  - name: host-fs
    hostPath:
      path: /
  containers:
  - image: docker.io/busybox
    name: hack-example
    command: ["/bin/sh", "-c", "sleep infinity"]
    securityContext:
      privileged: true
      allowPrivilegeEscalation: true
    volumeMounts:
    - name: host-fs
      mountPath: /host
```

This manifest will test all the security requirements in a single deployment.