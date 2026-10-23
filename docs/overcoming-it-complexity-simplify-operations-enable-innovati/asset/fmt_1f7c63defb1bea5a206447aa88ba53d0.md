
Centralized truth facilitates the use of standardized best practices, reuse patterns, and layered configurations in a more consistent manner, allowing a more dramatic reduction in application complexity. While accessing a single configuration source from all parts of the application may not always be the most convenient solution, using this model more strongly encourages consistency and reuse than a simple single-source-of-truth model.

### Revision management

Once the configuration is centralized, you can use revision control to manage it. `Git`, the tool used by software engineers worldwide to manage software source code, can just as easily be used to maintain configuration and system files. Doing so gives you all the benefits of revision management that apply to source code:
* Configurations can be backed up and maintained consistently in multiple redundant locations.
* Updated configurations can be compared to previous configurations to make sure that only the desired changes have been made and no unnecessary, undesired, or incorrect changes have accidentally been included.
* Configuration changes can be peer reviewed before they are deployed.
* Full revision control and approval workflows can be implemented to ensure that only correct and desired changes occur.
* If a decrease in performance or reliability is observed, revision history allows you to go back and see what changed at the point in time when the problem occurred, as an aid in diagnosing how to resolve the issue.
* With proper processes in place, it becomes difficult for bad actors to make configuration changes, and when they are made, the changes are easy to identify and resolve.

## Are You in Control?

You might be thinking that this all sounds obvious. Of course you don’t store configuration information exclusively in the devices themselves. You keep backups and ensure redundancy, and you use software provided by the network resource vendors to manage your network resources. This is best practice, right?

Well, you still might be more exposed than you think. For example, do you maintain off-site copies and single-source-of-truth versions of the following configuration files:
* Apache configuration files
* `/etc/hosts` files on your Linux servers
* `/etc/sysconfig/network-scripts/*` network scripts on Linux servers
* AWS IAM policies for the thousands of AWS components you use
* Tuning variables for your cloud-hosted MySQL database

You might be surprised by the breadth, scope, and interdependence of configuration information your complex application requires. Understanding where all your configuration requirements exist and how best to centralize them will require a complete audit and assessment of your application—the type of audit and assessment we discussed in [Chapter 2](#ch02.html#auditing_and_assessing_your_it_ecosystem). Once you have done this assessment, you’ll understand where your configuration-based knowledge is stored, and you can create a strategy to manage it in a centralized single source of truth.

# Reuse

Centralized knowledge management facilitates reuse. How so? Suppose all your configuration files for your applications, devices, services, and components are in one location. In that case, you can easily leverage information from one configuration in another, perhaps for a similar device. Reuse reduces reinvention, making it less likely that multiple distinct solutions to the same problem are employed. This limits variability in design and hence decreases the amount of information needed to understand how the system as a whole is operating, reducing complexity and cognitive load.

There are multiple patterns for implementing reuse, each with a distinct set of advantages and disadvantages. I will focus on two of them here.

## Pattern 1: Copy/Paste Reuse

With the copy/paste reuse model, a system, infrastructure, or security engineer or architect looking for a configuration or design pattern to accomplish a particular task searches through the pool of existing systems for an implementation of a design pattern that appears to fit their needs. They copy the design pattern into their own system, make any adjustments as needed, and they’re done! They’ve leveraged an existing configuration to create a new configuration, relying on reuse to make sure the new component is configured as similarly as possible to the existing component.

**Advantages:** Copy/paste reuse reduces the time it takes to create new configurations by leveraging known, working configurations. In addition to reducing time to completion, this approach reduces errors in the initial implementation by reusing a working implementation.

**Disadvantages:** Once the configuration has been copied, it no longer tracks changes in the original configuration. The new configuration and the old configuration can drift apart over time, and eventually may no longer be similar. This tends to cause complexity to increase little by little as time passes.

## Pattern 2: Layered/Template Reuse

With the layered reuse model, rather than copying a configuration from one component to another, you create a shared layer of configuration that is common between the two. This *configuration template* is shared by both components. If a change is later made to that template, it propagates down and is automatically included in all configurations based on the shared layer, keeping them consistent.

**Advantages:** This has all the advantages of the copy/paste reuse model but eliminates the risk of slow divergence and increasing complexity over time, keeping the different configurations in sync with each other.

**Disadvantages:** More work is required both to create the shared layers/templates and to set up the necessary automation to use them in any given configuration, including syncing the changes automatically. Additionally, layered reuse introduces a danger: since a shared template is used in many places, a change to it for a given purpose may have unknown and undesired side effects in other places. Change management, change versioning, version pinning, continuous integration/continuous delivery (`CI/CD`) tooling, and change reviews can help mitigate this issue and ultimately create an environment where there is substantially less complexity because there is significantly more commonality and reuse among similar configurations.

# Example: Router Configuration Files

A good example of the power of centrally managed configuration and reuse is the management of network router configurations.

A network router is used to route traffic from one spot in a network to another. Which traffic it allows to pass through can vary because of built-in firewalls, security profiles, network shaping, and routing and redirection rules. Additionally, where traffic is routed may change based on the type of traffic and the desired destination. Each of these routers has a configuration that dictates what rules the device must follow in how it routes traffic. Managing these rules can be extremely complicated, especially in a large enterprise that may have hundreds, thousands, or even tens of thousands of routers, switches, and other networking infrastructure components, all working together to create a safe, functional, secure environment for applications to operate in.

When a network has only a few of these routers, they can be managed by simply updating the configuration on the routers themselves. Most low-end routers even have web-based setup pages that allow an engineer to update the router’s configuration on the fly; your home network probably has at least one networking device (perhaps a WiFi router) that offers such a configuration option.
