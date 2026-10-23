# chariscloud20 | User settings

Azure Active Directory

+   - Mobility (MDM and MAM)
+   - Password reset
+   - Company branding
+   - User settings
+   - Properties
+   - Security
+   - Monitoring

![](bbox=[0.12, 0.1, 0.845, 0.39])

Figure 8.27 – Verifying the app registration settings

If the preceding option has been disabled in the tenant, then the permission highlighted in the following table is needed. The built-in roles listed in the Azure AD Role column have this permission:

| Method | Permission Needed | Azure AD Role |
|---|---|---|
| Create a new application registration | microsoft.directory/servicePrincipals/create | - Application Administrator
- Application Developer
- Cloud Application Administrator
- Custom roles cannot currently be used |

Table 8.2 – Roles for creating a new app registration

Using a service principal is a great way to bypass a lot of security controls such as MFA, conditional access, and privileged identity management settings. This is because calls made by service principals are not blocked by conditional access, and Privileged Identity Management (PIM) is not supported for service principals.