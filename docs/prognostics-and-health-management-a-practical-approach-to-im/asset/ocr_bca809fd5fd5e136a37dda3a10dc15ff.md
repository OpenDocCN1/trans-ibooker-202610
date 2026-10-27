![](bbox=[0.1, 0.12, 0.85, 0.26])

Figure 6.22 Period-burst sampling: sampling period (TS) and burst period (TB).

failure – fast enough that detecting damage and estimating a future time of failure is, practically speaking, not viable. Sampling is performed to support diagnostics by detecting damage as soon as possible, with the intent to mitigate further damage to the system or a catastrophe due to failure to detect the fault. Use periodic sampling (see Figure 6.21, bottom) when the expected distance in time between the onset of damage and functional failure is sufficiently long: (i) there is no urgency to detect a fault within the distance in time between samples, and (ii) your customer wishes to defer maintenance to a future point in time – provided the customer is sufficiently confident in the accuracy of the prediction information from the PHM system.

# 6.3.2 Periodic-Burst Sampling

You should be aware that only using periodic sampling usually results in insufficient accuracy. It is highly likely that to support prognostics, you should use periodic-burst sampling (Figure 6.22). To do so, you need to (i) select the sampling period; (ii) use burst-mode sampling; (iii) select the sampling frequency; and (vi) select a burst period. Those design decisions must be made to meet the accuracy requirements of the prognostic information.

# Example 6.1 The SMPS you are to prognostic enable is a DC-DC converter that supplies power to EMAs (Figure 6.23). The power supply has an output of 24 VDC with an output filtering circuit of 4800 µF capacitance comprising 22 parallel-connected tantalum oxide capacitors that are known to fail short and burn open so that each failure of a capacitor reduces the filtering capacitance by 220 µF. The SMPS has a ripple-voltage

![](bbox=[0.1, 0.75, 0.75, 0.9])

Figure 6.23 Example of power supplies and EMA subsystems.