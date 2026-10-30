
Much more costly than a vending machine hack are the sensor attacks on keyless entry cars. One well-known example is the viral video feed from an external security camera that shows two men approach a parked and locked Tesla Model S in the homeowner's driveway. A man with a laptop scans close to the home's external walls, hoping to come within distance of the Tesla's key fob. The other man stands very near the car door with a signal relaying device. By triggering the key fob, the first thief can relay the signal to his accomplice. The relay attack allows them to unlock the car and ultimately steal it.

Credit card skimmers and shimmers are forms of sensor attack. A skimmer is when the attacker eavesdrops on the data from the magnetic stripe reader in a credit card terminal. The card's magnetic data might be read by a portable device carried by, for example, a malicious wait server. Or the reading device could be a stationary point-of-sale terminal, where the attacker installed a second sensor to read the same data and record it.

A shimmer is another device that also lifts data from bank cards, but, unlike a skimmer, the shimmer intercepts data from the card's embedded `EMV` (Europay, MasterCard, Visa) chip. On bank cards with the `EMV` chip, the chip is enclosed within the card, but it has familiar gold-plated contacts on the surface to provide conductive touchpoints for when the bank card is inserted at a point-of-sale (`POS`) reader. The attacker's shimmer is a thin plastic sleeve or plate, shoved completely inside the `POS` reader. The shimmer as a plastic sleeve is little more than a surface to hold electrical contacts and a small integrated circuit (`IC`) chip for recording the inserted bank card. The shimmer's contacts are placed in a position to be “sandwiched” between the bank card and the legitimate `POS` reader.

Output attacks involve manipulating the actuators controlled by the embedded system to bypass the controls imposed by the system. Imagine an electric door lock in which the cabling is exposed, so all the attacker needs to do is strip the insulation and apply the appropriate voltage to cause the door to unlock.

Processor attacks involve compromising the processor directly, through means that can range from connecting directly to the processor or memory chips to carefully removing the tops of integrated circuits and using ion beams to probe the chip to obtain or manipulate information within the processor. Processor attacks are normally preceded by a physical attack to gain access to the processor.

Embedded systems that support firmware updates may be vulnerable to accepting rogue or unauthorized firmware. The attacker loads their own version of the firmware (either written from scratch or reverse engineered from code extracted from the device or downloaded from the manufacturer) that causes the device to behave as the attacker desires.

Protection against firmware attacks can include digital signatures to prevent unauthorized firmware from being accepted and executed, and cryptography to protect against reverse engineering of firmware updates.

As with IoT devices, a problem is that it is difficult, if not impossible, to upgrade the software in many embedded systems. This means that vulnerabilities that are discovered after the product has shipped may be difficult or impossible to patch. The result may be the need for compensating controls to mitigate the risk from the unpatched vulnerability or the need to replace the unit entirely.

## High-Performance Computing Systems

*High-performance computing* (`HPC`) refers to the use of one or more supercomputers, generally for the purpose of highly complex computational science and other mathematically involved applications. Supercomputers have been around since the 1960s, but they continue to evolve, with most modern `HPC` systems leveraging cloud-based technologies to increase their distributed computing capabilities.

Generally speaking, `HPC` systems experience many of the same security concerns as traditional systems and other cloud-based systems (as discussed in earlier sections). They typically run Linux-based operating systems and are subject to software vulnerabilities, configuration issues, and compromised credentials. All of the traditional security measures should be considered here. A distinction, however, may come in the form of highly specialized, purpose-built hardware and software that supports the `HPC` environment. Any customized hardware and software present an added threat vector that must be secured. In the case of high-performance computing, the challenge becomes ensuring secure operation without adversely impacting performance.

As a best practice, `HPC` systems should be moved to their own physical enclave or logical security zone that is separate from traditional systems. Doing so segregates `HPC` systems from other types of computing systems that may have different security needs or regulations. The separate enclave provides a single point for ingress and egress monitoring of the `HPC` environment.

Accommodating the high volume of data traffic that passes through `HPC` systems requires a modified approach to network monitoring. Instead of using stateful firewalls for deep packet inspection, it's typically best to use firewalls that inspect packet headers, but not the payloads themselves. Separately, deep packet inspection may be used for intrusion detection, because stateful IDSs are not used inline, thus not introducing throughput delays. Here, using separate tools allows achieving traditional security intentions that accommodate for the uniqueness of `HPC` systems.

## Edge Computing Systems

*Edge computing* is a distributed computing model that brings compute and storage resources closer to the location where it is needed, improving response times and reducing bandwidth. The concept of edge computing dates back to the content delivery networks (`CDNs`) of the 1990s and now extends into the world of cloud computing. `CDNs` are covered in detail in [Chapter 4](#c04.xhtml), “Communication and Network Security.”

Today, edge computing helps manage the tremendous growth we're seeing with IoT devices. Edge computing allows pseudo-local data processing to minimize data sent over the internet. For example, imagine a factory with 5,000 internet-connected video cameras that send video to a remote location. This many live streams being sent over the company's network would be both slow and costly. Edge computing addresses this by processing the camera data locally and sending only necessary information over the internet. As opposed to all 5,000 continuous live streams being transmitted, your edge device might use artificial intelligence (`AI`) to identify anomalous behaviors in the factory and send 30 second clips when something needs to be investigated.

### Security Concerns

From a security standpoint, edge computing presents a few challenges.

Devices located at the edge, rather than centrally managed in a data center or other tightly managed facility, may not always receive the same diligence as their peers. You must be sure to apply the same security rigor to edge devices as your centrally managed devices. This includes hardening, patching, and providing the right level of physical security for edge computing systems. `SOAR` (covered in [Chapter 7](#c07.xhtml)) can help organizations observe and harden their devices, including those located at the edge.

Data must be encrypted when in transit between edge devices and centralized systems (or the cloud), and `VPN` tunneling may also be advisable for sensitive data when managing remote systems.
