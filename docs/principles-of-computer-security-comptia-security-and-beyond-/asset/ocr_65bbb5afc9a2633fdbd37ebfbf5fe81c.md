+   e. Connect the switches together by selecting Copper Cross-Over (fourth from the left in the bottom-middle pane), clicking each switch, and selecting GigabitEthernet0/2 for each. Orange circles by each switch will turn to green triangles shortly thereafter, indicating a good connection.

3e–3f

# Step 3 Configure the PCs as follows:

+   a. Click the first PC from the left (the one Packet Tracer labeled PC0).

+   b. Click the Config tab at the top of the PC0 window that opened.

+   c. In the pane on the left, under INTERFACE, click FastEthernet0.

+   d. In the IP Configuration section, give PC0 an IP address of 10.1.0.100, with a subnet mask of 255.255.0.0. In the next lab exercise, you’ll add default gateway IP addresses to the PCs as well as routers to this topology.

In an IPv4 address, all host bits can’t be 0s (because it represents the network ID) and all host bits can’t be 1s (because it represents a network broadcast address). Since the fourth octet here (representing the last 8 of 16 host bits for this network addressing scheme) can’t have a value of 0, which would match up nicely with the name PC0, 100 has been chosen as an alternative.

+   e. Click the Desktop tab and then click the Command Prompt icon. Type ipconfig and press ENTER to verify that you’ve assigned the IP address and subnet mask correctly.

+   f. Repeat these steps on the other three PCs, assigning IP addresses of 10.1.0.1 to PC1, 10.1.0.2 to PC2, and 10.1.0.3 to PC3. Each PC will, likewise, have a subnet mask of 255.255.0.0.

4b–4c, 4e–4h

# Step 4 Configure the switches, send traffic from the PCs through the switches, and examine what the switch learned, as follows:

+   a. Click the first switch, labeled Switch0.