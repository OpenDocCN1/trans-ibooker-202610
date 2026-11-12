![](bbox=[0.14, 0.14, 0.26, 0.2])

**NOTE** By default, all VLANs can traverse a trunk. VTP pruning is used on trunk connections dynamically to remove VLANs not active between the two switches. It must be enabled on a VTP server switch, and the other switches must be either servers or clients.

Only a VTP switch in *server* mode can enable VTP pruning, and the remaining switches in the domain must be either in VTP server or client mode. If you have transparent mode switches, you’ll have to prune VLANs off their trunk links manually.

# Management VLAN

Unlike Cisco routers, every Cisco switch comes with a default configuration. For instance, some preconfigured VLANs are already on the switch, including VLAN 1. During the configuration, all VLAN commands refer to the VLAN number, even though you can configure an optional name for the VLAN. Every port on your switch, by default, is associated with VLAN 1. And all communications from the switch itself—VTP messages, Cisco Discovery Protocol (CDP) multicasts, and other traffic the switch originates—occur in VLAN 1. Recall from Chapter 9 that the Cisco switch’s IP configuration is based on the VLAN interface for which you configure your IP address.

VLAN 1 is sometimes called the *management VLAN*, even though you can use a different VLAN. It is a common practice to put all of your management devices—switches and management stations—in their own VLAN. If you decide to put your switch in a different VLAN than VLAN 1, it is recommended that you change this configuration on all your management devices so that you can more easily secure them, since other VLANs would have to go through a layer 3 device to access them; and on this layer 3 device, you can set up access control lists to filter unwanted traffic.