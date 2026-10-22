# Things all companies do in the same way

In the previous chapters, you saw that the type of subdomain at play affects strategic and tactical design decisions:

+   - How to design the bounded contexts’ boundaries
+   - How to orchestrate integration between the contexts
+   - Which design patterns to use to accommodate the complexity of the business logic

To design software that is driven by the business domain’s needs, it’s crucial to identify the business subdomains and their types. However, that’s not the whole story. It’s equally important to be alert to the evolution of the subdomains. As an organization grows and evolves, it’s not unusual for some of its subdomains to morph from one type to another. Let’s look at some examples of such changes.

# Core to Generic

Imagine that an online retail company called BuyIT has been implementing its own order delivery solution. It developed an innovative algorithm to optimize its couriers’ delivery routes and thus is able to charge lower delivery fees than its competitors.

One day, another company—DeliverIT—disrupts the delivery industry. It claims it has solved the “traveling salesman” problem and provides path optimization as a service. Not only is DeliverIT’s optimization more advanced, it is offered at a fraction of the price that it costs BuyIT to perform the same task.

From BuyIT’s perspective, once DeliverIT’s solution became available as an off-the-shelf product, its core subdomain turned into a generic subdomain. As a result, the optimal solution became available to all of BuyIT’s competitors. Without massive investments in research and development, BuyIT can no longer gain a