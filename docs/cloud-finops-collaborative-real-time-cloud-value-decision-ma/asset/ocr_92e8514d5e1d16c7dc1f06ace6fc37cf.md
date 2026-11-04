![](bbox=[0.15, 0.08, 0.25, 0.18])

The value of tags *does* extend outside the scope of FinOps, and understanding these benefits can be useful when a business is trying to implement a tagging standard. For the security team, tags can identify the service and team responsible for resources affected by a possible security issue. Operations teams are easily able to determine what services will be impacted by a notification of a resource having issues. A good tagging policy in an organization will pay dividends for many different stakeholders.

The more thought-out a tagging standard is at the start, the less likely a future revision will be required. If you need to go back to a team and ask them to change a bunch of tags after they just finished rolling out your previous strategy, you’re not going to make any friends.

As a business scales out on the cloud, it will be generating thousands, if not millions, of resource charges, each with its own tags. Changing a tagging strategy means that teams will have to update all of these resources. As tags are commonly applied by automation infrastructure like Terraform, CloudFormation, and Azure Resource Manager (ARM) templates, updates to automated tags will need to be rolled out in code. Companies often find that many resources cannot be updated, and they end up having half-implemented standards.

# Picking the Right Number of Tags

A tagging standard shouldn’t force teams to apply too many tags. Requirements for an excessive number of tags will only cause resistance to the standard, which will lead to noncompliance. Always keep in mind the questions that you need to answer, and balance your tag strategy against the data you can get from your account hierarchy. It’s best to aim to require tags only for the core metadata needed. Instead of defining all tags as required, define optional tags that clearly describe how your teams should implement them if they choose to do so.

If you’re unsure of where to start with a tagging standard, here are some tags that companies with successful FinOps implement:

+   - A *cost center/business unit* tag that clearly defines where the costs of the resource should be allocated within your organization.
+   - A *service/workload name* tag that identifies which business service the resource belongs to, allowing the organization to differentiate costs among the services that teams are running.
+   - A *resource owner* tag to help identify the individual/team responsible for the resource.