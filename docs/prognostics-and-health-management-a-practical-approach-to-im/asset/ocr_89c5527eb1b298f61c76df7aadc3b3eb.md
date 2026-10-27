**Table 2.2** Failure distributions and example applications.

| Distribution | Example applications |
|---|---|
| Exponential | Fatigue, wear caused by constant stress: resistors |
| Gamma | Failures caused by shock and vibration: boards, package connections |
| Lognormal | Failures caused by failure of insulation resistance, crack growth, and rate-dependent processes: encapsulation failures |
| Gumbel | Failures caused by corrosion, shear breaks (strength), dielectric breakdown: conductor connections, interconnects |
| Weibull | Life (use) and breakdown failures: capacitors, cables |

The following distributions form characteristic curves that are useful for modeling complex failure mechanisms (a more complete list can be found in Chapter 1):

*Exponential:* \(f(x,\theta)=\frac{1}{\theta}e^{-x/\theta}\)

*Gamma:* \(f(x,\theta,k)=\frac{1}{\Gamma(\theta)\theta^k}x^{k-1}e^{-x/\theta}\)

*Lognormal:* \(f(x,\mu,m,\sigma)=[e^{-(\ln(x-\mu)-m)^2/(2\sigma^2)}]/[(x-\mu)\sigma\sqrt{2\pi}]\)

*Gumbel:* \(f(x,\sigma,\mu)=\frac{1}{\sigma}e^{-(x-\mu)/\sigma}\exp[-e^{-(x-\mu)/\sigma}]\)

*Weibull:* \(f(x,\beta,\eta,\gamma)=\frac{\beta}{\eta}\left(\frac{x-\gamma}{\eta}\right)^{\beta-1}e^{-((x-\gamma)/\eta)^\beta}\)

In engineering applications, the Weibull, an extreme-value type of distribution, and the lognormal distribution are frequently used for modeling because they can be fitted to data from a large number of applications: especially lifetime distributions where failures are bounded below zero (Xu et al. 2015). The versatility of the Weibull distribution is evidenced by the following example life-time applications and by the example Weibull plots in Figure 2.8:

+   - Maximum flood levels of rivers; maximum wind gusts; steam boiler pressure
+   - Fatigue life of bearings; blending time of powder coating materials
+   - Tensile strength of wires; yield strength of beams
+   - Lifetimes of passive electronic components, such as resistors and capacitors
+   - Time-dependent dielectric breakdown (TDDB) phenomena; count of detectable high-resistance events in solder joints

# 2.2.3  Physics of Failure (PoF) and Reliability Modeling

PoF used in reliability modeling and simulation is significantly different from the constant-failure rate (CFR) modeling (based on exponential distribution) used as the basis for the Military Handbook 217 series (MIL-HDBK-217C). The PoF approach has dominated since the 1980s: root causes of failure, such as fatigue, fracture, wear, and corrosion, are studied and corrected to achieve lifetime design requirements by