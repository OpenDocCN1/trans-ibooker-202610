into consideration the environment from which the person tries to access the data: they might get a different response when working from home than they do in the corporate environment.

# Identity Providers

When using RBAC or ABAC in a federated or distributed environment, identity providers (IDPs) play an important part. IDPs are external sources for retrieving attributes from the user. They offer user authentication as a service and are often used to handle sign-in processes for other systems, such as websites, CRM applications, and file servers. The best-known IDP implementations are based on either Microsoft Active Directory (AD) or OpenLDAP.

For authentication, there are currently two popular open protocols: Security Assertion Markup Language (SAML) and OpenID Connect. SAML is based on an XML standard for exchanging authorization and authentication data. OpenID Connect works similarly to SAML but is more open and is used by large companies like Microsoft, Facebook, Google, PayPal, and Yahoo. OAuth is sometimes also mentioned in this context, but it's used only for the authorization process and not for authentication. Another concept you'll hear a lot about in this space is single sign-on (SSO). With SSO, as the name suggests, users have to authenticate only once with a smart card or credentials such as a username and password, and can then access multiple applications without authenticating again.

# Real-World Example

In this section, we'll make the security architecture concrete by discussing a solution design. Figure 8-6 is the top part of an architecture that builds upon the data product architecture that we discussed in Chapter 4. It holds all the metadata and security components that are needed for the bigger picture.

The components that you see in Figure 8-6 only represent half of the complete architecture. The other half, the bottom part, is visualized in Figure 8-7. In this design, secure access to data products is virtualized. The information about what consumers are allowed to consume comes from the upper security architecture.