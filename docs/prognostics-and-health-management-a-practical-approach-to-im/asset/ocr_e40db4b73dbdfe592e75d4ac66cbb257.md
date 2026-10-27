# EMA 51: RUL & PH ESTIMATES from ARULEAV

![](bbox=[0.11, 0.1, 0.6, 0.37])

# SoH ESTIMATES from ARULEAV

![](bbox=[0.11, 0.4, 0.58, 0.65])

Figure 6.85 EMA (load) plots: RUL, PH, and SoH estimates using DPS-based FFS and ARULEAV.

one data point (time and amplitude) at a time, which results in data and performance metrics: Figure 6.85 and Table 6.3.

From the logged output file, you collect and calculate the following information (in hours): BD was estimated to occur at 441 – a difference of 3 hours from the 444 hours when degradation actually began; BDDET at 504, BD_OFF is calculated as 63 using the estimated BD; EOL was detected at 3168 – just prior to functional failure; EOL was estimated at 3164; PD_MAX is calculated as 2727 (3148 minus 441); PD_FOM is calculated as 97.7%; PH_25 was reached at 576; and PH_10 was reached at 1368. From that information, you calculate the following: χ_25 is 97.3% from ((2727 – (576 – 441))/2727) and χ_10 is 68.3%, which are excellent results and exceed our recommended targets of 70% and 35%. See Table 6.4 for a summary of results.

The plots and performance metrics confirm that an FFP-to-DPS transform is not necessary: the FFP signature is only slightly curvilinear. In fact, after plotting (Figure 6.85) and other analyses, you and the customer are ecstatic with the RUL, PH, and SOH