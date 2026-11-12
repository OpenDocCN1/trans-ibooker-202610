![](bbox=[0.12, 0.07, 0.87, 0.47])

+   1. Ensure that all devices are connected as per the diagram and powered on, and that IP addresses have been assigned.
+   2. Go to the CLI of VAN-R1.
+   3. On the VAN-R1 router, verify that the Gig0/0 and Serial0/3/0 interfaces are up by viewing their statuses.
+   4. View the routing table on VAN-R1 with the **show ip route** command.
+      You should have two connected networks: 12.0.0.0 connected to Gig0/0 and 13.0.0.0 connected to Serial0/3/0.
+   5. Switch to the CLI of LA-R1 and view the status of the interfaces Gig0/0 and Serial0/3/0. Note the IP addresses of each interface.
+   6. View the routing table of LA-R1 by using the **show ip route** command.
+      You should have two connected networks: 14.0.0.0 connected to Gig0/0 and 13.0.0.0 connected to Serial0/3/0.
+   7. Test connectivity between ComputerA and VAN-R1 by launching a command prompt on ComputerA and using the **ping 12.0.0.1**