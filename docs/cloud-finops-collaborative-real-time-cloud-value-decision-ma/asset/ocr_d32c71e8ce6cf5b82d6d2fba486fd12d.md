# Usage Reduction by Removing/Moving

Research shows that there are two types of cloud teams: those that forget about some of their resources, and those that fib about forgetting about some of their resources. In other words, it is very common for resources to be forgotten about and left in cloud accounts. Don’t assume bad intent or carelessness when you find the same in your company. The job of engineering has many competing priorities, and automated tooling to detect waste is not always prioritized by leadership. In other words, it happens to the best of us.

![](bbox=[0.15, 0.28, 0.24, 0.38])

Find waste by looking for areas with very stable costs and that use dated resources. “Old and unchanging” is not how the cloud should operate. If it’s been sitting there untouched for a long time, it likely can be optimized.

Simply telling teams not to forget about resources doesn’t cover all the bases. It’s possible a resource was created by automation that wasn’t configured to remove it when instructed to do so. Or the resource was intentionally left there by the team until an unspecified later date, but no follow-up action was taken as priorities shifted. Additionally, some resources can be configured to remain when their parent resource gets deleted, like volumes attached to a server, and when deleting some resources, the cloud service provider can automatically create a snapshot of the data that sticks around whether you need it or not.

A lost or forgotten resource is the easiest type for teams to handle. Usage reduction in such cases can be as simple as deleting the resource, saving 100% of the cost of that resource.

Another conversation we often hear is around the need to keep data stored inside a resource. Usually, teams have compliance reasons to retain data for very long periods.

Even if you need to retain an unused volume for this purpose, there are ways to reduce what you are paying. For example, cloud service providers offer multiple storage tiers, and paying for data in higher-price storage offerings makes no sense when you can move it to a lower-price “cold storage” resource like Azure Archive or AWS Glacier storage.

Consider creating a data retention policy that makes it clear to teams what data they need to keep and for how long. Once a retention policy is in place, teams can automatically move data to different storage offerings and delete it when appropriate.