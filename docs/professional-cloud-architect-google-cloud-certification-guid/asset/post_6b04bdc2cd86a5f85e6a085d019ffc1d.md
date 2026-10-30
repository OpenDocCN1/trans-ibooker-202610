
The following diagram shows what the customer is expected to manage in each service model. From left to right, we can see the difference between fully self-managed on-premises data centers and SaaS offerings:

![](img/de2c97ab017f36b3750a5bade111edd7_469_0.png)

Figure 15.1 – Shared responsibility model

Now that we have introduced security at a high level, let's move on to understanding Cloud Identity in GCP.

## Cloud Identity

Cloud Identity is a key GCP service that's offered by Google as an Identity-as-a-Service (IDaaS) solution. This is an optional service and a more basic approach can be taken to apply individual permissions to services or projects. However, for enterprise organizations, using an identity service makes far more sense. It enables businesses to manage who has access to their resources and services within a GCP organization, all from a single pane of glass. Cloud Identity can be used as a standalone product for domain-based user accounts and groups. We should note early on in this chapter that Google also offers a similar management pane for Google workspace users. The console works in the same way, and user and domain management have a lot of similarities. You may wish to investigate this further in detail, but for this book and, more importantly, the exam, we will focus solely on GCP Cloud Identity.

Like many on-premises directories that can reduce the overhead of user administration, GCP allows us to centrally manage our users in the cloud through Cloud Identity. It offers a central, single pane of glass to administer users, groups, and settings and is referred to as the Google Admin console. To utilize Cloud Identity, your domain name should be enabled to receive emails, hence allowing your existing web and email addresses to be used as normal. Your GCP organization will have a single Cloud Identity, and the organization is the root node in the resource hierarchy. It is deemed the supernode of your projects.

It should be noted that there are two editions of Cloud Identity. One is free and one is a premium subscription. The premium subscription offers additional features, such as device management and security and application management. A full comparison can be found here: https://support.google.com/cloudidentity/answer/7431902?hl=en. Finally, you must understand that Cloud Identity is not controlled through the GCP console, but in fact through the Google Admin console. Once you're logged into the admin console, you will be able to add users, create groups, and assign members and disable users.

The following is a screenshot of what the **Google Admin** console page looks like:

![](img/de2c97ab017f36b3750a5bade111edd7_470_0.png)

Figure 15.2 – Google Admin console

Adding users manually is classed as overhead in terms of effort and is not scalable if you have a large environment to manage. The whole point of Cloud Identity is to simplify the management of users and groups as it's unlikely that an organization can afford one person to add, remove, and manage things. This is where Google Cloud Directory Sync (GCDS) comes in. Many organizations will have an LDAP database such as Microsoft Active Directory (AD). GCDS can synchronize an organization's AD or LDAP database onto Cloud Identity, and it is highly scalable. Synchronization is only one-way – that is, from on-premises to GCP –so your on-site database is never compromised. GCDS allows the administrator to perform delta syncs and scheduled synchronizations and perform tasks manually. If we require permissions to be revoked from a user, or indeed disabled, then the results are immediate.

When we start to use Cloud Identity for authentication, Google stores and manages all authentications and passwords by default, but there is an option to disable this. Two-step verification can be added to this by using multi-factor tools. There is also an alternative to using Single Sign-On (SSO), which is a SAML 2.0-based authentication that also includes multi-factor authentication (MFA). Finally, password complexity can be set within Cloud Identity's password management feature to align with existing policies your business may have:

![](img/de2c97ab017f36b3750a5bade111edd7_471_0.png)

Figure 15.3 – Password management

In the preceding screenshot, we can see an example of the Password management screen. In the next section, we will look at GCP Resource Manager.

## Resource Manager

GCP Resource Manager allows you to create and manage a hierarchical grouping of objects such as organizations, folders, and projects together. Let's look at an example where we have an organization, brgerrard.co.uk, and several folders underneath that to add to the structure. Folders are optional but can be used to group projects. Access to these folders will work on a hierarchical model, meaning that if you have full access to the Departments folder, then this will be inherited down to, for example, the Google Cloud Architect Project folder, as shown in the following screenshot:

![](img/de2c97ab017f36b3750a5bade111edd7_472_0.png)

Figure 15.4 – Resource Manager

A good example to think of would be the separation of development, testing, and production environments in GCP. Separate projects for each environment allow you to grant access to only those who need access to the resources. The following screenshot shows how the hierarchy of GCP is set up:

![](img/de2c97ab017f36b3750a5bade111edd7_473_0.png)

Figure 15.5 – GCP hierarchy (Source: https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy, License: https://creativecommons.org/licenses/by/4.0/legalcode)

It's important to understand that setting permissions at a higher level results in permission inheritance. For example, granting a user permission to the Dept Y folder means that the user will also have the same permissions applied to all subfolders and projects. Therefore, it is important to design your organization and permissions in such a way that they only grant access to the resources that are required to fulfill the requested role.

In the next section, we will look at Identity and Access Management.

## Identity and Access Management (IAM)

GCP offers us the ability to create GCP resources and manage who can access them. It also allows us to grant only the specific access that's necessary, to prevent any unwanted access. It allows us to meet any requirements for the separation of duties. This is known as the security principle of least privilege, and we will look at this in detail shortly. First, we will have a look at some key concepts of IAM. In Cloud IAM, we can grant access to members. Members can belong to any one of the following types:

- **Google accounts:** These represent someone who interacts with GCP, such as a developer.
- **Service accounts:** These belong to your application and not an end user. We will look at service accounts in more detail later in this chapter, in the *Service accounts* section.
- **Google groups:** These are named collections of Google accounts and service accounts and are a good way to grant an access policy to a collection of users. A Google group can be used with IAM to grant access to roles. One important exception is that a group can only be assigned the owner role of a project if they are part of the same organization.
- **Google Workspace Domain:** This domain represents a group of all of the Google accounts that have been created in an organization's Google Workspace (Formerly G Suite).
- **Cloud Identity Domain:** Similar to Google Workspace, this domain represents all of the Google accounts in an organization.

The following are some of the concepts that are related to access management:

- **Resources:** Resources are projects, Compute Engine instances, or Cloud Storage buckets.
- **Permissions:** A permission dictates what operations are allowed on a resource and are seen in the form of <service>.<resource>.<verb>; for example, `compute.instance.list`. Permissions cannot be assigned directly to a user.
- **Roles:** Roles are a collection of permissions. To provide a user with access to a resource, we grant them a role rather than assigning permissions directly to the user. There are three kinds of roles – that is, **basic**, **predefined**, and **custom** – and we will discuss them later in this chapter. Please note that Basic roles were originally known as *Primitive roles.*
