# Getting ready

To prepare for this recipe, follow these steps:

+   1. Confirm that Power BI Pro licenses are available to administrators and members of the workspace.
+   2. Ensure that the workspace aligns with the policy or standard of the organization for Power BI content distribution.
+   3. If premium capacity has been provisioned but not bulk assigned to all workspaces of an organization, determine if the new workspace will be hosted in a premium capacity.
+   4. If Premium capacity has been provisioned and authorized for the new workspace, evaluate the current utilization of premium capacity, and determine if the expected workload of datasets and user queries from the new app deployment will require a larger or separate capacity. In addition, confirm that the workspace administrator has workspace assignment permission, or assign this permission in the Power BI admin portal per the first step in How to do it....
+   5. Retrieve your Power BI tenant's UTC offset by clicking the question mark (?) icon in the upper-right corner of the Power BI service and choose About Power BI. The UTC offset is shown in Figure 12.14, in this case -0400:
+   6. Retrieve the Premium capacity ID from the Capacity settings of the admin portal in the Power BI service.
+   7. While in Capacity settings, click the gear icon under Actions for the desired capacity and note the CAPACITY ID.
+   8. From a workspace in the Power BI service, select Get data from the lower-left corner of the screen.
+   9. In the Services section, click the Get button.
+   10. Search for Premium and choose the Premium Gen2 Capacity Utilization Metrics app.

![](bbox=[0.2, 0.48, 0.87, 0.64])

Figure 14: UTC offset of Power BI tenant