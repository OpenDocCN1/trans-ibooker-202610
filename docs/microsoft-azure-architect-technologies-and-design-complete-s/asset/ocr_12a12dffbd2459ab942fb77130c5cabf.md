provided for an additional review and may contain some supplementary tips, procedures, or diagrams that are helpful for building a reliant IT solution on Azure.

# Recovering Azure Networking

The networking backbone on which Azure Functions is fully supported by Microsoft for all cloud hosting models. You wouldn’t need to take any proactive activities from a networking infrastructure perspective. Because of that, from a BCDR perspective, you should focus on two other aspects. The first one has to do with your IaaS-based Azure products that are supported specifically by ASR. From a networking perspective, notice in Figure 9.34 that when you test a failover, you select the Azure virtual network into which you want to place the recovered items. If one doesn’t exist in the target region, you are prompted to create one.

# **FIGURE 9.34**  Azure Site Recovery Test failover VNET creation

![](bbox=[0.147, 0.379, 0.857, 0.781])

Once the test is successful and you need to actually trigger a failover, the virtual network in the target region is the place where your VMs will be placed into. That virtual network is ready to support the compute resources running within it. The other aspect concerns the utilization of the plethora of network virtual appliances (NVAs) available to you