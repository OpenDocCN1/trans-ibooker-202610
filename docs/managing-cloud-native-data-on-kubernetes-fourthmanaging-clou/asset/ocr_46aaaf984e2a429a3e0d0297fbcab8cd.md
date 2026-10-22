clusters. We realized that even without the intricacies of the network configuration, this was going to be a step beyond what we could implement effectively using Helm.

In the end, we realized that we were trying to make Helm do too much. It’s easy to get into a situation where you learn how to use the hammer and everything looks like a nail, but what you really need is a screwdriver. However, we don’t see Helm and operators as mutually exclusive. These are complementary approaches and we need to use each one in terms of its strengths. We continue to use Helm to perform basic installation actions including installing operators and setting up the administrator service account used by Cassandra and other components; these are the sort of actions that package managers like Helm do best.

Note: this section was adapted from the post [We Pushed Helm to the Limit, then Built a Kubernetes Operator]( ).

# Summary

In this chapter, you’ve learned how a package management tool like Helm can help you to manage the deployment of applications on Kubernetes, including your database infrastructure. Along the way you’ve also learned how to use some additional Kubernetes resources like ServiceAccounts, Secrets, and ConfigMaps. Now it’s time to round out our discussion of running databases on Kubernetes. In the next chapter, we’ll take a deeper dive into managing database operations on Kubernetes using the operator pattern.