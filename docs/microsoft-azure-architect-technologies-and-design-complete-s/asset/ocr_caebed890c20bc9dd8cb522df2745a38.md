There is a free tier for a Log Analytics workspace, which is the one that has a limit on the number of workspaces per subscription. Also notice that it is currently not possible to export the data you store into the workspace. This is the current situation; it might change in the future. Lastly, the more diagnostic logging you configure, the more data that is generated. Note that there is a limit on the amount of data that can be placed into the workspace per minute.

# Application Insights

Application Insights has a rather large set of capabilities. To begin with, you already know the difference between Log Analytics and Application Insights, right? Take a moment to remind yourself in what scenario you would look into Log Analytics data versus Application Insights. Figure 9.11 shows some additional differentiation, specifically, table names into which metric data is stored.

# FIGURE 9.11 Application Insights versus Log Analytics workspace logs

![](bbox=[0.2, 0.4, 0.8, 0.57])

Earlier there was an attempt to compare these two components that have been merged into the Azure Monitor service. I tried to explain how Log Analytics stores more platform-specific information, while Application Insights stores application and code data. Specifically calling out that the Event table shown for Log Analytics is more platform-related, you would expect to find within it what you see in the Event Viewer on a Windows server. Exceptions or request tables accessible through Application Insights are, simply by name, more associated to the code and an application than to a platform. It is hopeful now that the relation (or lack of relation) between the two is clearer. Also, as illustrated in Figure 9.11, it is possible to perform queries across the two data repositories, both of which are ADX clusters. This will become more obvious later.

Take a look back at Figure 9.9, and you will see the Applications link in the navigation menu. Navigate back to the Azure Monitor blade and click that link. On that link you will be provided with the ability to add a new Application Insights service. It is a simple process and requires only a subscription, a resource group, and a name. Once created, navigate to it and click the View Application Insights data link and notice two specific items. The first one is on the Overview blade; there is an attribute called the Instrumentation Key that