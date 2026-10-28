
The HASS profile consists of two parts, the precipitation screen and the detection screen. The test begins with the precipitation screen. The precipitation screen is a stress level that is below the destruct limit and above the operating limit. The precipitation screen accelerates process defects to failure (refer to Figure 21.12). The HASS screening level applied to the product needs to be determined. A good stress level for temperature is between 80% and 50% of the destruct limits. The initial vibration stress level is at 50% of destruct limits. It is important to stay below the destruct limits; otherwise damage to a good product is likely. The purpose of the precipitation screen is to sufficiently damage defective products so it can be detected in test. Bad products are identified as the assembly passes through the detection screen.

During detection screening, the temperature stress is reduced to levels below the soft failure limit but above the product specification limit. The HASS profile is usually short, typically three to five cycles of precipitation and detection is adequate to detect failures before they occur during customer use.

Detection and precipitation screen is performed as one operation. You increase temperature past the soft failure range but below the damage level (this is the precipitation phase), then the stress is reduced below the soft failure level where a “good” assembly will recover (this is the detection range). If it doesn’t recover, then a defect has been detected.

The HASS profile must not damage or severely degrade good products. Generally, if the right stress levels are applied, the defective assemblies will degrade at a significantly greater rate than good products so that they can be easily detected. The HASS profile must also be severe enough to precipitate a process defect to failure. A proof of screen (POS) is used to ensure that the HASS levels are not damaging good product by removing too much product life but effective enough to identify defective units.

The environmental stresses induced on the product by the HASS screen will remove some of the life expectancy of the product. This is unavoidable. The goal of the HASS screen is to provide a stress level high enough to precipitate manufacturing defects without removing an excessive amount of product life. How much product life is removed in HASS can be estimated through the POS.

![](img/50dc187750ab899846d825394ccbb93a_341_0.png)

Figure 21.12 HASS stress levels.

The POS process repeats the HASS screen over and over again on a good product until it fails. Each HASS screen removes some product life. Applying the HASS stress repeatedly causes the product to continually degrade at an accelerated rate. Eventually, the product will fail because the stress continually degrades the product until it has reached wear out. If it takes 20 HASS cycles to render the product nonoperational, then it is reasonable to estimate that 5% of the product life is removed with each HASS screen. If the device failed after only four HASS screens, it can be assumed that 25% of the life of the product was removed each time. There is no minimum number of stress cycles desired before a product fails. Some companies want at least 20 cycles without a failure. The test should be run on a large enough sample size to ensure that normal manufacturing process variations are accounted for.

If, on the other hand, you run the HASS test for 100 cycles without a failure, the HASS stress levels may be set too low. Some practitioners recommend seeding products when doing POS to determine if the HASS screen is effective at detecting defective products. Seeding a board requires intentionally inserting a known manufacturing defect(s) into the product. The product undergoes HASS to determine if the defects can be found during the HASS screen. The problem with seeded defects is that it is difficult to insert seeded defects that are real representations of product defects (i.e. manufacturing process drift or supplier changes).

## 21.4 Highly Accelerated Stress Screen (HASS)

After HALT has completed and design changes implemented a re-HALT is performed to validated the effectiveness of the design fixes and ensure no new reliability issues were created in the respin/redesign. One of the outputs from HALT is identification of the product’s soft and hard failure limits. The results from HALT were then used to develop a HASS profile for screening production/manufacturing material. Before the HASS profile can be released to production, it must go through a POS to verify the effectiveness of the HASS profile and ensure the HASS profile will not damage or degrade good product. The results is a HASS profile that is released to manufacturing for early production to help accelerate design maturity and ramp readiness. The result is a profile like Figure 21.13 for production HASS.

## 21.5 Operate FRACAS

A FRACAS was installed in the previous phase. The FRACAS database (often a purchased software program) is customized for the user’s particular application. The customization of the database is part of the installation and checkout process for FRACAS.

With the product now in the design validation phase, prototypes are built and tested to validate the performance of the product. During development testing and design validation, failures will occur in the product. There is a natural tendency, especially early in the development phase, to treat these failures informally. They are often noted in a notebook, a piece of paper, or sometimes are fixed without any documenting at all. Treating any failure as irrelevant is shortsighted no matter how insignificant the failure may seem. These failures often resurface later in the program when it is more costly to rectify.

| Variables |  |  |  |  |
|---|---|---|---|---|
|  | Temp. (C) | Vib. (Grms) | HALT Hard Fails Temp | Vib |
| Start/End | 20 | 0 |  |  |
| Min | -40 | 0 | -50 |  |
| Max | 80 | 30 | 100 | 60 |
| Detect H | 40 | 3 |  |  |
| Detect L | 10 | 3 |  |  |
| Hot Soak | 20 min. |  |  |  |
| Cold Soak | 15 min. |  |  |  |
| Diagnostic run time | 15 min. |  |  |  |
| Thermal Ramp | 60 C/min |  |  |  |
| Transmissibility |  |  |  | 40% |
| Total GRMS Minutes | 1,016 |  |  |  |

![](img/50dc187750ab899846d825394ccbb93a_343_0.png)

### Figure 21.13 HASS profile.

FRACAS prevents this from occurring. FRACAS becomes operational once the first prototypes are built. Every failure that occurs from that time on is recorded in the FRACAS database. The failures are recorded as well as the activity to determine the root cause and corrective action. FRACAS will track the progress being made to resolve failures as well as indicate the rate at which new failures are occurring. This information will provide a good indication as to how fast the design is maturing or if it is still in a state of flux.

For FRACAS to be effective, everyone who identifies a failure must use it. Preventing designers from using their own system to document failures during prototype can be a difficult challenge. The problem can be resolved by providing training in the previous phase on how to use the FRACAS database. This, in conjunction with a commitment from the management that all designers will use the FRACAS database to record every failure, will ensure success.

## 21.6 Design FMEA

In the product design phase, a failure modes and effects analysis (FMEA) was performed on every subassembly, circuit board, and at the system level. There was a significant amount of effort and resources required to complete the task. The FMEA in the design validation phase is not intended to repeat the previous effort but to complement it. The design validation FMEA complements the previous effort by only evaluating significant design changes (ECOs) that resulted out of design validation, HALT, and other design-related failures. AN FMEA is not required for simple ECOs, such as a component value change. Significant ECO changes usually result in a new board or mechanical layout. The design FMEAs that are conducted in the design validation phase only address that which has changed. The FMEA is not repeated for the entire assembly; only significant changes made to the design need to be analyzed using with an FMEA. The time required to perform an FMEA for a design change will be significantly less than the time required for the original FMEA.

## 21.7 Closure of Risk Issues

At the end of the design validation phase, the product is complete and ready for market. The high-risk issues captured earlier should be closed before the end of the design validation phase. There should be no unresolved high-risk issues; it doesn’t matter if the risk is a design, manufacturing, test, supplier, or a reliability issue. Any unresolved high-risk issues represent escapes in the risk mitigation process. Each high-risk issue has a contingency (backup) mitigation plan that should resolve the risk issue before entering the production phase. If a high-risk issue has not been resolved before entering the production phase, is required.

Escalation of unresolved high-risk issues is required because these issues often become costly problems once a product is on the market. The escalation process starts well before the completion of the design validation phase. Escalation begins by elevating the problem to senior management. Often, these problems are related to the way in which the risk is being managed, the type of resources used to solve the problem, or the skills of the people working to fix the problem. Senior management must determine why the problem is not being resolved and implement changes to fix it.

At the end of the design validation phase, the product is ready to be sold.
