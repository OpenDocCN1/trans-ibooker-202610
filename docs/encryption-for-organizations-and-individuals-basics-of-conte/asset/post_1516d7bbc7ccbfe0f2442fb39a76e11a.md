
## Optical Attenuator, Isolator, and Narrow Band Pass Filter

The optical attenuator (A), isolator (I), and narrow band pass filter (F) components work together to reduce the amount of reflected light (see Figure 12-2). In a QKD setting they are responsible for attenuating the photon pulse levels so that information leakage isn’t an issue in the context of a quantum Trojan horse attack.

## Laser Diode, Intensity Modulator, and Phase Modulator

The *laser diode (LD)* component produces photon pulses, that is, the main units of information in a QKD system (see Figure 12-2). An *intensity modulator (IM)* provides security against photon-splitting attacks by adding decoy states into the stream.

A *phase modulator (PM)* component in this QKD solution can encode four discrete phase values onto the photon pulse; a PM on the receiver’s device is then used to decrypt these values.

## Beam Splitter and Polarizing Beam Splitter

Beam splitters (BS) are used by the receiver to extract the polarization data of each photon particle. A *polarizing beam splitter (BMS)* ensures that photons travel through opposite paths in the interferometer pair and always arrive at the final beam splitter.

## Monitoring Detector

Implementing a *monitoring detector (MD)* into a receiver’s device helps to ward off thermal blinding attacks. This optical power monitor receives around 1% of the device’s input. This is enough to detect any suspicious influx of heat energy typical in thermal blinding.

## Delay Line

An optical *delay line (DL)* is a component which protects against Trojan horse attacks directed at the phase modulator (PM). The delay line makes it impossible for a malicious actor to detect any reflected light from the phase modulator unit before a modulated photon has been detected by Bob/the receiver.

## Electronic Polarization Controller

An *electronic polarization controller (EPC)* in a receiver device functions to mitigate environmental disturbances during QKD operation (see Figure 12-3). An EPC is placed before the receiver’s interferometer to compensate for the constant polarization rotations in the transmission fiber.

## Fiber Length Stretcher

The path lengths of the fiber optic based interferometers in the transmitter and receiver are potentially influenced by fluctuations in temperature. To compensate for this, an electrically driven *fiber length stretcher (FS)* is placed in the receiver’s interferometer.

## Avalanche Photodiodes and Self-Differencing Circuits

The so-called avalanche photodiodes (APDs) are used for photon detection on Alice’s/the receiver’s device. A diode is simply a device which converts light into electricity. An APD is a type of diode with signal-amplifying properties. The APDs in the QKD system presented were cooled to -30° Celsius for optimal performance.

Also a part of the presented system were *self-differencing circuits (SD)* which allow the avalanche photodiodes to work effectively in the so-called Geiger-mode, without excessive levels of signal noise. In this mode a device’s output pulses create “on/off” trigger events. Avalanche photodiodes operating in Geiger-mode are some of the most optimal components available today for detecting single photons.

## The Externals

All of this advanced technology is housed under a rather minimalistic steel chassis as can be seen from Figure 12-4. Both the transmitter and the receiver devices measure 19″/48 cm in width and 5.25″/13.3 cm in height.

![](img/df9b6c40c665f58cab0c435444990c63_256_0.png)

*Figure 12-4. A QKD transmitter and receiver*

> 1 Image source: www.ncbi.nlm.nih.gov/pmc/articles/PMC5434053. Used under Creative Commons 4.0 International License (https://creativecommons.org/licenses/by/4.0/).

## In Closing

After finishing this chapter, you will have learned the following:

- The major cryptographic attacks in the realm of QKD
- Which hardware components a secure QKD system consists of

In the next chapter we’ll explore other current implementations of QKD, including *SwissQuantum*, *Tokyo QKD Network*, and others.

## References

Dixon, A. R., Dynes, J. F., Lucamarini M. et al. Quantum key distribution with hacking countermeasures and long term field trial. 2017. www.ncbi.nlm.nih.gov/pmc/articles/PMC5434053

Luis Adrian Lizama-Pérez, José Mauricio López, and Eduardo De Carlos López. 2016. Quantum Key Distribution in the Presence of the Intercept-Resend with Faked States Attack. Entropy/MDPI 19.

Yusheng Zhao. 2018. Development of Quantum Key Distribution and Attacks against It. IOP Conf. Series: Journal of Physics: Conf. Series 1087.

Fabio Grazioso, Frédéric Grosshans. 2013. Photon-Number-Splitting-attack resistant Quantum Key Distribution Protocols without sifting. Physical Review A, American Physical Society.

J. Nitin, E. Anisimova, I. Khan et al. 2014. New Journal of Physics 16. Trojan-horse attacks threaten the security of practical quantum cryptography.

B. Ouchao, A. Jakimi. International Journal of Advanced Engineering, Management and Science, June 2018. Performance Evaluation of Secure Key Distribution Based on the B92 Protocol.

Yang-Yang Fei, Xiang-Dong Meng, Ming Gao, Hong Wang & Zhi Ma. 2018. Quantum man-in-the-middle attack on the calibration process of quantum key distribution. Scientific Reports volume 8.

## CHAPTER 13

## Implementations of QKD

Chapter 12 provided a basic overview of the hardware components in a quantum key distribution (QKD) system. We also explored the Tokyo QKD Network built to test the threat resilience of QKD. In this chapter we’ll delve into larger implementations of this technology, exploring the basics of a total of five major QKD networks.

## The DARPA Quantum Network

The honor of being the world’s first quantum network belongs to *The DARPA Quantum Network*. Funded by the *Defense Advanced Research Projects Agency* of the US Department of Defense, this QKD system was operational between 2002 and 2007. The DARPA Quantum Network was built by *BBN Technologies* collaborating with Harvard University and the *Boston University Photonics Center (BUPC)*.

DARPA ran on unused fiber optic cable (i.e., dark fiber) beneath the streets of Boston and Cambridge, Massachusetts. The system was compatible with conventional Internet technology. It consisted of ten optical nodes, connecting Harvard University with Boston University.

## CHAPTER 13 IMPLEMENTATIONS OF QKD

The DARPA system incorporated several QKD distribution protocols into a single protocol stack. Privacy was enhanced using *universal hashing*, in which an algorithm is chosen at random from a family of hashing algorithms. This approach contains very few probabilities for collisions and spans the mathematical domains of integers, vectors, and strings. Key sifting in DARPA was achieved by either traditional means, such as run-length encoding (RLE), or using the SARG04 protocol.

SARG04 by Scarani et al. is a more robust version of the ubiquitous BB84 protocol offering improved resistance against photon-number-splitting attacks. SARG04 works best in scenarios where attenuated laser pulses are used instead of individual photons.

> *Run-length encoding (RLE)* is the simplest lossless form of data compression dating back to the late 1960s. Instead of including repeated sets of data, RLE stores only single sets of these duplicate repeating bits while recording the amount of times these sets are to be repeated.

## Secure Communication Based on Quantum Cryptography (SECOQC)

In 2003 a grand European cryptographic project was initiated. The SECOQC was created to study the practical applications of quantum key distribution technology in the context of the Q3P protocol. The project’s participants included 41 organizations from the European Union, Russia, and Switzerland. We’ll next examine the findings of this rather large team of experts.

Now, there are two primary types of QKD networking approaches.

- 1. **Quantum channel switching.** An end-to-end quantum channel is used between transmitter and receiver (e.g., Alice and Bob).
- 2. **Trusted repeater.** The keys are transported over many nodes (i.e., intermediate locations) which are deemed trustworthy.

Participants in the SECOQC project focused on researching the implementation of a six-node trusted repeater network (see Figure 13-1). This came to fruition in Vienna in 2008.

![](img/df9b6c40c665f58cab0c435444990c63_260_0.png)

Figure 13-1. The node layout of the SECOQC network

The nodes were all located within Vienna, apart from St Pölten (node 1) which was hosted by a repeater station on a communication line from Vienna to Munich, Germany (see Table 13-1).
