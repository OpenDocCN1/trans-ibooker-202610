To attach each of these replicas to the load balancer (Kubernetes Service) pool, Kubernetes uses a probe called "Readiness Probe" (https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/) to make sure that the container running inside the Pod is ready to accept incoming requests, in other words, it has finished bootstrappingsg. In figure 5.2, Pod C is not ready yet, hence it is not attached to the Service pool, and no request has been forwarded to this instance yet.

Now, if we want to upgrade to using the `my-app:1.1` container image, we need to perform some very detailed orchestration of these containers. Suppose we want to make sure that we don't lose any incoming traffic while doing the update. We need to start a new replica using the `my-app:1.1` image and make sure that this new instance is up and running and ready to receive traffic before we remove the old version. If we have multiple replicas, we probably don't want to start all the new replicas simultaneously, as this will cause doubling up all the resources required to run this service.

We also don't want to stop the old `my-app:1.0` replicas in one go. We need to guarantee that the new version is working and handling the load correctly before we shut down the previous version that was working fine. Luckily for us, Kubernetes automates all the starting and stopping of containers using a rolling update strategy (https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/).

# 5.1.1 Rolling updates

Deployments and StatefulSets come with these mechanisms built-in, and we need to understand how these mechanisms work to know when to rely on them and their limitations and challenges.

A rolling update consists of well-defined checks and actions to upgrade any number of replicas managed by a Deployment. Deployment resources orchestrate ReplicaSets to achieve rolling updates, and the following figure shows how this mechanism works: