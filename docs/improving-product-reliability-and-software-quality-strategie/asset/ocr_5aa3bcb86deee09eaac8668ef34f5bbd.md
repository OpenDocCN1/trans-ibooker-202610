that describes the difference between the two stresses. In this case, the two stresses are temperature. The accelerated stress temperature is 50 °C and normal temperature the product operates at is 25 °C. When there are many possible failure modes, each will have its own failure mechanism and acceleration factor that may or may not be temperature dependent. So assigning a single acceleration factor to system that is thermally stressed needs to be conservative and only provides an estimate for the accelerated run time. In this example, I’ve chosen a conservative activation energy, E_a, equal to 0.36.

# The Arrhenius Acceleration Model

```
AF = t₁ / t₂ = exp[ (Eₐ / k) (1/T₁ - 1/T₂) ]
(8.1)
```

where

|  |  |  |
|---|---|---|
| A | = | Constant |
| exp | = | Natural logarithm, base 2.7182818 |
| Eₐ | = | Activation energy for the reaction |
| k | = | Boltzman's constant = 8.625 × 10⁻⁵ eV/K |
| T | = | Absolute temperature (K) |
| t | = | Time |

# 8.5 Accelerated Early Life Test (ELT)

An accelerated ELT is intended to identify unintended early life wear out mechanisms. These failures are systemic in nature so can potentially affect the entire field population. The unintended early life wearout mechanism is an escape in the reliability process. It can also be due to a component running hotter than anticipated, causing it to have a significantly shorter useful life. It can be a significant coefficient of thermal expansion (CTE) mismatch between a large component and the circuit board it is mounted to. The CTE mismatch can create significant strain energy as the product heats up causing it to develop solder cracks early in the product life. Once the solder crack is created, additional thermal cycles cause the stress intensity at the crack to be greater due to a stress concentration created at the solder fracture. The result is that the solder crack continues to propagate until there is a full separation of the solder connection. A component that is unintentionally used beyond its absolute maximum recommended limits is likely to suffer premature wear out. This may be due to a misunderstanding between the differences between absolute maximum limits and recommended operating limits. If you’re lucky, these types of design errors get detected during HALT when you step stress the part. However, if it doesn’t get detected in HALT, it is very likely to end up as an early life wearout failure.

The accelerated ELT can be the same test use for ARG testing (ARG) except that it runs for a significantly longer time. The sample size does not have to be large because the failure mechanism we are hoping to detect is due to wear out. Typically, ELT runs for a period of six months using the same test protocol as ARG.