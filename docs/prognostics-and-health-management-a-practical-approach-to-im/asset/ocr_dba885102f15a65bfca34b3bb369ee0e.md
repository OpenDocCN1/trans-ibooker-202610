![](bbox=[0.12, 0.13, 0.86, 0.38])

Figure 7.6 Same MTTF for different failure distributions and signatures.

# 7.2.4 TTF and PITTFF

We need to know how to determine, calculate, and/or estimate a TTF value that begins when degradation begins and ends when functional failure occurs (see TTF 1 and TTF 2 in Figure 7.5). But at the time when degradation is first detected, there is no a priori knowledge of that future time of failure; yet our prediction program needs to converge from an initial estimate of that time to a very accurate estimate of the time of failure. Research into reliability metrics such as MTTF, MTBF, and FIT indicates that they are not really close in value to what we need for TTF.

The prediction program we are using, ARULEAV, provides a parameter called PITTFF0 (introduced in Chapter 6) as a means to specify an initial value for TTF. TTF is not a value that is looked at or specified by manufacturers and/or vendors of products; failures in the field are usually due to either an anomalous event such as a lightning strike or degradation. Degradation typically is not caused by a part entering what is referred to as the wear-out region of a bathtub curve; rather, degradation is typically due to an accumulation of fatigue damage caused by cyclic stresses and strains (such as thermal and mechanical) during operation (Hofmeister et al. 2006).

We can estimate an initial value for TTF using a number of methods: a service-life determination, an end-use test method, or an MTTF-based method. But be aware that the supplier of the prediction program advises that, in general, that program converges to within 25% accuracy in less time when the initial estimate is higher, rather than lower, in comparison to the true time of functional failure.

# TTF: Service Life Determination

Instead of using MTTF or MTBF values for an extremely low failure rate, you might use end-use values based on service-life values from vendors. For example:

+   - A tire is warranted to last 5 years or 40 000 miles, and on or about that amount of usage, the tire is deemed “worn out” and is replaced: the tire is not repairable. The