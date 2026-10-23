
### Table 5.3 Standard parameters for calculation of dosage and intake determined for a landfill.

| Parameter | Adults (males) | Child age 6–12 | Child age 2–6 |
|---|---|---|---|
| Average body weight (kg) | 70 | 29 | 16 |
| Skin surface area (cm²) | 18150 | 10470 | 6980 |
| Water ingested (L/day) | 2 | 2 | 1 |
| Air breathed (m³/h) | 0.83 | 0.46 | 0.25 |
| Retention rate (inhaled air) (%) | 100 | 100 | 100 |
| Absorption rate (inhaled air) (%) | 100 | 100 | 100 |
| Soil ingested (mg/day) | 100 | 100 | 200 |
| Bathing duration (minutes) | 30 | 30 | 30 |
| Exposure frequency (days) | 365 | 365 | 365 |
| Exposure duration (years) | 30 | 6 | 4 |

The parameters vary greatly depending on the assumed exposure conditions (i.e. the selected exposure scenario). As an example, the air-breathing rate for adult males is 0.83 m³/h in Table 5.3. However, this rate can vary by an order of magnitude from 0.6 m³/h at rest to 7.1 m³/h for vigorous physical exercise.

Examples 5.5 and 5.6 illustrate the calculation of contaminant intake via the inhalation and dermal route, respectively.

### Example 5.5 Calculation of Contaminant Intake

Determine the chronic daily inhalation intake, by adults, of a noncarcinogenic chemical as a function of concentration in fugitive dust at the ABC Landfill (Table 5.3).

### Solution

For an adult exposed to a noncarcinogenic constituent, the intake I_N may be calculated from Eq. (5.2):

```
I_N = (C × CR × EF × ED × RR × ABS) / (BW × AT)
```

From Table 5.3, we take the air-breathing rate for adults as 0.83 m³/h and write

- CR = 0.83 × 24 = 19.92 m³/day
- EF = 365 days
- ED = 30 years for chronic exposure to noncarcinogens

In the absence of better information, a conservative approach would assume the RR and the absorption into bloodstream would both equal 1.0.

- BW = 70 kg (Table 5.3)
- AT = 365 days × 30 years

```
I_N = (C × 19.92 × (365 × 30) × 1.0 × 1.0) / (70 × (365 × 30))
```

```
I_N = 0.285 m³/kg/day × C
```

C = exposure point concentration (mg/m³)

### Example 5.6 Average Daily Intake from Dermal Contact with Soil

Determine the average daily intake of chlorobenzene over one year of exposure for on-site workers from dermal contact of the soils:

Assume the following additional parameters:

- A = skin exposed = 20% × 18150 cm² = 0.2 × 18150 cm² = 3630 cm²
- DA = dust adherence = 0.51 mg/cm²
- ABS = skin absorption rate = 6%
- SM = effect of soil matrix = 15% (i.e. due to soil matrix only 15% of contamination is actually available for contact)
- EF = two exposure events per day; 156 exposure days per year
- ED = 1 year
- BW = 70 kg
- AT = 365 days

### Solution

```
I_N = [C (mg/kg) × A (cm²) × DA (mg/cm²) × ABS × SM × 2 exposure events/day × 156 days/year × ED × 10⁻⁶ kg·soil/mg·soil] / (BW × AT)
```

```
I_N = (C × 3630 × 0.51 × 0.06 × 0.15 × 2 × 156 × 10⁻⁶) / (70 × 365)
```

```
I_N = C × 2.03 × 10⁻⁷ /day
```

The given average concentration of chlorobenzene in soil is 1.39 mg/kg. The daily intake of chlorobenzene is

1.39 mg/kg × 2.03 × 10⁻⁷ /day

Thus, I_N = 2.82 × 10⁻⁷ (mg/kg·day)

Again, the foregoing examples represent only two approaches to calculating contaminant intake in the course of an exposure assessment. The next factor to be analyzed is toxicity.

## 5.1.4 Toxicity Assessment

Broadly speaking, toxicity is the degree to which a substance is poisonous. Most chemicals, toxic or otherwise, enter the body through eyes, respiratory tract, digestive tract, and skin. Two levels of toxicity are defined: acute “short-term” exposure that initiates poisoning and chronic “long-period” exposure that causes anemia, leukemia, and death.

In the context of the third stage of the risk assessment process, toxicity is defined by the dose–response relationship for each surrogate chemical. The output takes the form of mathematical constants for insertion into risk calculation equations. In addition to providing a set of mathematical constants for calculating risk, the toxicological assessment should also analyze the uncertainty inherent in these numbers and describe how this uncertainty may affect the estimates of risk.

In a prudent approach to public health protection, the EPA has built several safety factors into its methods for establishing reference doses and carcinogenic slope factors (USEPA 2002a, 2002b, 2005). Virtually all errors of uncertainty are made in the direction of public health protection to insure that risks are overestimated rather than underestimated. Four examples of this protective approach follow:

- For noncarcinogens, extrapolation of animal reference doses to humans utilizes two safety factors of ten, one for animal-to-human extrapolation, a second for variation in toxic sensitivities within the human population.
- For carcinogens, the linearized multistage model assumes the upper-bound 95% confidence level of extrapolated data (i.e. it is likely to be conservative 19 out of 20 times).
- For carcinogens, the linearized multistage model extrapolates data from the 10–90% carcinogenesis range observed in experimental animals to the regulatory target of 0.0001% carcinogenesis, a step which could overstate risk by several orders of magnitude.
- Although evidence indicates that, like noncarcinogens, nongenotoxic carcinogens have thresholds below which they fail to influence cellular differentiation or division, they are treated mathematically like genotoxic carcinogens according to the linearized multistage dose–response model.

## 5.1.5 Risk Characterization

The final stage of a four-stage human health risk assessment is to estimate risks. Cancer is not the only undesired health consequence of pollution: exposure to ionizing radiation and coal dust, for example, can lead to radiation sickness and birth defects, as well as cancer. To illustrate risk characterization, however, we shall focus on the calculations that yield quantitative estimates of both the carcinogenic and noncarcinogenic risks to receptors for all exposure scenarios considered. Estimates are typically calculated for all three exposure routes and for the maximally exposed individual as well as the most probable exposed population. Such calculations are straightforward and yield quantitative estimates of risk. The challenge lies not in making the calculations but in interpreting the results such that they are applied properly in decision-making. The overall effort is referred to as risk characterization. Risks from toxic chemicals, depending on the context, may be defined, described, and calculated in different ways.

Risk is normally defined as the probability for an individual to suffer an adverse effect from an event. What is the probability that certain types of cancer will develop in people exposed to aflatoxin in peanut products or benzene from gasoline? What is the likelihood that workers exposed to lead will develop nervous system disorders? In the context of this text, a chemical release is an example of an event. As with any relationship expressing or using probability, there is no defined way of expressing (mathematically or with scientific rigor) a single deterministic value of a phenomenon that is probabilistic. A conceptual way of expressing chemical risk is to write it as a function of hazard and exposure.

Hazard is the potential for a substance or situation to cause harm or create adverse impacts on persons or the environment. The magnitude of the hazard reflects the potential adverse consequences, including mortality, shortened life-span, impairment of bodily function, sensitization to chemicals in the environment, or diminished ability to reproduce. Exposure denotes the magnitude and the length of time the organism is in contact with an environmental contaminant, including chemical, radiation, or biological contaminants.

When risk is in terms of probability, it is expressed as a fraction, without units. It has values from 0.0 (absolute certainty that there is no risk) to 1.0 (absolute certainty that an adverse outcome will occur).

For chemicals the term hazard is typically associated with the toxic properties of a chemical specific to the type of exposure. Similar chemicals would have similar innate hazards. However, one must examine the exposure to that hazard to determine the risk. For example, let us say you have three pumps that are all transporting the same chemical (same hazard), but one pump has a seal leak. Which pump poses the greatest risk to the worker? The pump with the seal leak has the greatest potential for exposure, while the hazards are equal (same chemical), so the seal leak pump poses the greatest risk. To expand, let us say we have three pumps that are transporting different chemicals; which one poses the greatest risk to the worker? In this case, the engineer would need to examine the hazard, or innate inherent toxicity, of each of the chemicals, as well as the operation of the pumps to determine which poses the greatest risk.
