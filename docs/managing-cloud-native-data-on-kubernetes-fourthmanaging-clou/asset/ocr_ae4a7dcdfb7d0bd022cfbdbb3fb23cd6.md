“metadata storage” vs “data storage”. These StorageClasses will make different cost/performance tradeoffs and provide specific service level agreements (SLAs). We may even see some standard definitions start to emerge for these new StorageClasses.

Ideally, application teams should not be picking into what storage solutions are chosen. The only thing an application developer should be concerned with is specifying PersistentVolumeClaims for their application, with the StorageClasses they need. The other details of managing storage should be hidden, although of course the storage subsystem will report errors including status and logs via the standard Kubernetes mechanisms. This capability will make things a lot simpler for application developers, whether they’re deploying a database, or some other stateful workload.

These innovations will guide us to a more optimal place with storage on Kubernetes. Today we’re in a place where deploying infrastructure is easy. Let’s work together to get to a place where deploying the right infrastructure is easy.

As you can see, storage on Kubernetes is an area in which there is a lot of innovation, including multiple open source projects and commercial vendors competing to provide the most usable, cost effective, and performant solutions. The Cloud-Native Storage section of the CNCF Landscape provides a helpful listing of storage providers and related tools, including the technologies referenced in this chapter and many more.

# Summary

In this chapter, we’ve explored how persistence is managed in container systems like Docker, and container orchestration systems like Kubernetes. You’ve learned about the various Kubernetes resources that can be used to manage stateful workloads, including Volumes, PersistentVolumes, PersistentVolumeClaims, StorageClasses. We’ve seen how the Container Storage Interface and Container Attached Storage pattern point the way