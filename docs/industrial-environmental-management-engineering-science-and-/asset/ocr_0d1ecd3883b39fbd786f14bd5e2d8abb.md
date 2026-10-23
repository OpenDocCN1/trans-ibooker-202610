# Figure 4.15 A process train to measure PM emissions. Source: Adopted from USEPA (1991b).

# Example 4.14 Check for Emission Standards Compliance

As a consulting engineer, you have been contracted to modify an existing control device used in fly ash removal. The standards for emissions have been changed to a total numbers basis. Determine if the unit will meet an emission standard of 10^5.7 particle/acf. Data for the unit are given below:

+   - Average particle size, d_p = 10 µm, assume constant
+   - Particle specific gravity = 2.3
+   - Inlet loading, IL = 3.0 gr/ft³
+   - Efficiency (mass basis), E = 99.5% = 0.995

# Solution

The outlet loading (OL) is given by

```
OL = (1.0 − E)(IL)
    = (1.0 − 0.995)3.0 gr/ft³
    = 0.015 gr/ft³
```

Assume a basis of 1.0 ft³

Particle mass − M = ρ_p V_p = ρ_p (π d_p²)/6

```
= (7000 gr/lb) { [π (10 µm) (0.328×10⁻⁵ ft/µm)³] (2.3)62.4 lb/ft³ / 6 }
```

= 1.881×10⁻⁸ gr/particle

Number of particles = 0.015 gr/ft³ / 1881×10⁻⁸ gr/particle = 8.0×10⁵ particle/ft³

Allowable number of particles/ft³ = 10^5.7 = 5.01×10⁵

```
particle/ft³ < 8.0×10⁵ particle/ft³
```

Therefore, the unit will not meet the particle emission standard.