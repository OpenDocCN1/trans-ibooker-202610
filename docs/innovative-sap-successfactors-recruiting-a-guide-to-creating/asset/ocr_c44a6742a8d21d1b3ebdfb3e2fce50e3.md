# Step 1: Permissions in Admin Tools – Enable Background Check Integration

Log into the customer’s instance as an administrator and navigate to Admin Tools ➤ Manage Permission Roles. Choose System Admin as the Permission Role and ensure the following are selected. The SuccessFactors Recruiting system requires several permissions to be set up for different roles. Table 10-2 shows the applicable list of permissions.

**Table 10-2.** *Role Based Permissions Needed for Background Checks*

| System Role | Permission Category | List of Permissions |
|---|---|---|
| System Administrator | Manage Recruiting | – Background Check Central |
| System Administrator | Metadata Framework | – Admin Access to MDF OData API<br>– Manage Data |
| Recruiter | Recruiting Permissions | – Background Check Initiate Permission<br>– Background Check Update Permission |
| API User | General User Permissions | – User Login<br>– SFAPI User Login |
| API User | Recruiting Permissions | – Background Check Initiate Permission<br>– Background Check Update Permission<br>– ODATA API Application Create<br>– ODATA API Application Export<br>– ODATA API Application Audit Export<br>– ODATA API Application Update |