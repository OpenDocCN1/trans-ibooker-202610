![](bbox=[0.11, 0.12, 0.83, 0.51])

# Figure 2.20 Example of the output of a SMPS.

that include the following: thermal noise; ripple-voltage variations; switching noise; harmonic distortion; and responses to load variations such as damped-ringing responses, an example of which is shown at the bottom of Figure 2.20 (Hofmeister et al. 2017).

Noise contains information, and when noise in CBD is appropriately isolated and conditioned, leading indicators of failure can be extracted as feature data (FD). Useful FD at the output node of an SMPS includes, but is not limited to, ripple voltage (amplitude and frequency), switching amplitude and frequency, and other features found in a damped-ringing response. An important first step in prognostic-enabling an object such as a component, circuit, or assembly in an electrical system is to perform analyses, such as PoF and FMEA, to identify FD of interest associated with failure modes of interest.

# Example 2.12 Circuit, PoF, and FMEA analyses indicate that the identified failure mode should result in measurable changes in several features. Of those, you decide to focus on ripple voltage (increase in amplitude) and the damped-ringing response, increase in resonance frequency (f_R), instead of a change in decay time (1/τ) or a change in amplitude (A_R); see Figure 2.21.

# CBD: Prognostic Requirements

It is not cost-justifiable to select all applicable features of CBD for use as FD in signatures: you need to select one or more FD that lets you meet prognostic requirements. Important requirements include the following: