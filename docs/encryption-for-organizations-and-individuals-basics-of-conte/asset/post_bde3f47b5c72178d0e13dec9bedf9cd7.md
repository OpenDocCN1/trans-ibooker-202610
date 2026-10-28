
## Table 13-1. The SECOQC nodes

| Designation | Full Name | Designation | Full Name |
|---|---|---|---|
| STP | St Pölten | ERD | Erdberger Lände |
| BRT | Breitenfurterstraße | GUD | Gudrunstraße |
| SIE | Siemensstraße | FRM | Siemens Forum |

The research group behind SECOQC identified three main components for their networking approach:

- 1. **The Q3P communication interface**, a quantum point-to-point protocol which provides a secure communication primitive between two adjacent nodes. It utilizes technologies such as QKD-TL and QKD-NL.
- 2. **QKD-transport layer (QKD-TL)**, a layer responsible for linking the correct applications for quantum transmission, connection management, and network congestion control.
- 3. **QKD-network layer (QKD-NL)**, a layer responsible for actually delivering quantum data between nodes, including data route determination.

SECOQC also highlighted some issues with implementing QKD, such as the poor rates of transmission (1 GiB per month). In addition, although the SECOQC approach was found to be completely interoperable within the framework, it’s not transferable to mixed networks (e.g., consisting of both QKD and conventional TCP/IP).

## Quantum Experiments at Space Scale (QUESS)

QUESS was a joint Austrian–Chinese space mission facilitated by the Tiangong-2 space laboratory, launched in September 2016. As one part of its payload, Tiangong-2 included the tools for Space to Earth quantum key distribution. The equipment allows ground receivers, potentially separated by thousands of kilometers, to establish secure quantum channels using entangled photons. A satellite named Micius, named after an ancient Chinese scientist, had the honor of carrying out these tasks.

QUESS successfully demonstrated QKD between two observatories in China with a distance of over 2500 kilometers. The system also facilitated the first intercontinental secured quantum video call between China and the Institute for Quantum Optics and Quantum Information in Austria. The distance between these two locations spanned over 7500 kilometers. In addition, QUESS tested quantum teleportation, transmitting the states of sub-atomic particles from the satellite into a ground station in Tibet.

The main benefit of satellite-based quantum key distribution is in its greatly reduced issues with entangled particles due to scattering; both fiber optic cables and ground-based line-of-sight quantum devices suffer considerably more from this issue. However, QUESS cannot function when basking in sunlight or without direct line of sight with ground nodes.

China has the ambitious goal of creating a global QKD network using technology found in Micius satellites by 2030.

## SwissQuantum

Built to test the long-term performance of a QKD system, Swiss company *ID Quantique* built the *SwissQuantum* QKD network in 2009. The system ran for two years in the Geneva metropolitan area. SwissQuantum ran between three nodes: the *University of Genova (Unige)*, CERN, and *hepia (Haute Ecole du Paysage, d’Ingenierie et d’Architecture)*. Like its DARPA counterpart, SwissQuantum ran on dark fiber cables.

SwissQuantum used a three-layered approach in implementing its QKD network.

- **Quantum layer.** Consisted of point-to-point QKD links running on commercial QKD devices.
- **Key management layer.** This layer was responsible for secret key distribution across the network and between the other layers.
- **Application layer.** A layer which offers its users a graphical user interface for interacting with the QKD system.

The SwissQuantum system was monitored using three *virtual local area networks (VLAN)*. A single VLAN was assigned for each of the three layers. The system was protected by two firewalls; only ID Quantique, Unige, and hepia had access to the network.

SwissQuantum supported the similar BB84 and SARG protocols, of which the latter is more resistant against photon-splitting attacks. SARG is a better protocol for long distances and was thus chosen for use in the SwissQuantum network. The QKD key sifting was done in three steps: *error correction*, *privacy amplification*, and *authentication* of the related classical communications. The privacy amplification phase was implemented using universal hash functions.

The SwissQuantum network demonstrated that relatively complicated layered QKD systems can be a realistic and reliable solution. The CERN node resided in France, while the other two were in Switzerland (see Table 13-2).

**Table 13-2.** *The links and their properties in SwissQuantum*

| Name | Nodes | Fiber Length | Optical Loss (dB) |
|---|---|---|---|
| SQ1 | CERN-Unige | 14.4 km | -4.6 |
| SQ2 | CERN-hepia | 17.1 km | -5.3 |
| SQ3 | Unige-hepia | 3.7 km | -2.5 |

Now, *optical loss* is a phenomenon which describes photon depletion over distances. In general, the longer the optical fiber cable, the more QKD-based data loss is likely to occur. A proposed solution is in *quantum repeater devices*, which would amplify the signal at specific points in the network to mitigate this issue. See Table 13-2 for some measurements of optical loss in the SwissQuantum network and how it relates to fiber length.

> Not only limited to the world of audio, optical loss in fiber optic sources is denoted in *decibels (dB)*. A decibel is a relative unit of measurement used in expressing the ratio of one value to another.

## Tokyo QKD Network

A collaboration between nine partners, the *Tokyo QKD Network* ran in 2010 on six nodes. In this project, gigahertz-clocked QKD systems, a QKD smartphone, a reliable commercial QKD product, and an entanglement QKD system were to be tested. For one, secure TV conferencing was achieved. Such a tested conferencing implementation incorporated an automatic communications rerouting system should eavesdropping be detected (see Figure 13-2).

![](img/df9b6c40c665f58cab0c435444990c63_265_0.png)

Figure 13-2. The six-node Tokyo QKD Network configuration. Secure video communication was demonstrated between Koganei and Otemachi servers. The dashed line denotes an alternate, 135 km route for these communications (i.e., through Koganei 2 to Otemachi 2)

Much like SwissQuantum, the Tokyo QKD Network had a three-layer approach. A quantum layer consists of point-to-point quantum links with each link generating a secure key using any arbitrary protocol or key format. Connected QKD devices then deliver the keys onto layer two, the key management layer.

In this middle layer, a key management agent (KMA) operates as a trusted node. Each KMA receives the makings of a key and proceeds to resize and save them. The qubit error rate (QBER) and the key generation rate are also stored for analysis.

The top, communication layer, handles numerous tasks, such as secure video conferencing and key transfer to mobile devices. The video in the system is delivered at a rate of 128 kbps, which provides somewhat passable fidelity. For comparison, a 720p video stream on a typical classical network (e.g., YouTube) is usually considered optimal at between 2000 and 4000 kbps, depending on the video protocols in use.

The teams behind Tokyo QKD Network experiments concluded that while they demonstrated long-term QKD use, the focus in this field in the near future should be on system miniaturization. Eventually, all critical levels in the Japanese government are expected to utilize QKD for their confidential communications.

## In Closing

After finishing this chapter, you’ll have learned the basics of five recent implementations of QKD networks, specifically DARPA, SECOQC, QUESS, SwissQuantum, and the Tokyo QKD Network. See Table 13-3 for a rundown.

## Table 13-3. Recent implementations of QKD networks at a glance

| QKD Network | Nodes | Longest Node Separation (L)/Total Network Length (T) | Area(s) of Study |
|---|---|---|---|
| DARPA | 10 | L: 19 km (BU-BBN) T: 29 km (Harvard-BU) | Combining QKD with classical Internet technology (i.e., TCP/IP) |
| QUESS | 3 | L: 7500 km T: n/a | Satellite-based QKD using quantum entanglement and quantum teleportation over vast distances |
| SECOQC Vienna | 6 | L: 85 km (STP-BRT) T: 200 km | Trusted QKD repeater technology, Q3P-protocol development |
| SwissQuantum | 3 | L: 17.1 km (CERN-hepia) T: 35.2 km | QKD durability in continuous operation |
| Tokyo QKD Network | 6 | L: 135 km (Koganei 1 - Otemachi 2) T: 148 km | Absolutely secure video transmission even under QKD eavesdropping |
