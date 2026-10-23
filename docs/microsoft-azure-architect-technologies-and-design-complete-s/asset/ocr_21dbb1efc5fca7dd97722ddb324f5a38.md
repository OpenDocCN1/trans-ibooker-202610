selectable option. An error will be thrown if you attempt to use the regions via PowerShell, a REST API, or any other supported client.

It is possible to configure a subscription-wide Azure Policy instance that restricts where VNets and other resources can be configured. This prevents accidentally placing any resources into a region that doesn’t comply with customer, country, or company requirements. Azure Policy is discussed in more detail in Chapter 6. Also, Chapter 6 discusses the Microsoft Trust Center, which provides a lot of information about compliance and policy making. If you are urgently wanting to learn more about those topics, skip forward and take a look. If not, continue reading to learn more about some key Azure virtual networking features and capabilities.

# Key Features and Capabilities

In the previous section, we touched briefly on how the location of resources impact or restrict inclusion in a VNet. We also touched a bit on the connectivity between VNets running on Azure and the connections of those VNets with perhaps an on-premise front-end or backend resource. In this section, I discuss this area a bit more, starting with some key VNet benefits and then concluding with some coverage of filtering and routing traffic in the networks.

The following are the areas covered in this section:

+   - Connecting with other Azure resources
+   - Connecting with other Azure VNets
+   - Connecting with the internet
+   - Connecting with on-premise resources
+   - Filtering network traffic
+   - Routing network traffic

The terms Azure products and features have been the common expressions for defining what Azure entity is specifically in scope at the time. The Azure products SQL Azure, Azure VM, and Azure App Service, for example, come with a calculable price. Features bound to a product generally have no charge. This is the case, for example, for EasyAuth, network security groups (NSGs), and network interfaces. Going forward, using the term Azure resource encompasses all Azure products and features. It is likely you are now able to differentiate between calculated price and no charge anyway.

# Some Key Azure VNet Features and Capabilities

It should come as no surprise that the primary reason to use a VNet is to isolate a group of Azure resources into a segmented group, where the isolation comes in the form of the VNet and the segmentation as the subnet within it. When a VNet is created, as you will do in Exercise 3.2, it is required to provide an address range for the VNet. A typical starting value for this is 10.0.0.0/16, which provides a range of IP addresses from 10.0.0.0 to