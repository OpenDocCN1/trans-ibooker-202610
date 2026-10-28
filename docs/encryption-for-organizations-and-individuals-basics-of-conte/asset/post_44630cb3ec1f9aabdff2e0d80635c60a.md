
## Photon Number Splitting (PNS)

Using the BB84 protocol, Alice would be ideally sending Bob a single photon at a time. However, due to the limitations of creating single photons under most circumstances, sometimes more than one photon is sent during transmission with BB84. This paves the way for *photon number splitting*. In this attack, Eve the eavesdropper can intercept some photons while letting others reach their intended recipient that is, Bob. Due to negligible rates of quantum errors in this attack vector (i.e., photon loss), both legitimate parties will assume to be in transmission with each other only. This leads to a scenario in which Eve knows exactly what Alice is trying to communicate to Bob while remaining completely invisible herself (Zhao, 2018).

A family of protocols free of key sifting has been proposed to provide resistance to photon number splitting (Grazioso & Groshans 2013). The decoy state protocol, too, offers protection against this variety of attack.

> The *decoy state protocol* uses several settings of intensity in the transmitter side. Only one of these settings contains valuable information; the rest are decoys. Only after each transmission is the intended intensity setting revealed. This approach offers good protection against the photon number-splitting attack. The decoy state protocol has been confirmed to work over distances of at least 100 kilometers.

## Denial of Service

Instead of flooding a server with requests as is the case in a classical denial-of-service (DoS) attack, in quantum-based settings malicious actors can simply cut fiber optic cords. In the case of unsealed optical quantum transmission devices, blocking the line of sight between these units also does the trick. A quantum DoS is there primarily to disrupt any proceedings and not to gather intelligence.

## Trojan Horse

Its name notwithstanding, the *Trojan horse attack* in a quantum context shares very little with its classical counterpart. Instead of a malicious software package, the quantum-based Trojan horse uses the approach of shining a bright beam of light into the quantum channel and examining its back reflections. With only a handful of reflected photons, an eavesdropper can deduce the basis choice of one of the legitimate parties (Nitin et al. 2014).

*A Trojan horse attack in the context of QKDs is also known as a large pulse attack.*

## Intercept and Resend (IR)

The *intercept and resend* attack is exactly what it sounds like. An eavesdropper called Eve intercepts photons Alice intends to reach Bob. Eve replaces these photons with another set which she has already measured. Statistically speaking Eve is successful in getting the basis right 50% of the time. In turn Bob is also right 50% in deducing Eve’s basis. This results in a detectable error rate of 25% (50% of 50% equals 25%).

A variation of this attack known as the *Intercept and Resend with Faked States (IRFS)* does not focus on guessing any original basis states. Instead Eve’s emphasis is on producing pulses of light detectable by Bob. This provides a cover for eavesdropping; Alice and Bob think they are operating with unaltered quantum states, unaware of Eve’s influence on the proceedings. This can reveal the full secret key/password without raising alarm via spikes in the quantum error rate (Lizama-Pérez et al. 2016).

## Thermal Blinding Attack

Sometimes bright light is all it takes to compromise a QKD-based system. A type of attack known as *thermal blinding* uses continuous-wave illumination and short, bright trigger pulses to manipulate voltages in a quantum circuit. This can reveal the full key/password without raising the qubit error rate, which would under most circumstances raise an alarm. As of 2020, at least the *Clavis2* QKD system has been compromised using this type of attack vector.

Unlike photon counters, *homodyne sensors* may be able to ward off thermal blinding attacks with a considerable rate of success. The technology was also fundamental in demonstrating quantum entanglement.

## Man in the Middle

A common type of attack in the classical world, *man in the middle* is also a possibility during any unauthenticated use of QKD. In particular, the calibration phase when establishing a QKD connection may be exposed to this type of attack. By installing a malicious precondition into the signal exchange, an eavesdropper may receive full details on the final key (see Figure 12-1). The BB84 protocol and its derivatives in particular are vulnerable to this attack (Fei & Meng, et al., 2018).

![](img/df9b6c40c665f58cab0c435444990c63_251_0.png)

Figure 12-1. MITM during quantum key distribution. Alice sends calibration signals to Bob only to have them intercepted by Eve, who in turn sends her faked signals to Bob. The T denotes a single cycle in a single pulse of photons

Before quantum key distribution can be initiated, a quantum channel must be established with single-photon detectors. These detectors will be calibrated using a process known as line length measurement (LLM). During a typical LLM, Alice sends calibration signals to the detectors at Bob’s disposal. This process includes gauging the channel length and the relative delay between the arrival timing of the pulses. The activation timing of each detector is scanned electronically to discover the timing when the count rates are at maximum (Fei & Meng, et al., 2018).

The receiver, Bob, has photon detectors in place, but he’s not aware of Eve the eavesdropper who is actually sending him faked signals. Eve creates discrepancies between the activation timing of the various detectors by replacing the calibration signals with phony ones. These are to contain more than one pulse in each cycle as opposed to the signals sent by Alice, which have one pulse per cycle. The arrival timings of responding signals are denoted as t0 and t1 in Figure 12-1. Eve wants to know the activation timing for each detector (i.e., t0 or t1). This can be solved by a time-shift attack (TSA) in the beginning of the QKD process during which Eve chooses the arrival timing of signals between t0 and t1 at random (Fei & Meng, et al., 2018).

Now, if this interception of the LLM is successful, Eve will be in complete control over the quantum channel in question while Bob will remain blissfully unaware; this approach constitutes the man-in-the-middle attack in the quantum realm.

## The Hardware of QKD

Having looked into potential attack vectors, let’s next take a peek at the hardware components a QKD system includes running on the BB84 protocol. The system we are about to explore was based on the work by researchers at *Toshiba Corporate Research Center, Toshiba Research Europe Ltd.*, and the *Quantum ICT Laboratory* in Japan.

This system was successfully implemented on a 45-kilometer stretch of a metropolitan telecom network in Tokyo. The QKD transmissions were carried over fiber optic cables (Dixon & Dynes et al., 2017).

## Component Breakdown

Next, we’ll go through the main components of the Tokyo QKD system; Figures 12-2 and 12-3 show the basic parts of the transmitter and receiver devices the network utilized. These will be discussed in detail.

![](img/df9b6c40c665f58cab0c435444990c63_252_0.png)

*Figure 12-2.* A layout for a secure QKD transmitter device

![](img/df9b6c40c665f58cab0c435444990c63_253_0.png)

The presented design uses *Mach–Zehnder interferometer*. This refers to the technology used to examine relative phase shift variations between two beams derived by splitting light from a single source. Both the transmitter and the receiver devices in the design operate on these principles and contain the necessary hardware to do so.

## Field Programmable Array Gate (FPGA)

Found in numerous types of devices, *field programmable array gates* (FPGAs) are semiconductor circuits consisting of a matrix of logic blocks. These are highly configurable units, unlike *application-specific integrated circuits (ASIC)* which are designed for a specific, usually fixed purpose (e.g., CPUs in classical computers). An FPGA is reprogrammable should the need to implement new tasks and/or features arise; it’s the perfect prototyping solution. Like you probably noticed from Figures 12-2 and 12-3, an FPGA is a central component to both the receiver and the transmitter device.

In the context of the QKD system presented in this chapter, the FPGAs run at a 1 Gigahertz transmission rate.
