
![](img/05efaadbca64ad00f9e8d44c1db8cc7b_363_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_363_1.png)

**Figure 7.9** Estimated PD and actual PD: EMA winding (top) and EMA transistor (bottom).

fails to meet the requirement that unexpected failures will be less than 5%, it establishes a baseline estimate of cost.

Your customer arranged for special delivery of 6 power supplies and 12 EMAs. Those 18 units were subjected to end-use tests similar to HALTs (refer back to Section 2.2). Your team assisted in the design of the experiment and building of the test beds; the tested units (power supplies and EMAs) needed to be prognostic enabled, as described in Chapter 6. For analysis purposes, test failures are to be evaluated as though all of the tested units were installed at the same time and failed in the sequences and times indicated by the test.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_364_0.png)

**Figure 7.10** RUL and PH plot for PITTF0 = 4800 and for PITTF0 = 2290.

The numbers to be used in a cost-benefit analysis are provided by the customer and listed in Tables 7.1 and 7.5. After examining the test results (see Figure 7.15 and Table 7.6), your customer concludes the cost-benefit analysis for the power supply scenario is sufficient for evaluation of the five approaches.

**Table 7.4** Tabulated calculations using enhanced program with PITTF0 = 4800.

Comparison of PD(EST) before and after ARULEAV is enhanced.

PITTF0 = 4800 (h): Initial PD (PD0) = PITTF0 \* PITTFADJ.

| LRU type | BD detected | Failure detected | PD measured | PD0 - PD Before | PD0 - PD After | % Difference Before | % Difference After |
|---|---|---|---|---|---|---|---|
| EMA | 504 | 3168 | 2664 | 4296 | 3646 | 61% | 37% |
| EMA | 960 | 4440 | 3480 | 3840 | 3190 | 10% | -8% |
| EMA | 1104 | 2760 | 1656 | 3696 | 3046 | 123% | 84% |
| SMPS | 1368 | 4200 | 2832 | 3432 | 2782 | 21% | -2% |

### 7.3.2 Cost Analyses

**No PHM Approach**

In this scenario, the system runs until a unit fails – a “do nothing until failure occurs” approach. When failure occurs, the failed unit is removed and replaced. The system is restarted and runs until the next unit fails, and so on, until the end of a sustainment period of 14 400 hours (600 days of operation):

- On average, #1 power supplies would fail and be replaced: 14 400/4200 = 3.4 times:
    - Cost = 3.4\*10000 + 0\*2000 + 3.4\*4000 = 47 600

Repeating this for the remaining five power supplies results in 24 removals and replacements because of unexpected outages due to degradation failure, at a baseline cost of $336 000.

**Usage-Based MTTF Approach**

In this scenario, all power supplies are replaced when usage equals 3592 hours (MTTF). Examination of the data in Table 7.6 shows that power supplies #3, #4, and #6 would functionally fail before they are replaced. In the sustainment period, a total of 27 power supplies would be removed and replaced, at a total cost of $354 000 – an increase of $18 000 per system during the sustainment period to reduce the number of unexpected outages from 24 to 14. However, 52% of all repair and removal actions would be attributable to degradation failures. This approach fails to meet requirements.

**Usage-Based 2/3 MTTF Approach**

In this scenario, all power supplies are replaced when usage equals 2395 hours. There would be no unexpected outages, but 36 power supplies would be replaced at a cost of $432 000 – an increase in cost of $96 000 and a reduction of unexpected failures to zero. That $96 000 becomes $96.0 million for a population of 1000 systems.

**Damage-Detection Approach**

In this scenario, whenever damage is detected in a power supply, it is removed and replaced within 720 hours, which would also, for these supplies, result in zero unexpected outages. This approach is seemingly a good one because there would be

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_367_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_367_1.png)

**Figure 7.12** RUL and PH plots for EMA 51: before and after PITTFADJ = 2.0.

**SoH at 75% or Less Approach**

In this scenario, whenever a prognostic SoH estimate is 75% or less for a power supply, it is removed and replaced within 120 hours: again, for this approach, there would be zero unexpected outages. This approach would result in the removal and replacement of 28 power supplies during the sustainment period at an estimated cost of $336 000 per system – neither a savings nor an increase in cost compared to the baseline cost of a “do nothing until failure” approach.

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_368_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_368_1.png)

**Figure 7.13** RUL and PH plots for EMA 61: before and after PITTFADJ = 2.0.

Yes, there is the cost of the sensors and PHM systems, but the SoH approach has the following advantages:

- It reduces unexpected outages due to degradation failure to zero.
- It is less expensive than the next-most-expensive approach (usage-based 2/3 MTTF).
- It has the fewest repair and replace activities (28) compared to the usage-based 2/3 MTTF approach (36) or the damage-detection approach (43).

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_369_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_369_1.png)

**Figure 7.14** RUL and PH plots for EMA 62: before and after PITTFADJ = 2.0.

- It requires the smallest number of repair hours and thereby increases mission-availability time.
- It does not have the drawback of unexpected early onset of degradation and/or faster-than-expected time from onset to failure – which are drawbacks of the usage-based 2/3 MTTF approach.
- It saves $180 million compared to the damage-detection approach for a 1000-system network over a 14 400-hour sustainment period.

**Table 7.5** Cost estimates for benefits evaluation of prognostic enabling.

*Estimated costs: use for benefits analysis; Sustainment (Use)*

| LRU name | Acquisition | Scheduled R&R | Unplanned failure | Expected life (h)/LRU | Period (h)/LRU |
|---|---|---|---|---|---|
| Power supply | $10 000 | $2 000 | $4 000 | 3 500 | 14 400 |
| EMA | $25 000 | $3 000 | $6 000 | 3 500 | 14 400 |

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_370_0.png)

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_370_1.png)

**Figure 7.15** Plots: test results for six power supplies (top) and 12 EMAs (bottom).

wear-out; instead, failure is most often due to cyclic stresses and strains that cause fatigue damage that accumulates (Hofmeister et al. 2006). As fatigue damage accumulates, one or more measurable signal changes, which can be captured as signature data.

So, there is really nothing about a bathtub curve that can be used to enable or to support CBD-based prognostics.

### 7.4.1 Bathtub Curve: MTBF and MTTF

As you can see in Figure 7.16, MTBF (between failures) is not a time-axis value: it is a failure-rate value. Neither MTTF nor MTBF is seen in a typical view of a bathtub curve, perhaps because of the relationship suggested in Figure 7.17 (Seastrunk 2016).

### 7.4.2 Trigger Point and Prognostic Distance

Also shown in Figure 7.16 is a conceptual diagram intended to convey the notion that it is possible to employ a prognostic trigger to provide advance warning of a probable failure within time PD while, at the same time, conveying a notion that useful life does not extend into the wear-out region. Figure 7.18 conveys a more practical view of prognostic trigger points:

- Envision multiple sensors attached to a complicated product such as an EMA with a built-in power supply, as already described in this book.

and MTBF; instead, a rationale was presented for using the time between the onset of degradation and the time when such degradation results in functional failure: TTF. Methods to determine or calculate a value for TTF include service life, end-use testing, and MTTF-based methods. A section on cost-benefit analysis of prognostic approaches included example approaches for the following approaches: (i) no PHM; (ii) usage-based MTTF; (iii) usage-based 2/3 MTTF; (iv) damage-detection; and (v) SoH at 75% or less. The section after that focused on the bathtub curve and how it relates to failure distributions, MTBF, MTTF, trigger points, and CBD signatures.

By no means does this book cover the entirety of information related to PHM and CBD – conditioning, modeling, and processing for CBM. On the other hand, this book contains a wealth of information dealing with basic approaches and, importantly, CBD signatures and how to process and linearize those signatures, which lessens the burden on prediction programs and improves the accuracy of prognostic information. Chapter 6 presented the design of a hypothetical prototype PHM system to illustrate the challenges a designer might face and to demonstrate the application of the approaches discussed in this book.

## References

- Abernethy, R.B. (2006). *The New Weibull Handbook*, 5e, 2006. Berringer and Associates.
- Astfalck, L., Hodkiewicz, M., Medjaher et al. (2016). A modelling ecosystem for prognostics. Annual Conference of the Prognostics and Health Management Society.
- DAU (2018). Failure modes & effects analysis (FMEA) and failure modes, effects & criticality analysis (FMECA). Acquipedia, Defense Acquisition University, 9820 Belvoir Road, Fort Belvoir, VA 22060. https://www.dau.mil/acquipedia.
- Ellerman, P. (2012). *Calculating Reliability Using FIT & MTTF: Arrhenius HTOL Model*, MicroNote™ 1002, Rev 0. MicroSemi Corp. https://www.microsemi.com/document-portal/doc_view/124041-calculating-reliability-using-fit-mttf-arrhenius-htol-model (accessed 2018).
- Hofmeister, J.P., Lall, P., and Graves, R. (2006). In-situ, real-time detector for faults in solder joint networks belonging to operational, fully programmed field programmable gate arrays (FPGAs). In: *Proceedings of the IEEE AUTOTESTCON, Anaheim, CA, 18–21 Sept. 2006*, 237–243. IEEE.
- Johnston, A. (2010). *Reliability and Radiation Effects in Compound Semiconductors*, 117–132. Singapore: World Scientific Publishing Co. Pte. Ltd.
- O’Connor, P. and Kleyner, A. (2012). *Practical Reliability Engineering*. Chichester, UK: Wiley.
- RAC (2005). *Reliability Toolkit: Commercial Practices Editions*. Reliability Analysis Center, Rome Laboratory. https://www.dsiac.org/sites/default/files/journals/2Q2005.pdf (accessed 2018).
- Ridgetop Group. (2018). Sentinel Network, view of the graphical user interface (GUI) for an electronic power supply (EPS). Courtesy and permission of Ridgetop Group, Inc., 3580 West Ina Road, Tucson, AZ, 85741.
- Seastrunk, B. (2016). Reliability, warranty, and why nothing lasts as long as it used to. http://opinionbyopen.com/reliability-warranty-and-why-nothing-lasts-as-long-as-it-used-to (accessed 2018).
