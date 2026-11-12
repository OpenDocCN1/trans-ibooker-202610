
![](img/999fd6f5dfec4455873f6d26acc75ff0_112_0.png)

Figure 3-17. In this model, the hub acts as a central distribution point

The design of the hub is usually different from other data landing zones. It's domain-agnostic and owned by a central team responsible for overseeing which data is distributed to which other domains. Inside the hub, you typically see mainly central storage services. Inside these, data is logically organized per domain, isolated, and not integrated. The hub typically also carries (self-service) services to facilitate providing or consuming data.

With a hub, you can also apply data virtualization. So, instead of duplicating data or always provisioning data products in the hub, domains use standard services for serving out virtualized data products. Such virtualization could simplify the overall data access management or could camouflage the underlying technical complexity of a diverse landscape, if your domains are using different services.

This topology is a good option if your organization needs to control what data is distributed and consumed by different domains. It's also a useful option for managing large quantities of historical data for many large data consumers, because a hub is more efficient when sharing data across many customers. The hub model is commonly seen within the financial industry, regulated environments, and organizations that operate on a global scale and are required to have full control of all cross-regional data movements.

## Multiple Data Landing Zones

On a larger scale, multiple data landing zones are better for separating duties or concerns. Figure 3-18 shows an example of what this data architecture might look like.

![](img/999fd6f5dfec4455873f6d26acc75ff0_113_0.png)

Figure 3-18. Multiple data landing zones better allow for separation of concerns

Multiple aspects determine which functional data domains you should logically group together and make candidates for sharing a data landing zone. Let's take a look at some of the main factors:

### Security controls

Different applications might have different security profiles, requiring different control policies and mechanisms around them.

### Cohesion and efficiency in sharing data

You should consider how large data processing tasks will be managed within landing zones, because these types of workloads often require data to be stored close together.

### Regions

Regional or geographical boundaries could result in the implementation of the same business capabilities and blueprints. Multiple regions should always result in multiple landing zones.

### Isolation

A landing zone becomes a unit of security protection. Potential risks and security threats should be contained within a landing zone without affecting others. There could be different security needs that require you to isolate one landing zone from another. The same applies for data isolation. Isolating data stores to a landing zone limits the number of people that can access and manage that data store. This contains the exposure of private data and helps with, for example, GDPR compliance.

### Flexibility and pace of change

There can be differences in the velocity of innovation within domains. These disruptive changes might contrast with other domains that strongly value stability over agility.

### Value streams and business processes

Different business units or products might have different purposes and processes. You should establish different landing zones to serve business-specific needs.

### Selling off

If there's a chance that you might want to sell off or separate out some capabilities, it wouldn't be wise to integrate them tightly with shared services from other domains.

### Differences between teams

Different teams have different responsibilities and resource needs. Highly skilled and mature teams typically want to operate their own services and infrastructure.

### Cost management

Some enterprises want to implement chargeback/showback models in their cloud infrastructure to split and manage costs based on capacity that has been used.

### Limit allocation

Certain clouds have limits per subscription or account. Separating workloads into different landing zones allows you to set limits more accurately, and avoids preventing applications with different resource needs from working as intended.

### Experiments and learning

Experiments can fail, so it’s better to manage these apart from other workloads.

### Politics

Unfortunately, political boundaries can be a driving force as well.

### Service level agreements

Contrasting availability and continuity requirements could result in the usage of extra services.

Let me provide an example of how these items may influence your landing zone design. Imagine a situation where you build a new architecture in the cloud. When you deploy your first resources, you must make an explicit choice about the cloud region in which your applications will be deployed. For example, if you are based within the United States, your first landing zone and applications will be deployed in the USA region. Now imagine that after some time you need to expand your business to Europe. Because of the geographic boundary (data residency), you need to provision a second data landing zone within the European cloud region. After you’ve expanded to Europe, you land a substantial investment for innovation for the European market. Due to the nature of these disruptive and risky activities, you decide to provision a third data landing zone in which these resources will be managed apart from all your other resources. With this investment, you decide to further expand your business. This time you intend to set up a partnership structure with another company, which has a different level of trust. You decided to not integrate this partner with your existing business activities. Consequently, you set up a fourth data landing zone.

Provisioning multiple data landing zones in this manner can help you group functional domains based on cohesion and efficiency. When doing so, ensure that all your data landing zones adhere to the same auditing and controls for data management. The linking pin for all the landing zones is your management landing zone. This central logical entity should oversee what domains sit where and what data they create, distribute, and use.

Different landing zones don’t stand alone. They can be connected to hubs or other landing zones with shared data lakes or storage services. This pattern allows domains to collaborate across your enterprise. For example, you can apply shared storage services or data virtualization to allow your domains to directly read data from other domains without duplicating it.

When deploying multiple data landing zones, realize that there’s management overhead attached to each one. A typical landing zone requires configuration for accounts, linking services, linking landing zones to other zones, and so on. If you define the number of data landing zones as n, then you might end up with n network connectivity points for at least all the storage accounts and potentially other services within these zones. This could lead to an exponential increase in the number of configuration items you need to manage. The bottom line is that there is a strong relationship between the number of landing zones and the amount of configuration management overhead.

## Multiple Data Management Landing Zones

The last topology we’ll consider is for large enterprises operating on a global scale—for example, organizations that have many highly specialized departments using hundreds or thousands of applications. In such a scenario, there could be contrasting data management requirements between different parts of the organization. To tackle this, you can deploy multiple management landing zones and data landing zones together in one architecture. Such a design is visualized in Figure 3-19.

![](img/999fd6f5dfec4455873f6d26acc75ff0_116_0.png)

Figure 3-19. Large-scale enterprises may require different management landing zones

Each landing zone introduces management and integration overhead, so you should make a conscious decision about whether the extra complexity is warranted. For example, adding another management landing zone might make sense for situations where your organization’s (meta)data must not be seen by anyone outside the organization, or where different parts of the organization manage different parts of the infrastructure using their own unique ways of working, policies, security standards, data management methods, and so on. You might also want to use this topology when you have different MDM services that manage data from different parts of the organization.

If you decide to use the scaling mechanisms described in these sections, your architecture and organization can grow progressively without limitation. You can scale by adding more landing zones or domains, and you can manage complexity and costs by providing multiple blueprints. Let’s discover how this works through a concrete example.

## Practical Landing Zones Example

In Chapter 2, we discussed business capabilities and domains. We used the fictional company Oceanic Airlines as an example for learning how to organize capabilities and set boundaries. Knowing how we can use domain topologies and data landing zones to design a large-scale architecture, what would a potential architecture using landing zones look like for this hypothetical company? Let’s discover this by unpacking the design shown in Figure 3-20.
