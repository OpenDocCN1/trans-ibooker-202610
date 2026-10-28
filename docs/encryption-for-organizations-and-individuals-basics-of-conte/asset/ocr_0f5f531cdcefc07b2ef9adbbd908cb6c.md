# SwissQuantum

Built to test the long-term performance of a QKD system, Swiss company *ID Quantique* built the *SwissQuantum* QKD network in 2009. The system ran for two years in the Geneva metropolitan area. SwissQuantum ran between three nodes: the *University of Genova (Unige)*, CERN, and *hepia (Haute Ecole du Paysage, d’Ingenierie et d’Architecture)*. Like its DARPA counterpart, SwissQuantum ran on dark fiber cables.

SwissQuantum used a three-layered approach in implementing its QKD network.

+   - **Quantum layer.** Consisted of point-to-point QKD links running on commercial QKD devices.
+   - **Key management layer.** This layer was responsible for secret key distribution across the network and between the other layers.
+   - **Application layer.** A layer which offers its users a graphical user interface for interacting with the QKD system.

The SwissQuantum system was monitored using three *virtual local area networks (VLAN)*. A single VLAN was assigned for each of the three layers. The system was protected by two firewalls; only ID Quantique, Unige, and hepia had access to the network.

SwissQuantum supported the similar BB84 and SARG protocols, of which the latter is more resistant against photon-splitting attacks. SARG is a better protocol for long distances and was thus chosen for use in the SwissQuantum network. The QKD key sifting was done in three steps: *error correction*, *privacy amplification*, and *authentication* of the related classical communications. The privacy amplification phase was implemented using universal hash functions.