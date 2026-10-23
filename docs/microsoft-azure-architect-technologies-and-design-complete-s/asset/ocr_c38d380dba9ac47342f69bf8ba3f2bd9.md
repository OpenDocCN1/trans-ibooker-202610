An NSG can restrict traffic based on a protocol, port, and specific IP address. This is a tight and secure way to manage what can access the resources within the subnet. Outbound traffic can be controlled, as well. The outbound request originates from one of the IP addresses in the subnet, and it will pass any restrictions placed elsewhere; therefore, making sure the traffic is flowing over the expected port using the expected protocol could also be warranted. This is covered in greater detail in the “Network Security” section.

Once the VNet is configured, you can sleep well knowing that it is being monitored and supported by Microsoft. To fully understand this complexity, you need to look at this from an Azure (or any other cloud hosting company) perspective. Neither entity can predict when a customer creates a VNet if it is for fun, for testing, or if it will be a full-blown data-intensive bandwidth-hungry monster. From that perspective. Microsoft has you covered from three perspectives. The first is that the network is monitored for bandwidth and will scale as your workload requires it. Second, if your workload is in a section of the data center with other customers and they start consuming massive amounts of network capacity, it gets handled. Finally, the network is constantly monitored from a health and wellness perspective and gets auto-healed when telemetry-based thresholds are breached.

# Connecting with Other Azure Resources

Recall from Figure 3.3 that some Azure resources must reside within a VNet, some can exist in a VNet, while other Azure resources cannot exist in a VNet or do not by default. Table 3.4 provides a compact view of which Azure resources can/cannot be deployed into an Azure VNet. As this list changes frequently, conduct a search on the internet for the most current information. Table 3.4 contains those items most likely to be covered when taking the Azure Solutions Architect Expert exam.

# TABLE 3.4 Azure Resource VNet Support

| Type | Azure Resource | VNet Deployable |
|---|---|---|
| Web | Azure App Service | × |
| Compute | Azure VM | ✓ |
| Networking | Azure Application Gateway (web application firewall) | ✓ |
| Web | Azure Logic Apps | ✓ |
| Security | Azure Key Vault | × |
| Data | SQL Azure | × |
| Web | API Management | ✓ |
| Networking | Azure Firewall | ✓ |