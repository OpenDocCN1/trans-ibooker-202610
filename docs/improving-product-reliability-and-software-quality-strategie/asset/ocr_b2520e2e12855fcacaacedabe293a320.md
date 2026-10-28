# Table 6.2 Advantages of proactive reliability growth.

| Proactive | Reactive |
|---|---|
| Rapid problem discovery
Failure data can be more accurate because the design team collects it
Equipment is easier to modify in-house
Can react to the first incidence
Manufacturer's reputation maintained | Data more difficult to obtain
Data clouded by less-skilled service personnel
More costly to modify (recalls)
May need several occurrences
Manufacturer's reputation decreased |

# 6.6 Reliability Demonstration Testing

RDT is often confused with reliability growth. In the last section, we showed how reliability growth is used to bring a design to maturity before the first customer shipment. The more mature a design is at product launch, the lower the warranty cost will be. This is achieved by investigating all failures early in the design and keeping track of the company's corrective action to closure.

RDT is a process that statistically ensures that the reliability goal set early in the program is met. There can be confusion regarding the order in which reliability growth and RDT takes place. Reliability growth always precedes RDT. We can start reliability growth measurements once the design is defined. FMEA and HALT identify problems; these surface and then get tracked using reliability growth techniques. RDT cannot start until the design is finalized and there is a final product to be tested. Put another way, reliability growth is a tool that improves the MTBF of the product and the fruits of that effort are verified through RDT.

Earlier in this chapter, we showed that even though a system has a specified MTBF, systems in the field will demonstrate a variety of different MTBFs. The MTBF we specify for the fleet is a measure of the “average MTBF” for all the systems in the fleet. Individual systems in the fleet will experience failures at different times, at different rates and have different MTBF experiences. The fleet MTBF is the accumulated runtime for the entire population in the field, divided by the sum of all the field failures for that same population. An individual single system MTBF varies because of the random nature of failure events. The random nature of these events can be mathematically modeled as a confidence interval. The confidence interval represent the range of expected values that are likely. As the confidence interval increases, i.e. greater confidence that the measured value will be between confidence interval, the range of expected values will also increase. The MTBF calculation assumed an exponential distribution and the confidence interval is based on a chi-squared distribution that is not symmetrical. The upper and lower limits will likely not be symmetrically distributed around the mean MTBF value.

RDT is the tracking of the accumulated product runtimes and the number of failures to verify that the product has achieved its MTBF goal. RDT is a way to show, through product testing, that the product indeed achieves the specified MTBF promised. RDT is a common method used to verify that the design meets a contractual reliability requirement. Reliability engineers are responsible for tracking the accumulated runtime hours