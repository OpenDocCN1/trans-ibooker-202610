# Table 5.3 Standard parameters for calculation of dosage and intake determined for a landfill.

| Parameter | Adults (males) | Child age 6–12 | Child age 2–6 |
|---|---|---|---|
| Average body weight (kg) | 70 | 29 | 16 |
| Skin surface area (cm²) | 18150 | 10470 | 6980 |
| Water ingested (l/day) | 2 | 2 | 1 |
| Air breathed (m³/h) | 0.83 | 0.46 | 0.25 |
| Retention rate (inhaled air) (%) | 100 | 100 | 100 |
| Absorption rate (inhaled air) (%) | 100 | 100 | 100 |
| Soil ingested (mg/day) | 100 | 100 | 200 |
| Bathing duration (minutes) | 30 | 30 | 30 |
| Exposure frequency (days) | 365 | 365 | 365 |
| Exposure duration (years) | 30 | 6 | 4 |

vary greatly depending on the assumed exposure conditions (i.e. the selected exposure scenario). As an example, the air-breathing rate for adult males is 0.83 m³/h in Table 5.3. However, this rate can vary by an order of magnitude from 0.6 m³/h at rest to 7.1 m³/h for vigorous physical exercise.

Examples 5.5 and 5.6 illustrate the calculation of contaminant intake via the inhalation and dermal route, respectively.

# Example 5.5 Calculation of Contaminant Intake

Determine the chronic daily inhalation intake, by adults, of a noncarcinogenic chemical as a function of concentration in fugitive dust at the ABC Landfill (Table 5.3).

# Solution

For an adult exposed to a noncarcinogenic constituent, the intake I_N may be calculated from Eq. (5.2):

```
I_N = (C × CR × EF × ED × RR × ABS) / (BW × AT)
```

From Table 5.3, we take the air-breathing rate for adults as 0.83 m³/h and write

+   CR = 0.83 × 24 = 19.92 m³/day
+   EF = 365 days
+   ED = 30 years for chronic exposure to noncarcinogens

In the absence of better information, a conservative approach would assume the RR and the absorption into bloodstream would both equal 1.0.

+   BW = 70 kg (Table 5.3)
+   AT = 365 days × 30 years

```
I_N = (C × 19.92 × (365 × 30) × 1.0 × 1.0) / (70 (365 × 30))
```

```
I_N = 0.285 m³/kg/day × C
```

C = exposure point concentration (mg/m³)

# Example 5.6 Average Daily Intake from Dermal Contact with Soil

Determine the average daily intake of chlorobenzene over one year of exposure for on-site workers from dermal contact of the soils:

Assume the following additional parameters:

+   A = skin exposed = 20% − 0.2 × 18150 cm² = 3630 cm²
+   DA = dust adherence − 0.51 mg/cm²
+   ABS = skin absorption rate − 6%
+   SM = effect of soil matrix = 15% (i.e. due to soil matrix only 15% of contamination is actually available for contact)
+   EF = two exposure events per day; 156 exposure days per year
+   ED = 1 year
+   BW = 70 kg
+   AT = 365 days

# Solution

```
I_N = [ C (mg/kg) × A (cm²) × DA / exp.event mg/cm² × ABS × SM × 2 exp.events/day × 156 days/year × ED × kg·soil / 10⁶ mg·soil ] / (BW × AT)
```

```
I_N = (C × 3630 × 0.51 × 0.06 × 0.15 × 2 × 156 × 10⁻⁶) / (70 × 365)
```

```
I_N = C × 2.03 × 10⁻⁷ /day
```

The given average concentration of chlorobenzene in soil is 1.39 mg/kg. The daily intake of chlorobenzene is

```
1.39 mg/kg × 2.03 × 10⁻⁷ /day
```

Thus, I_N = 2.82 × 10⁻⁷ (mg/kg·day)

Again, the foregoing examples represent only two approaches to calculating contaminant intake in the course of an exposure assessment. The next factor to be analyzed is toxicity.

# 5.1.4 Toxicity Assessment

Broadly speaking, toxicity is the degree to which a substance is poisonous. Most chemicals, toxic or otherwise, enter the body through eyes, respiratory tract, digestive tract, and skin. Two levels of toxicity are defined: acute “short-term” exposure that initiates poisoning and chronic