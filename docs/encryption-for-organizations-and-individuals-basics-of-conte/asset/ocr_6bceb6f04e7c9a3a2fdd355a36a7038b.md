# CHAPTER 13　IMPLEMENTATIONS OF QKD

The DARPA system incorporated several QKD distribution protocols into a single protocol stack. Privacy was enhanced using *universal hashing*, in which an algorithm is chosen at random from a family of hashing algorithms. This approach contains very few probabilities for collisions and spans the mathematical domains of integers, vectors, and strings. Key sifting in DARPA was achieved by either traditional means, such as run-length encoding (RLE), or using the SARG04 protocol.

SARG04 by Scarani et al. is a more robust version of the ubiquitous BB84 protocol offering improved resistance against photon-number-splitting attacks. SARG04 works best in scenarios where attenuated laser pulses are used instead of individual photons.

> *Run-length encoding (RLE)* is the simplest lossless form of data compression dating back to the late 1960s. Instead of including repeated sets of data, RLE stores only single sets of these duplicate repeating bits while recording the amount of times these sets are to be repeated.

# Secure Communication Based on Quantum Cryptography (SECОQC)

In 2003 a grand European cryptographic project was initiated. The SECOQC was created to study the practical applications of quantum key distribution technology in the context of the Q3P protocol. The project’s participants included 41 organizations from the European Union, Russia, and Switzerland. We’ll next examine the findings of this rather large team of experts.

Now, there are two primary types of QKD networking approaches.