![](bbox=[0.113, 0.104, 0.607, 0.684])

**Figure 6.81** Plots of the RUL, PH, and SoH estimates produced by ARULEAV.

# Final Design Specifications

The customer approves your changes to the design specifications for prognostic-enabling of the SMPS for degradation of the capacitance in the output filter: (i) use the advanced version, ARULEAV (PIPMOD = 1); (ii) use FFP to DPS signature transform (XDPS = 3); (iii) use 0.45 as a power value (XDPSNV = 0.45); and (iv) use a complex signature model in ARULEAV (PIFMOD = 4).

Continue to use the remaining design specifications: (i) NM (FNM = 2.5); (ii) FFP failure threshold (FAILM = 70); (iii) number of points to calculate nominal FD value (FD0PTS = 20); (iv) number of rolling FD points for data smoothing (FDPTS = 4); (v) number of rolling FFP points for data smoothing (FFPPTS = 4); and (vi) number of rolling FFS points for data smoothing (FFSPTS = 4).