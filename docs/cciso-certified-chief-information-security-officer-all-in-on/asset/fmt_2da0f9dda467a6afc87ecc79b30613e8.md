
# Stateful Firewalls

A stateful firewall monitors the state of network conversations and allows packets to flow through the firewall only if they are part of existing authorized conversations. When an authorized host creates a new conversation, the “state” of that conversation is tracked so that the return traffic (coming from the other party) will be allowed through. When the conversation is complete, the firewall notices the end of the conversation, since it is monitoring the state of the conversation, and closes the hole for the return traffic. Then, even if the same host (the other party) who was previously in the conversation, and whose traffic was previously allowed, tries to communicate through the firewall, its traffic will be blocked.

![Images](Images/exam.jpg)

> **EXAM TIP** CCISO candidates should be familiar with the different types of firewalls and their functions.

## Network Segmentation and DMZs

Networks are often divided into *segments* to implement security controls that limit access to or from portions of the network. Segmentation limits the flow of traffic to parts of the network based on policies. For example, an organization may use segmentation to limit access to a portion of its network used for their financial systems. Only authorized users can access that portion of the network. Segmentation creates more barriers for an attacker to go through to reach sensitive systems, increasing the *work factor* of an attack.

Segmentation can be accomplished *physically* by separating a network into smaller networks. Or segmentation can be accomplished *logically* by creating logically separate networks (virtual networks or VLANs) using the same physical network hardware. VLANs are implemented by configuring network switches and routers to associate specific nodes on the network with logical segments and restricting communication between segments based on security policy.

A demilitarized zone (DMZ) is a network segment that is in between the protected internal network and the external network, as shown in [Figure 4-8](#ch4.xhtml#fig4_8).

![Images](Images/F04-08.jpg)

**Figure 4-8** Demilitarized zone

A DMZ creates a buffer zone. Two levels of firewalls form the DMZ border and protect the internal network assets. Companies usually place their e-mail, web, and DNS servers in the DMZ. Although these servers are “Internet facing” they are still behind one firewall. But servers in the DMZ are still vulnerable and should be carefully hardened since they are first in line to external attacks.

## Virtual Private Networks

A VPN is a secure private connection to an organization’s protected network that travels through a public untrusted network. Using VPN technology, an organization can extend, virtually, its private network outside the network’s normal perimeter. It does this by using tunneling technology that provides an encrypted channel through untrusted networks, such as the Internet, into the organization’s protected enterprise. Therefore, remote users such as employees working at home offices can access resources on the internal corporate network almost as if they had a direct connection.

VPNs use tunneling protocols that encapsulate the frame/packet so it can be transmitted through a network. They preserve the original protocol headers so the message can be “unwrapped” at its destination but they allow the encapsulated frame to be routed as normal traffic. These are the most commonly used VPN protocols:

- **Point-to-Point Tunneling Protocol (PPTP)** PPTP was for many years the de facto VPN standard. However, there are newer VPN technologies that are better and in wider use such as IPSec.
- **Layer 2 Tunneling Protocol (L2TP)** L2TP was developed by Cisco as a solution to the limitations of PPTP. However, L2TP does not provide encryption, so it is commonly used in combination with other methods such as IPSec.
- **IP Security (IPSec)** IPSec is a suite of protocols that, in combination, provides data integrity, authentication, and confidentiality. It is now the de facto standard for VPNs.

## IDS/IPS

Intrusion detection systems (IDSs) and intrusion prevention systems (IPSs) are companion technologies involving specialized tools to detect (IDS) and prevent (IPS) malicious activity. Both deploy sensors throughout the network that communicate with a control or reporting system that allows the security staff to view indicators of malicious activity and take measures to prevent security breaches.

A *network-based IDS (NIDS)* uses sensors deployed throughout the network in the form of computers or specialized appliances. A *host-based IDS (HIDS)* uses agents installed on host computers that monitor for, and detect, malicious activity on the host. HIDS agents look for host- or OS-specific activities such as running processes, registry changes, file alteration, and so on. Whether network or host based, there are several detection methods employed by IDSs. Modern IDS products usually take advantage of more than one of these methods:

- *Signature-based IDSs* look for specific patterns in traffic, specific messages, or specific files on systems. These specific patterns are called *signatures*. Usually signatures are maintained in a library. Very often IDS product vendors provide access to a subscription service whereby a third-party vendor conducts threat intelligence activities to continually learn about new attacks and what kinds of signatures would be indicative of their presence. Those signatures are downloaded to the IDS products so that they are continuously monitoring for and able to detect the latest attacks. Signature-based IDSs are good at detecting already known attacks and malware but not as good at detecting new kinds of attacks.
- *Anomaly-based IDSs* are better for detecting unknown attacks. If a signature-based IDS is good at knowing “what bad looks like,” an anomaly-based IDS is good at knowing “what good looks like.” An anomaly-based IDS uses specialized methods such as artificial intelligence and machine learning to build a picture of what normal activity looks like on the network or system. Then if the IDS sees activity that deviates from the norm, it passes an alert to indicate the activity should be investigated. Anomaly-based IDSs are good at detecting previously unknown attacks for which signatures may not exist.

IDS and IPS products use essentially the same technology and methods to detect potentially malicious activity. The difference lies in what each one does about that activity. An IDS simply reports the activity to a console where the security staff can see the alert and perform an investigation to determine if the alert represents actual malicious activity or a breach and then determine the correct course of action. An IPS goes beyond mere detection and reporting by taking action to try to stop the attack or minimize its impact. Candidate actions may include blocking traffic from a particular IP address, turning off a port on a firewall, resetting a device such as a router, or even changing data in packets or hosts to remove or replace malicious messages or files. While it sounds tempting to deploy an IPS that can automatically stop an attack in its tracks, many organizations, especially government/military/defense organizations, are reluctant to use IPS functions due to the risk of automatically making changes to the environment without first testing the impact.
