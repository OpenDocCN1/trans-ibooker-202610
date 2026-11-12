![](bbox=[0.142, 0.083, 0.856, 0.417])

Figure 8-8. Different methods of addressing shared data ownership

The method of encapsulating metadata for security is most effective when data security is fine-grained—that is, when you have many owners and complex security scenarios to manage. To address the overhead, it is recommended to implement a data ownership delegation model to lower the burden of requiring all data owners to individually approve requests. So, for example, if a consumer requests a single data product that has 20 owners, only one data owner has to give approval.

This discussion has focused on securing data product architectures, but security doesn't end there. In the next sections, we’ll examine the concerns specific to securing API- and event-based architectures.

# Securing API-Based Architectures

Securing APIs, for example within SOA, is complex because communication occurs in real time. Security typically happens on two levels: at the API layer and at the API provider.

The first level of security is implemented at the API layer using API gateways, enterprise service buses, and service meshes. As seen in Figure 8-9, API consumers first have to prove their identities by calling specific authorization services. After successful identification, an identity or access token is provided (and, optionally, a refresh token), which allows API consumers to make authorized API calls using