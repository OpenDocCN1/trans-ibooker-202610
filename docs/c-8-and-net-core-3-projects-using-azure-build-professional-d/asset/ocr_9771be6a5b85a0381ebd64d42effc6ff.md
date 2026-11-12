# # Cleanup

A Kubernetes cluster can be an expensive business. To be clear, it’s usually much cheaper than buying and running the services yourself; however, if you’re playing around with these things, you can quickly run up a hefty bill. To remove the resources that we’ve created in this chapter, let’s start with the cluster:

+   1. Start by opening the Kubernetes services blade and selecting your cluster:

![](bbox=[0.106, 0.272, 0.892, 0.588])

This is definitely not a fast process, but once it’s complete, it should remove the virtual machines that were created, too.

+   2. Next, open the Storage account blade and find the account that you created for the storage queue: