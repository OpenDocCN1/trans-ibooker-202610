![](bbox=[0.145, 0.085, 0.855, 0.34])

Figure 3-19. Large-scale enterprises may require different management landing zones

Each landing zone introduces management and integration overhead, so you should make a conscious decision about whether the extra complexity is warranted. For example, adding another management landing zone might make sense for situations where your organization’s (meta)data must not be seen by anyone outside the organization, or where different parts of the organization manage different parts of the infrastructure using their own unique ways of working, policies, security standards, data management methods, and so on. You might also want to use this topology when you have different MDM services that manage data from different parts of the organization.

If you decide to use the scaling mechanisms described in these sections, your architecture and organization can grow progressively without limitation. You can scale by adding more landing zones or domains, and you can manage complexity and costs by providing multiple blueprints. Let’s discover how this works through a concrete example.

# Practical Landing Zones Example

In Chapter 2, we discussed business capabilities and domains. We used the fictional company Oceanic Airlines as an example for learning how to organize capabilities and set boundaries. Knowing how we can use domain topologies and data landing zones to design a large-scale architecture, what would a potential architecture using landing zones look like for this hypothetical company? Let’s discover this by unpacking the design shown in Figure 3-20.