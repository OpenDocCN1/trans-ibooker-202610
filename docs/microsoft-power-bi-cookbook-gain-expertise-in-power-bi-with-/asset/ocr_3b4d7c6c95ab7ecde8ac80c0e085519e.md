# Integrating Power BI with Other Applications

+   18. Click the **Pin** button.
+   
+   19. In the SSRS portal, click on the gear icon and select **My subscriptions** to confirm the **Power BI Dashboard** subscription:

| Edit | Report | Description | Status | Type | Folder | Delivery |
|---|---|---|---|---|---|---|
|  | Top 100 Customers |  | Enabled | Standard | /AdventureWorks Reports | Power BI Dashboard |

Figure 13.5: My subscriptions in SSRS

+   20. In the Power BI service, adjust the size, position, and optionally the title and subtitle of the dashboard tile.
+   
+   21. Click on the dashboard tile to test that the URL opens the report in the SSRS portal. Set the link to open in a separate tab.

This recipe is now complete.

# How it works...

Microsoft has been clear that Power BI and SSRS are designed for unique BI workloads and scenarios and organizations can choose the tool that is best suited for their given projects, as well as using multiple report authoring tools within the same solution and overall BI deployment. Power BI is designed for a modern, interactive, and rich data exploration experience.

SSRS, now included with Power BI Report Server, continues to deliver robust enterprise reporting capabilities with updated paginated report objects suited for operational reporting and distribution features such as subscriptions.

In certain reporting scenarios, a paginated or "classic" report with a fully configured page and report layout defined in a Visual Studio SSRS project is appropriate. Additionally, for organizations that can only deploy BI on-premises or if certain BI content such as highly sensitive reports must remain on-premises, Power BI Report Server provides a single on-premises solution and portal to include both traditional SSRS reports and optionally Power BI reports as well. Finally, Power BI Premium includes the capability to deploy SSRS paginated reports to the Power BI service.

Operational reporting workloads in which relatively simple, tabular report documents need to be distributed or made available across groups or teams in a specific file format such as PDF or Excel align well with SSRS. Paginated SSRS reports can provide a basic level of user interaction and data visualization via report parameters and charts, but this is not its strength or core use case. Note that SSRS also has a mobile report type and mobile report authoring tool in Microsoft SQL Server Mobile Report Publisher. Power BI supports individual user email subscriptions to reports, but SSRS supports data-driven report subscriptions that apply parameters to a report based on subscriber information, such as Eastern Region or Sales Managers.