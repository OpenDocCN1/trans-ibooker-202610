Table 3.8 is relevant when the Azure VM does not have its own ILIP and instead shares an outbound IP address with the other VMs in the pool. To determine whether your VM has an ILIP, navigate to the Networking blade in the portal for the VM. You will see something like that shown in Figure 3.20. Notice the NIC public IP address.

# **FIGURE 3.20**  Azure portal view of the NIC public IP address

![](bbox=[0.147, 0.233, 0.858, 0.326])

Take note that all Azure features that make outbound connections to the internet will have some dependency on TCP/IP, PAT, SNAT, or ephemeral ports. Make sure you recognize that this is not an Azure restriction; it is TCP/IP and the way networks function. You won’t need to know this level of detail for the Azure Solutions Architect Expert exam; however, a quality Azure Solutions Architect Expert would have some knowledge of this. They would know the scenarios where you get all 65,535 addresses or just a subset of them. The Azure Solutions Architect Expert would also need to know how to troubleshoot and provide some optimizations. More about this will be covered in Chapter 7.

If you haven’t noticed, 2¹⁶ − 1 or 65,535 is mentioned a lot in networking discussions, so I would therefore like to call attention to the areas where that number is relevant for networking. The “minus one” is present because the count begins at zero. 65,535 is as follows:

+   - The default TCP receive window buffer size
+   - The upper limit for TCP and UDP ports
+   - The highest number represented in 16-bit binary
+   - The maximum number of private IP addresses per VNet
+   - The maximum number of NICs per subscription
+   - The number of IP addresses with the /16 CIDR prefix

65,535 is a common number in computer science; just keep in mind that there are some limits around that number. It’s also cool to have an answer or comment about that number in case you see it when troubleshooting some issue: “Did you know that 65,535 is 2¹⁶?”

> **TCP and UDP**
> 
> TCP and UDP are supported on Azure VMs. UDP is a transport protocol that is best suited for media streaming like videos. UDP doesn’t mind if there are some missing packets that may cause a slight blip in the video. Using TCP for video streaming wouldn’t be prudent because a missing packet would be retransmitted, causing the video to hang while it waits for the packet. For scenarios where more accurate transmission of data is required, TCP is the best choice.