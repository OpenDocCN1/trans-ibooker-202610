This process of consolidating identities and authentication methods is called *federation*. There are two common ways to use federation for embedded analytics:

# *Service provider–initiated flow*

In an SP-initiated flow, when a user attempts to access protected resources they are redirected to the identity provider's login page. This method allows users who already have accounts in both systems to quickly log in to applications without having multiple sets of usernames and passwords.

In embedded analytics, where the analytic system is the service provider and the host application the identity provider, this scenario is most useful when the business has some standalone analytics applications but also uses the same vendor's features for embedding.

# *Identity provider–initiated flow*

In an IdP-initiated flow—the preferred option for embedded analytics—when a user attempts to access protected resources from the host application they are redirected to the service provider's login page, where they can enter credentials and authenticate with SAML.

Even better for the user, an identity provider can use an API to log in to the service provider on their behalf, with all the necessary information (including signed security tokens) needed for them to authenticate via SAML on the SP side. This results in higher user adoption and satisfaction because it feels more natural to the user and enables the embedded analytics to feel like a complete component of the overall system.

In general, look for a system that works with identity providers that support SAML 2.0, such as Okta, Auth0, or Active Directory Federation Services (AD FS). Some enterprise applications, such as Salesforce, have their own identity providers that enable your users to use their logins for those applications to interact with your embedded systems.