![](bbox=[0.14, 0.13, 0.64, 0.41])

# Figure 2.10 HALT result – 30 of 32 FPGA devices failed (Hofmeister et al. 2006).

+   - A single OPEN in a given temperature cycle is not counted as a FAIL event.
+   - The HALT results in 30 of the 32 FPGA devices failing, as shown in Figure 2.10.

Now, suppose a prognostic target, such as an FPGA attached to a fiber-resin (FR-4) printed wire board (PWB), which is formerly and sometimes still referred to as printed circuit board (PCB), is operated such that the temperature varies in any given 24-hour period of time from less than −40 °C to over 100 °C, with different temperature ramp-up rates, different dwell times at high temperature, different ramp-down rates, and different dwell times at low temperature. Also suppose that during any given 24-hour period of time, the PWB is subjected to different rates of different magnitudes of vibration and shock (such as might be experienced from being mounted in an engine compartment of a vehicle). Further, suppose that the PWB comprises over a dozen different FPGAs, some of which use standard PbSn solder, some of which use lead-free solder balls, some of which use plastic grid array (PGA) die packages with and without staking, and some of which use ceramic-column grid array (CCGA); all are mounted at different distances from centers of maximum stress-strain. Estimating with a high degree of accuracy when the primary clock-input pin of a specific FPGA will fail and thereby cause the PWB to fail becomes a daunting task (Javed 2014).

# 2.2.6 Failure Distribution

As the independent variables change in distribution models, the rate of change of a given curve varies, which creates a family of failure curves having a failure distribution with a TTF as illustrated in Figure 2.11. The failure distribution is a probability density function (PDF), and the TTF is the expectation or the 0.50 value of the cumulative distribution function (CDF) of that PDF: note that TTF is not the same entity as MTTF – more on this topic is presented in Chapter 7.