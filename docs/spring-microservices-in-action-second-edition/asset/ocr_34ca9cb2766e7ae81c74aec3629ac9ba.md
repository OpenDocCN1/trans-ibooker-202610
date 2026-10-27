![](bbox=[0.146, 0.081, 0.893, 0.572])

Next, we’ll leave the default configurations for these sections: DB Instance Size, Storage, Availability & Durability, and Database Authentication. The final step is to set the following options. At any time you can click Info to get help about your selection. Figure 12.4 shows this screen.

+   - *Virtual Private Cloud (VPC)—*Choose Default VPC.
+   - *Publicly Accessible—*Select Yes.
+   - *VPC Security Group—*Select Create New, and add a name for the group. For this example, we named the security group ost​​ock-sg.
+   - *Database Port—*Enter 5432 for the TCP/IP port.

At this point, our database creation process begins (it can take several minutes). Once it’s done, we’ll need to configure the O-stock service properties to use the database in