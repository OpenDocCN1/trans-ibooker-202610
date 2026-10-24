+   - *Obstacles to refactoring the code for the business capability*—This can occur because the code is spread across the code bases for the two microservices. Such cross-code base refactorings are difficult because it’s hard to get a complete picture of the consequences of the refactoring and because tooling support is poor.
+   - *Difficulty deploying the two microservices independently*—After refactoring or implementing a feature that involves both microservices, the two microservices may need to be deployed at the same time or in a particular order. Either way, coupling between versions of the two microservices violates the characteristic of microservices being individually deployable. This makes testing, deployment, and production monitoring more complicated.

These costs are incurred from the time the microservices are first created until you’ve gained enough experience and knowledge to more correctly identify the business capability and a better scope for a microservice (the entire special offers business capability, in this case). Added to those costs is the fact that the difficulty in refactoring and implementing changes to the business capability will result in you doing less of both, so it will take you longer to learn about the business capability. In the meantime, you pay the cost of the duplicated data and data model and the cost of the lack of individual deployability.

We’ve established that preferring to err on the side of too narrow a scope easily leads to scoping microservices in a way that creates costly coupling between the microservices. To see if this is better or worse than erring on the side of too big a scope, we need to look at the costs of that approach.

If you err on the side of bigger scopes, you might decide on a scope for the special offers microservice that also includes handling coupons. The scope of this bigger special offers microservice is shown in figure 4.14.

![](bbox=[0.18, 0.62, 0.9, 0.88])

Figure 4.14 If you choose to err on the side of bigger scopes, you might decide to include the handling of coupons in the special offers business capability.