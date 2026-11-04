+   - A *name* tag to help identify the resource using a friendlier identifier than the one given to you by your cloud services provider.
+   - An *environment* tag to help determine cost differences among development, test/staging, and production.

We recommend using an account hierarchy in place of the most important one or two tags—say, cost center and environment—and then using tags for the remaining three to five allocations that must be tracked. A coarse-grained account hierarchy provides for easy mapping to a few dimensions, with tags providing the finer details of additional business dimensions.

![](bbox=[0.16, 0.3, 0.25, 0.38])

The most mature FinOps practices use tags to connect cloud resources to other sources of information, such as configuration management databases (CMDBs). This both enables large amounts of data to be referenced and allows quicker/simpler processes of updating information once and not having to correct potentially millions of tags on cloud resources.

# Working Within Tag/Label Restrictions

It’s best to ensure that your planned tagging policy (of required keys and values) will work across all cloud service providers you plan on using. Some cloud service providers’ services allow almost any character, while others are stricter. Tags can also have limits on their length, and services have restrictions on the number of tags supported per resource.

You should ensure that the services you intend to use will work with your proposed policy. Otherwise, you may end up having to change your tags at some point, or you may end up with multiple groupings with slightly different values.

For example, you may start tagging your resources with R&D, but then realize later that a particular service does not support the & character. This leaves you with two options: to go back and retag all the existing R&D resources as something like RandD, or to end up having both R&D and RandD values in your bill and your costs split across the two groupings. Some FinOps tooling offers ways to fix inconsistent tags after the fact by allowing you to apply rules to adjust how resources are allocated in their reports, without requiring changes to the raw cloud spend data.

Some cloud service providers allow you to tag more of their resource types than others do, which can be a common complaint from staff members tasked with tagging resources. The important thing here is to realize the benefit for all the resources that support tagging, and to continually push your cloud service providers to add more tag support to their resource types.