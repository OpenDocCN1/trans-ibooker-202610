“jack of all trades, master of none” model, we can build multiple models, each focused on solving a specific problem.

As a project evolves and grows, it’s not uncommon for the bounded contexts to lose their focus and accumulate logic related to different problems. That’s accidental complexity. As with subdomains, it’s crucial to revisit the bounded contexts’ boundaries from time to time. Always look for opportunities to simplify the models by extracting bounded contexts that are laser focused at solving specific problems.

Growth can also make existing implicit design issues explicit. For example, you may notice that a number of bounded contexts become increasingly “chatty” over time, unable to complete any operation without calling another bounded context. That can be a strong signal of an ineffective model and should be addressed by redesigning the bounded contexts’ boundaries to increase their autonomy.

# Aggregates

When we discussed the domain model pattern in Chapter 6, we used the following guiding principle for designing aggregates’ boundaries:

> The rule of thumb is to keep the aggregates as small as possible and include only objects that are required to be in a strongly consistent state by the business domain.

As the system’s business requirements grow, it can be “convenient” to distribute the new functionalities among the existing aggregates, without revisiting the principle of keeping aggregates small. If an aggregate grows to include data that is not needed to be strongly consistent by all of its business logic, again, that’s accidental complexity that has to be eliminated.

Extracting business functionality into a dedicated aggregate not only simplifies the original aggregate, but potentially can simplify the