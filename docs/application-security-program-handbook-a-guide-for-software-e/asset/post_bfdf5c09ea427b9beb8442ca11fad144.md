
**DEFINITION** Reference architecture is used to provide a template and common taxonomy for a solution in a given domain. It aims to create commonality for developing a solution. Additionally, the purpose of creating reference architecture is to align the organization's strategy around a specific set of tools, standards, guidance, and implementation. This helps drive lower costs in not just licensing but also in development time and effort.

Similar to creating standards, reference architecture is a prime opportunity for security to be injected into a process whose goal it is to develop and design a common approach to solving a problem. There are several opportunities to inject security into reference architecture. Some examples are:

- Federated identity management
- Secrets management when encrypting client data
- Authorization model using OAuth

These examples provide an opportunity to bring together architecture that meets the organization's need to solve a problem, like an authorization model, but does so with the intention of building security in. Once this reference architecture is developed and approved by the ARB, it is ready to be used by the organization when various applications are looking to develop a solution. Leaning on the example of the authorization model, building this reference architecture means that each application will have the same approach to designing authorization into their application. When the reference architecture is built with security in mind, and it is reused, this cuts down on the security concerns of that application going forward. It also means that if issues are discovered or changes need to be made to the reference architecture, that those changes can be easily communicated to the impacted applications through the ARB.

It’s clear to see why this is important from a security perspective. Being able to design securely once and reuse everywhere is powerful. However, in reality there will be nuance to the adoption of that reference architecture. Not every application will be able to leverage the reference architecture verbatim and it is impractical to build a reference architecture that covers all possible scenarios. This is where local architects will need to interpret the architecture in a way that meets the spirit of the design, especially the security aspects, while applying it in the context of the application.

I’ll use our favorite organization Superior Products to illustrate this further. Superior Products has several applications that are accessed through an API gateway. The user logs in through a mobile device or other user agent to the API gateway which then provides the access to the services through an API provider. This is a less than ideal scenario since Superior Products has to manage the user credentials in a datastore. The organization would like to move to a solution that allows for decentralized authentication through a third-party identity provider.

**DEFINITION** An identity provider (IdP) is used to create, maintain, and manage identity information through a decentralized network. This distributed network provides authentication information and services.

Dashing Danielle has heard the pain points from several different applications on the management of credentials and ensuring that users are able to gain access to the API services they offer. With this in mind she sets off on some research to determine what opportunities there are to develop a pattern and reference architecture that can be used by the applications in Superior Products. She comes across several different options but lands on a solution that leverages a third party IdP solution that is popular in the industry.

> **NOTE** There are many IdP solutions on the market. One of the leaders is Okta, but there are others like Ping Identity, RSA, Microsoft, and Oracle.

Dashing Danielle works with the third party to develop a basic architecture that can be used to bring to the ARB to get buy in. The architecture consists of a simple diagram that includes an unnamed IdP, the API gateway, and the backend services.

## Simple reference architecture for API access

![](img/cc84747f93d290964598894b4f5c153b_140_0.png)

Figure 5.5 API architecture to be used as reference architecture

Dashing Danielle documents the following workflow to help establish the basic understanding of how the IdP will be used in accessing Superior Products APIs:

- A user will login through the mobile or web application where the IdP will then be used to check the credentials and issue a token.
- Another token is created for that user and app through the API gateway’s OAuth token generation service. This is used to access the application going forward.
- The API gateway and the IdP ensure that the request is within the boundaries of the user’s permissions and returns a token to the end-user application that is used to call the actual API.
- The API gateway verifies the token before serving up the requested API.

Dashing Danielle brings this architecture pattern to the ARB where, after several iterations and clarifications, it is approved as a reference architecture in the ARB. Dashing Danielle is then able to take this approved architecture to the various applications that are currently experiencing issues with managing users of their API’s. She coordinates with the chosen third-party IdP to bring the solution across the board at Superior Products.

> **NOTE** To learn more about APIs and security, check out API Security in Action by Neil Madden.

At Acme Services, things are not going as well. There is no ARB which means that there is little opportunity to coalesce around a common architecture or standard. Each product finds themselves often developing a design that resembles their peer applications but using different solutions. This drives up the cost to the organization. Because there is no ARB, two of the teams came to the same conclusion that they wanted to leverage a third-party to manage identities for them but ended up pursuing two different third parties leading to additional complexities in the organization’s architecture. Eventually, their API economy is accessed in a disjointed way that increases cost, reduces supportability, and becomes a nightmare to manage.

## 5.3.3 Bringing requirements into the organization

One of the benefits of creating the reference architecture and standards is that these will lead to the development of requirements that can be easily consumed by development teams. Where standards and reference architecture focus primarily on the strategic level, requirements bring it down to the level where the code is actually being developed. As a reminder, requirements are part of the initial steps of software development. These are used to describe how the software should behave and the various goals it is being designed to achieve.

As I have been laying out in this section, the ARB is used to approve and manage the various architecture and standards, but it does not deal with requirements as that is in the hands of the local development team, including the product owners and architects. However, the reference architecture, standards, and industry practices are used as guidelines for developing those requirements. For instance, in the case of the reference architecture described previously, there would be several requirements around the access of the API gateway, the token generation, the IdP, and others. From a security standpoint, these requirements would focus more on ensuring that the architecture is applied in a way that does not compromise the confidentiality, integrity, and availability of the application and the data. That means that, as an example, the requirements will be written to state that tokens will be send over a secure channel (for example TLS), that the tokens will be short-lived which reduces the potential exposure, and that the design is able to handle potential disruptions with the IdP.

> **EXERCISE** Using the reference architecture earlier, write a requirement that can be used to implement a portion of that architecture. Even better, if your organization already has reference architecture in place, access it and write a requirement that meets a portion of that architecture. If the team you work in is required to meet that architecture, introduce the requirement in your team.

When creating requirements, it’s important to ensure that they provide sufficient guidance on what is being asked and that, especially when they are tied to an architecture or industry standards, they meet the spirit of the overall guidance. Being able to create requirements that meet the overall framework means that the architecture can be met by
