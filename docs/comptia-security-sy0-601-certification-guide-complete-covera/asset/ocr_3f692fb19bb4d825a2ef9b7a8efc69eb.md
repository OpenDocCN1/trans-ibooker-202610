# Network Monitoring

The following products can be used for network monitoring. Let’s look at this in turn, starting with NetFlow:

+   - **Netflow**: This is a CISCO product that monitors network traffic, so that they can identify the load on the network. This helps you utilize your network traffic efficiently. During an investigation, it can help identify patterns in network traffic.
+   - **Sflow**: This is a multi-vendor product that gives you clear visibility of network traffic patterns. This can help identify malicious traffic so that we can keep the network secure and safe.
+   - **IP Flow Information Export (IPFIX)**: This product can be used to capture traffic from the node itself. This data can then be exported to a collector within the node.
+     **Example**: IPFIX can be used to identify data traveling through a switch and this can be used for billing purposes. It can take IP Flow information and both format the data and forward it to a collector.

# Protocol Analyzer Output

A protocol analyzer such as *Wireshark* can capture data traveling across the network. Law enforcement has been able to use Wireshark for forensics by replaying video traffic sent to network devices they capture. The output can be saved in a packet capture file (pcap). Other names for a protocol analyzer are a packet sniffer, wireshark, or tcpdump.

In the next section, we will learn to apply mitigation techniques or controls to secure an environment.

# Knowing How to Apply Mitigation Techniques or Controls to Secure an Environment

Should we find that we have a vulnerability, we need to take action to mitigate the risk, so let’s look at some of the techniques or controls that we can use.

# Reconfigure Endpoint Security Solutions

When technologies change or we suffer a data breach, we might have to reconfigure the endpoint security solutions. We are going to look at these types of configurations in the following sections, starting with an application approved list