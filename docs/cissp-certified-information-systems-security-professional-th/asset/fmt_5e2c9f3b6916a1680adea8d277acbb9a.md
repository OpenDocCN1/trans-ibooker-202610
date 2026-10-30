
NAT can be implemented on a variety of different devices such as firewalls, routers, gateways, and proxies. It can be used only on IP networks and operates at the network layer (layer 3). Originally, NAT was designed to extend the use of IPv4, since the pool of available addresses was quickly being exhausted. To that point, NAT is a legacy technology that comes with advantages and disadvantages.

First, consider its advantages. NAT is used to accomplish network and security objectives to hide the identity of internal clients, mask the routable design of your private network, and keep network addressing costs at a minimum by using the fewest public IP addresses possible. [Figure 4.5](#c04.xhtml#c04-fig-0005) shows an example of how you might find NAT implemented on a perimeter firewall. Through NAT processes, the organization assigns internal IP addresses, perhaps even a private addressing scheme. The NAT appliance catalogs the addresses and will convert them into public IP addresses for transmission over the internet. On the internal network, NAT allows for any address to be used, and this does not cause collisions or conflict with public internet hosts with the same IP addresses. In effect, NAT translates the IP addresses of the internal clients to leased addresses outside the environment. NAT offers numerous benefits, including the following:

*   NAT connects an entire private network to the internet using only a single or just a few leased public IP addresses.
*   NAT uses private IP addresses (`10.0.0.0` to `10.255.255.255`) in a private network and retains the ability to communicate with the internet as the NAT translates to a public, routable address.
*   NAT hides the IP addressing scheme and network topography of an internal, private network from the internet.
*   NAT ensures that connections originated from within the internal protected network are allowed back into the network from the internet.

![FIGURE 4.5 NAT implemented on a perimeter firewall](images/c04f005.png)

**[FIGURE 4.5](#c04.xhtml#R_c04-fig-0005) NAT implemented on a perimeter firewall**

Public IP addresses are essentially all allocated after the remaining class A (see [Table 4.1](#c04.xhtml#c04-tbl-0001)) addresses were exhausted years ago. This explains the upward trend in the popularity of NAT. Security concerns also favor the use of NAT, which mitigates many intrusion types of attacks. With roughly only 4 billion addresses available in IPv4, the world has simply deployed more devices using IP than there are unique IP addresses available. The fact that early designers of the internet and TCP/IP reserved a few blocks of addresses for private, unrestricted use is becoming a good idea. These set-aside IP addresses, known as private IP addresses, are defined in the standard RFC 1918.

Now, consider some of NAT's disadvantages. Again, remember that NAT was developed to help deal with the fact that IPv4 addressing was being exhausted. To that end, NAT was assumed to be a temporary solution. Because it was considered only temporary, the IETF, responsible for defining protocol standards, didn't pursue creating an in-depth official standard for NAT. In fact, while the IETF recognized the benefits of NAT and published a general specification, it avoided developing a technical specification to discourage NAT's widespread adoption. For that reason alone, the biggest disadvantage to NAT is how inconsistent its implementation in devices is.

A few technical disadvantages of NAT have been recognized, but solutions to those problems were discovered or developed without needing to reinvent NAT. For example, consider how peer-to-peer communication is handled. Without NAT, an initiator communicates with a target. This is not a problem provided both the initiator and the target have routable addresses. With NAT implemented, an initiator on the internet seeking to connect with a target behind NAT cannot connect with a nonroutable address. One way to solve this is for the peer-to-peer session to begin “backward,” with the target first connecting with the initiator for the purpose of discovering NAT in place. Then, once NAT's outside public address is known, the originator can begin a new peer-to-peer session. Services such as Skype, which rely on peer-to-peer or VoIP protocols, needed to create innovative ways to sidestep how NAT would otherwise break their service. Skype, for example, employs “SuperNodes” on public addresses to permit a peer-to-peer connection, even if both the target and the initiator are behind NAT.

Another disadvantage is how IPSec checks integrity. IPSec computes a hash value for the purpose of ensuring the integrity of each packet. That hash value is computed using various parts of the packet, and since NAT changes the packet's values, that hash value is no longer valid. To address this, the technology of NAT-Traversal (`NAT-T`) was developed. `NAT-T` ensures that IPSec isn't broken when one or both ends of the IPSec tunnel cross over a NAT device.

While NAT is a critical technology for IPv4 networks, the technology is no longer needed in IPv6. This is one of several improvements offered by the latest version of the Internet Protocol, which is discussed in the following section.

## Internet Protocol Version 6

As opposed to IPv4's 32-bit IP addresses, IPv6 uses 128-bit IP addresses to support a significantly larger number of addresses than its older peer. Each IPv6 address is portioned into four hexadecimal digits, which are separated by colons for addressing, and segmented into two parts: a 64-bit network prefix and a 64-bit interface identifier. An example IPv6 address looks like `2607:F0F0:1002:00B1:0000:0000:0000:0004`. The added complexity allows IPv6 to support 1,028 times the total number of IPv4 addresses, allowing for internet-connected devices to expand for the foreseeable future. The IPv6 scheme intends also to improve network addressing and routing.

Some of the principal benefits of IPv6 over IPv4 include the following:

*   **Scoped addresses:** This adds a layer of security and access control for administrators who can group and then deny or allow access to network services, like file servers or printing.
*   **Autoconfiguration:** This removes the need for both DHCP and NAT, thanks to the much larger public address space.
*   **Quality of service (QoS) priority values:** Based on the content priority, traffic management is conducted according to preset QoS priority values.

IPv6 was developed by the IETF to manage the anticipated problem of IPv4 address exhaustion, but adoption of IPv6 has been slow. Operating systems since about 2000 have had the ability to use IPv6, either natively or via an add-in. Adoption hurdles include added cost of some IPv6-capable devices and the fact that IPv4 works well. Decision-makers are reluctant to either make a change for the sake of change itself or make process improvements that provide a minimal financial return on investment (ROI). Early adopters of IPv6 are found in private, internal networks in large corporations, research laboratories, and universities.

## Network Attacks

Networking protocols were designed long before the necessity of security was fully recognized. Consequently, even today, networked hosts remain vulnerable and networked systems fail to implement mitigating controls. Systems that are hardened against attacks that exploit misconfiguration and unnecessary services can still be vulnerable from attacks exploiting network services. Several updates and revisions to networking protocols have been adopted, but weaknesses remain due to security being designed as an afterthought, although progress continues to be made.
