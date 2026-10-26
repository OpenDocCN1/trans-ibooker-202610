> **Exam tip**
> 
> Heuristic/Anomaly-based NIPS/NIDS detect new patterns and are much more efficient than signature-based systems, which can only work with known variants.

# Modes of Operation

There are different modes of operation for the sensors of the NIPS/NIDS:

+   - **Inline:** The NIPS will be placed on, or very near to, the firewall as an additional layer of security. When the NIPS has been set up in inline mode, the flow of traffic goes through the NIPS. This is known as *in-band*.
+   - **Passive:** The traffic does not go through the NIPS. This mode is normally used by the NIDS as it detects changes in traffic patterns in the local network. This is known as *out-of-band*. Sensors and collectors forward alerts to the NIDS.

# Sensor/Collector

A sensor/collector can be placed on your network to alert the NIDS of any changes in traffic patterns within the network. If you place your sensor on the internet side of your network, it will scan all of the traffic from the internet.

# Monitoring Data

When we use analytics (how we analyze the data) to examine the information provided, it is based on rules that are set inside the IPS/IDS. However, no system is foolproof. They try their best but sometimes provide outcomes that are different from those expected. There are three different types:

+   - **False Positive:** The NIDS/NIPS has decided, based on the information gathered, that an attack is taking place. However, when the network administrator investigates it, they find that there is no attack. They do not agree.
+   - **True Positive:** When both the monitoring system and manual inspection agree this is known as a true positive.