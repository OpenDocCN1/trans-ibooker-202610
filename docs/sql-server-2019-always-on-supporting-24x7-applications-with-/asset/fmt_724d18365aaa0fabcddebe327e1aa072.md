
We then use the `Add-ClusterResource` cmdlet to add the DTC resource. The Name parameter names the resource and the ResourceType parameter specifies that it is a DTC resource. We then need to create the dependencies between the resources within the role. We did not need to do this when using the GUI, as the dependencies were created for us automatically. Resource dependencies specify the resource or resources on which other resources depend. A resource failing propagates through the chain and could take a role offline. For example, in the case of our ALWAYSON-MSDTC-C role, if either the disk or the virtual name becomes unavailable, the DTC resource goes offline. Windows Server supports multiple dependencies with both AND and OR constraints. It is the OR constraints that make multi-subnet clustering possible, because a resource can be dependent on IP address A OR IP address B. Finally, we need to bring the role online by using the `Start-ClusterGroup` cmdlet.

## Creating an MSDTC Role
```
#Create the Role
Add-ClusterServerRole -Name ALWAYSON-MSDTC-C -Storage "Cluster Disk 3" -StaticAddress 192.168.0.50
#Create the DTC Resource
Add-ClusterResource -Name MSDTC-ALWAYSON-MSDTC-C -ResourceType "Distributed Transaction Coordinator" -Group ALWAYSON-MSDTC-C
#Create the dependencies
Add-ClusterResourceDependency MSDTC-ALWAYSON-MSDTC-C ALWAYSON-MSDTC-C
Add-ClusterResourceDependency MSDTC-ALWAYSON-MSDTC-C "Cluster Disk 3"
#Bring the Role online
Start-ClusterGroup ALWAYSON-MSDTC-C
```
Listing 3-6

Creating an MSDTC Role

### Configuring a Role
After creating a role, you may wish to configure it to alter the failover policy or configure nodes as preferred owners. To configure a role, select Properties from the role’s context menu. On the General tab of the Properties dialog box, which is shown in Figure [3-29](#394392_3_En_3_Chapter.xhtml#Fig29), you can configure a node as the preferred owner of the role. You can also change the order of precedence of node preference by moving nodes above or below others in the Preferred Owners window.
![Figure 3-29 ¶ The General Tab](images/394392_3_En_3_Chapter/394392_3_En_3_Fig29_HTML.jpg)
Figure 3-29 ¶ The General Tab

You can also select the priority for the role in the event that multiple roles fail over to another node at the same time. The options for this setting are as follows:
* High
* Medium
* Low
* No Auto Start

We will configure the ALWAYSON-MSDTC-C role to failover with High priority.

On the Failover tab of the Properties dialog box, you can configure the number of times that the role can fail over within a given period before the role is left offline. The default value for this is one failure within 6 hours. The issue with this is that if a role fails over, and after you fix the issue on the original node, you fail the role back, no more failovers are allowed within the 6-hour window. This is obviously a risk, and I generally advise that you change this setting. In our case, we have configured the role to allow a maximum of three failovers within a 24-hour time window, as illustrated in Figure [3-30](#394392_3_En_3_Chapter.xhtml#Fig30). We have also configured the role to fail back to the most preferred owner if it becomes available again. Remember, when setting automatic failback, that failback also causes downtime in the same way that a failover does. If you aspire to a very high level of availability, such as five 9s, then this option may not be appropriate. We will configure the ALWAYSON-MSDTC-C Role to allow three failovers, within a 24-hour period. We will also configure the role to allow immediate failback.
![Figure 3-30 ¶ The Failover Tab](images/394392_3_En_3_Chapter/394392_3_En_3_Fig30_HTML.jpg)
Figure 3-30 ¶ The Failover Tab

## Summary
Before creating the cluster, the Microsoft Cluster Service (MCS) must be installed on all nodes. This can be achieved by installing the Cluster Feature, using the Add Roles and Features Wizard.

Once the cluster feature has been installed, clustering can be configured on each node by using the Create Cluster Wizard. Before building the cluster, this wizard will prompt you to run the Cluster Validation Wizard. The Cluster Validation Wizard will validate that environment meets the requirements for a cluster. If you find that your environment does not meet the requirements, you can continue to building the cluster, but the installation will not be supported by Microsoft.

Once the cluster has been built, it will also need to be configured. This will include configuring the quorum mode and may also include configuring MSDTC. After creating a role on the cluster, you may also wish to configure the role with failover policies or preferred owners.

© Peter A. Carter 2020 P. A. CarterSQL Server 2019 AlwaysOn[https://doi.org/10.1007/978-1-4842-6479-9_4](https://doi.org/10.1007/978-1-4842-6479-9_4) Begin Abstract

# 4. Implementing an AlwaysOn Failover Clustered Instance
Peter A. Carter^([1](#394392_3_En_4_Chapter.xhtml#Aff2) ) (1) Botley, UK
End Abstract Once the cluster has been built and configured, it is time to install the SQL Server AlwaysOn failover cluster instance. In our scenario, we want to build a clustered instance, which spans both nodes of the Windows cluster that we built in Chapter [3](#394392_3_En_3_Chapter.xhtml). We will also discuss how to build the failover clustered instance using PowerShell. To do this, we will need to use the Install a SQL Server Failover Cluster wizard on the primary node of the cluster. We will then need to run the Add Node wizard to allow the passive node to host the instance in the event of a failover. Therefore, in this chapter, we will perform the following tasks:
* Install a SQL Server failover clustered instance on the active cluster node.
* Configure the passive node of the wizard to support the failover clustered instance.

## Building the Instance
An AlwaysOn Failover Clustered Instance can be built using the Install A SQL Server Failover Cluster wizard, which can be invoked by opening the SQL Server Installation Center, on the node hosting the cluster core resources, and selecting the New SQL Server Failover Cluster Installation option from the Installation. The Installation tab of the SQL Server Installation Center is illustrated in Figure [4-1](#394392_3_En_4_Chapter.xhtml#Fig1).
![Figure 4-1 ¶ SQL Server Installation Center – Installation Tab](images/394392_3_En_4_Chapter/394392_3_En_4_Fig1_HTML.jpg)
Figure 4-1 ¶ SQL Server Installation Center – Installation Tab

The Product Key page of the wizard is the first to be displayed and is illustrated in Figure [4-2](#394392_3_En_4_Chapter.xhtml#Fig2). On this page of the wizard, you will either select one of the free versions of SQL Server to install, or enter a product key, or volume licensing key, which will automatically determine the correct version of SQL Server to install.
![Figure 4-2 ¶ Install a SQL Server Failover Cluster Wizard – Product Key Page](images/394392_3_En_4_Chapter/394392_3_En_4_Fig2_HTML.jpg)
Figure 4-2 ¶ Install a SQL Server Failover Cluster Wizard – Product Key Page

Tip
In SQL Server 2019, Developer Edition, which is functionally equivalent to Enterprise Edition, is free. Prior to SQL Server 2016, there was a nominal charge associated with this noncommercial license.

On the License Terms page of the wizard (Figure [4-3](#394392_3_En_4_Chapter.xhtml#Fig3)), you will be invited to accept Microsoft’s license terms, via a check box. The installation cannot proceed without agreement.
![Figure 4-3 ¶ Install a SQL Server Failover Cluster Wizard – License Terms Page](images/394392_3_En_4_Chapter/394392_3_En_4_Fig3_HTML.jpg)
Figure 4-3 ¶ Install a SQL Server Failover Cluster Wizard – License Terms Page
