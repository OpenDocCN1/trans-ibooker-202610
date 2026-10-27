![](bbox=[0.117, 0.097, 0.865, 0.572])

In summary, securing the actions that a container may be able to take on the host are controlled by the user that is running in the container and whether or not the container is allowed to run as a privileged container. To secure a cluster, you will need to create a policy that defines controls for each of these values.

Right now, you know why containers should not be allowed to run as root and why you should limit pods that are allowed to run as a privileged container, but we haven’t explained how to stop either of these actions from occurring on a cluster. This is an area that Anthos excels in! By providing Anthos Config Manager, Google has included all the tools you need to secure your cluster from these and many other common security settings.

In the next section, we will explain how to use ACM to secure a cluster using the included policy manager, Gatekeeper.