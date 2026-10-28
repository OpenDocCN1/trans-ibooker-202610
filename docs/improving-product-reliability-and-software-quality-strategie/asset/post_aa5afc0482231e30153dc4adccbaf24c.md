
the design team. The component manufacturer may be unaware of a possible condition during turn-on where the component can turn on in a bad or locked state. If the device is an ASIC that uses multiple supply voltages, it likely will not be known if there any unlawful power on conditions. There are other electrical stresses that should be margined in combination and alone as well. These stresses are AC line input voltage and AC line frequency, clock frequency, and so on.

## 8.2 Highly Accelerated Stress Screening (HASS)

Once the reliability design issues identified from HALT have been designed out, the product is ready for manufacturing. However, a good design is only half the battle. Product reliability is achieved through both good design and manufacturing processes. Contrarily, design flaws and poor manufacturing processes result in field failures. The HALT process focused on the design issues that result in field reliability issues. There is also manufacturing process variation, which can produce product weaknesses that eventually lead to field failures. Can the HALT process be applied to manufacturing to prevent products from shipping which have an unacceptable process variation?

The HALT process can be applied indirectly to the manufacturing process. The process that is used in manufacturing is called HASS. HASS can prevent marginal and defective units from being sold. The purpose of HASS is to identify products that have process-related defects and manufacturing weaknesses before shipment. HASS is also helpful in identifying when suppliers provide potentially defective parts.

HALT and HASS use similar types of accelerated stresses to identify failures. Both processes also stress the product when it is powered up and operational. But the similarities stop there. HASS stresses the product in a similar way to HALT, but at reduced stress levels. HASS is a gentler form of HALT. HALT is a means to stress test the design and we emphasize the “T” for test. HALT is intended to reveal design-related failures. It is a proactive tool to improve product design and is performed by design engineering. HALT reveals failures that customers will experience in the field if the design is not fixed.

HASS is a stress screen with the last “S” used to emphasize screen. After products are manufactured, they pass through HASS to verify that the manufacturing process is in control. HASS is performed in manufacturing as a means of product acceptance. Process variations are flagged by HASS and can immediately be corrected to prevent an unsatisfactory product from being shipped. HASS is a reactive tool to assure that the manufacturing process stays in control.

The more complex the manufacturing process, the greater the opportunity for manufacturing defects to enter the product, rendering it less reliable. Control of the manufacturing process is critical. Even with the best manufacturing practices in place, that is, statistical process control (SPC), continuous improvement, electrostatic discharge (ESD) protection and training, manufacturing defects due to process drift and supplier issues can surface. HASS is intended to either pass or “screen out” nonconforming product. A HALT chamber, added to the end of the production line, which applies reduced stresses to the product can perform the vital HASS step. The HALT chamber and the HASS chamber can be the same. This is an option for small companies who outsource manufacturing or produce products in low volumes. Many companies prefer to avoid

![](img/50dc187750ab899846d825394ccbb93a_157_0.png)

# Figure 8.17 HASS stress levels.

the scheduling conflicts between design and manufacturing by having separate chambers for HALT and HASS. HASS chambers also tend to be bigger for batch processing.

From the HALT results, the product operating and destruct limits are learned. In HASS, the product is stressed at levels beyond its operating limits but below the destruct limits. The HASS profile consists of two parts, the precipitation screen and the detection screen. The test begins with the precipitation screen. The precipitation screen is a stress level that is below the destruct limit and above the operating limit. Refer to Figure 8.17. The HASS screening level applied to the product needs to be determined. A good stress level for temperature is between 80% and 50% of the destruct limits. The initial vibration stress level is set at 50% of destruct limits. It is important to stay below the destruct limits; otherwise damage to good product is likely. The purpose of the precipitation screen is to sufficiently damage anything that is a latent defect (meaning it is bad but not yet detectable as being bad) so it can be detected later in a detection test. However, the stress must not damage or severely degrade good product. Generally, if the right stress levels are applied, the defective assemblies will degrade at a significantly greater rate than good product. A proof of screen (POS) (discussed in Section 7.3.1), will identify if the precipitation stress is too severe or ineffective.

The precipitation stress was designed to sufficiently damage defective products, so it can be differentiated from good product. The way we identify bad products is through a detection screen. The detection screen applies temperature stress at levels that are between the soft failure limit and the product spec limit. Set the temperature stress midway between the spec limit and the soft failure limit. The vibration level is set between 3 and 5 Grms (often referred to as a tickle vibration). The HASS profile is usually short, typically three to five cycles of precipitation and detection is adequate.

### 8.2.1 Proof of Screen (POS)

The environmental stresses induced on the product by HASS will lower the life expectancy of the product. This is unavoidable. The goal of HASS is to provide a stress level high enough to precipitate identification of manufacturing defects without

removing an excessive amount of product life. How much product life is removed in HASS can be estimated through a process called POS.

The POS process is simple; just repeat the HASS screen until the product fails. Applying the HASS stress repeatedly causes the product to degrade at an accelerated rate. Eventually, the product will fail because of the accumulated effect of the stress. If it takes 20 times to render the product nonoperational, then it is reasonable to estimate that 5% off the product life is removed with each HASS screen. If the DUT failed after only four HASS screens, it can be assumed that 25% of the life of the product was removed each time. There is no minimum number of stress cycles desired before a product fails. Some companies want at least 20 cycles without a failure. The test should be run on a large enough sample to assure that normal manufacturing process variation is accounted for.

If, on the other hand, you run the HASS test for 100 cycles without a failure, the HASS stress levels may be set too low. Some practitioners recommend seeding product to determine if the HASS screen is effective at detecting defective product. Seeding a board requires intentionally inserted manufacturing defects into the product. The product is then tested to determine if the defects are found during HASS screen. The problem with seeded defects is that it is difficult to insert seeded defects that are real representations of product defects (i.e. manufacturing process drift or supplier changes).

Are there alternatives to HASS? Is HASS the only way to screen manufactured products for defects? No, there are other techniques but HASS is the most effective. The alternatives are burn-in, environmental stress screening (ESS), and of course electrical test with no environmental stresses at all.

### 8.2.2 Burn-In

The burn-in process can be applied to components and final products as a final acceptance test. Recall the bathtub curve from Chapter 6; Figure 8.18. The early failure rate of a product is often higher than the failure rate during its useful life. It is the reason some people prefer to buy a year-old car because “the bugs have been worked out.” Burn-in is designed to accelerate infant mortality so failures occur before the product is sold. Typically, infant mortality failures occur in the first year of product use. Product failures

![](img/50dc187750ab899846d825394ccbb93a_158_0.png)

Figure 8.18 The bathtub curve.
