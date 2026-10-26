
## Network Monitoring

The following products can be used for network monitoring. Let’s look at this in turn, starting with NetFlow:

- **NetFlow**: This is a Cisco product that monitors network traffic, so that they can identify the load on the network. This helps you utilize your network traffic efficiently. During an investigation, it can help identify patterns in network traffic.
- **sFlow**: This is a multi-vendor product that gives you clear visibility of network traffic patterns. This can help identify malicious traffic so that we can keep the network secure and safe.
- **IP Flow Information Export (IPFIX)**: This product can be used to capture traffic from the node itself. This data can then be exported to a collector within the node.
  - **Example**: IPFIX can be used to identify data traveling through a switch and this can be used for billing purposes. It can take IP Flow information and both format the data and forward it to a collector.

### Protocol Analyzer Output

A protocol analyzer such as *Wireshark* can capture data traveling across the network. Law enforcement has been able to use Wireshark for forensics by replaying video traffic sent to network devices they capture. The output can be saved in a packet capture file (pcap). Other names for a protocol analyzer are a packet sniffer, Wireshark, or tcpdump.

In the next section, we will learn to apply mitigation techniques or controls to secure an environment.

## Knowing How to Apply Mitigation Techniques or Controls to Secure an Environment

Should we find that we have a vulnerability, we need to take action to mitigate the risk, so let’s look at some of the techniques or controls that we can use.

### Reconfigure Endpoint Security Solutions

When technologies change or we suffer a data breach, we might have to reconfigure the endpoint security solutions. We are going to look at these types of configurations in the following sections, starting with an application approved list.

### Application Approved List

We can use application whitelisting where the approved applications are listed. This means that if an application is not listed, it cannot be launched. Some devices such as pfSense have Allowed Lists. For the purposes of the Security+ exam, if we want to prevent users from installing applications or prevent malware from installing, we will create a whitelist. Neither the unauthorized applications nor the malware will be on the whitelist, therefore they will be prevented from being installed.

### Application Block List/Deny List

We can blacklist applications that are deemed dangerous, such as Kali Linux. If it is on the blacklist, it is totally banned and cannot be placed on the whitelist. Some devices such as pfSense firewall have Block Lists. We need the name of an application to blacklist it.

### Quarantine

If we find a device has been infected with a virus, we can remove it from the network by quarantining it. We can also use Network Access Control (NAC) so that devices that are not patched or are vulnerable are placed in a quarantine network where a remediation server applies patches to the system before it is allowed back on to the network. With NAC, the user is authenticated, then the device is checked to ensure that it is fully patched.

### Configuration Management

As new attacks emerge and new technologies are implemented, we may have to make some configuration changes to secure our environment. It is important that we take a robust approach to configuration management in the following aspects:

- **Diagrams:** Cybersecurity personnel, network engineers, and system administrators all use detailed diagrams to show the relationship of all the interconnected devices. This way, they can ensure that they have visibility of the security in place. They use heat maps to show the connectivity of wireless networks.
- **Standard Naming Convention:** It is important that all devices on your network can be identified as servers, routers, firewalls, or printers. We can do this by using a standard naming convention. Domain Controllers could start with a prefix of DC, and databases could start with SQL. If we looked on the network and saw DC1 and DC2, we could identify those as domain controllers, whereas SQL 1 and SQL 2 could be identified as database servers.
- **Asset Management:** If we had an up-to-date asset register coupled with a standard naming convention of different assets, we could keep track of all our assets and ensure they were accounted for and fully patched. Security administrators could compare the asset management register to unknown devices that could be identified as rogue devices on the network.
- **Baseline Configurations:** It is vital that each type of device being placed on the network has a secure baseline configuration. This could be achieved by having different images, one for each type of device. Use the principle of least functionality when we build the image, ensuring that only the required applications and services are installed.
- **Firewall Rules:** Firewalls can be used to block traffic and we can use either an MDM solution or group policy to change the configuration on endpoint devices.
- **Mobile Device Management (MDM):** An MDM solution can be used to push configuration changes to mobile devices. The password policy may change, or we might remove the ability to use the camera on mobile phones. The MDM solution will push out the new configuration changes.
- **Data Loss Prevention (DLP):** There may be a security risk of credit card information leaving the company or data that has a pattern match has been exfiltrated. We may use a regular expression to ensure that this data does not leave via email. Good examples of data that DLP prevents leaving our environment are PII and sensitive information.
- **Content Filter/URL Filter:** We might update the content filters on either a proxy server or a **Unified Threat Management (UTM)** firewall due to security events. The content filter blocks the target website; for example, a gambling website and the URL filter prevents the endpoints from visiting a website that has been subject to an attack.
- **Update or Revoke Certificates:** If the endpoints have reported a *host or trust error*, this means that we have a certificate problem. We may need to update the certificate because it has expired or revoke the certificate because it has been compromised. If the certificate is not installed on the Trusted Root Certification Authorities, it will also generate a host or trust error.

### Isolation

We may have to air gap research and development endpoints to isolate them from the network to protect them against a network-based attack. They have no wireless, Bluetooth, or Ethernet connection. The only way to add or extract data from an air gapped computer is by using a removable device such as a USB drive.

### Containment

If the security team finds that an endpoint has been compromised and may be infected by a virus, they will contain it so as to stop the malware spreading. Examples of containment are removing infected machines from the network or disabling user accounts that have been used to breach your network.

### Segmentation

We can use containerization in a BYOD environment, where we add a storage card to keep personal and business data separate on a mobile device. It will also protect the business data against remote wiping the device, as only the mobile device will be reset and not the storage cards. This is also known as storage segmentation. We may have to segment devices that have become vulnerable, such as an unpatched printer where there are no updates. We could place these printers in a VLAN.

### Security Orchestration, Automation, and Response (SOAR)

Orchestrations are the process of running multiple automations to perform complex tasks. Automations are the process of scripting a single activity. These systems are used to identify threats from multiple sources by using playbooks and runbooks. Let’s look at each of these in turn:

- **Runbooks:** These are documents with information on events and the necessary action that needs to be taken so that the human IT teams can take actions to stop threats. This information can be used to configure the playbook.
- **Playbooks:** Playbooks contain a set of rules and actions to enable the SOAR to identify incidents and take preventative action. These may need to be amended for better decision making as technology changes.
