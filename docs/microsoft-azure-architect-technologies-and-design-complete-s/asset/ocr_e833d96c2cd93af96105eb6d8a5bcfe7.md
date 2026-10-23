we discuss monitoring, you will come to learn how you can see the impact of your security policy on productivity. Additionally, you can use the “What If” feature, discussed in Exercise 2.4, to check how impactful the policy will be.

# Privileged Identity Management

Giving a group or individual user access to a resource is standard practice. This measure protects unauthorized individuals from gaining access and potentially causing the company or customer harm. There may be a scenario where just being authenticated or authorized to access the resource isn’t restrictive enough; for example, providing a user with read access to a database so that the individual can analyze behaviors to find new features for a product or to find unexpected behaviors due to poor code path execution in the application. Perhaps an individual needs to make a remote desktop connection to a server to troubleshoot an issue or give someone access to a certificate’s private key.

In each scenario, the security administrators have the justification to grant access, but the issue is how long they need access, because some of the resources being accessed are more sensitive than others and would therefore need some additional security policies. How can that be implemented? How can additional security levels be applied based on a specific resource? The solution is Privileged Identity Management (PIM). Simply, PIM is a feature for controlling, managing, and monitoring access to highly critical or sensitive resources within your company. This is performed by adding authentication capabilities to resources on the Azure platform based on an individual’s AAD identity, AAD group assignment, or Azure resources through RBAC (RBAC is discussed later in this chapter). Table 2.5 shows the additional authentication features available through PIM.

**TABLE 2.5**  Privileged Identity Management Authentication Features

| Authentication Feature | Description |
|---|---|
| Audit history | A report for analyzing who has accessed which resource, when, and how often |
| Notifications | Notification when a group or user is granted privileged access to a resource |
| Multifactor authentication | Additional layer of authentication |
| Time-bound | Apply start and end dates for the privileged access. |
| Access reviews | Reports that identify if access is still required for the specific user or group |
| Justification | Create rules for requested access, like a work item must be provided along with the request. |