![](bbox=[0.14, 0.106, 0.875, 0.449])

Figure 2.7  Example of a fault tree showing an RTD fault and an RTD-usage model.

three failure modes: power supply, resistive-temperature detector (RTD), or an air-data unit. For an RTD failure, a suitable analytical model is employed to produce prognostic information.

**Example 2.4**  A simple example of an analytical model is the power model used for degradation in capacitors and resistors. When power is applied, failure results because of increasing temperature (Viswanadham and Singh 1998):

$$t_f = A * P^{-n}, \tag{2.2}$$

where $t_f$ is time to fail, $A$ and $n$ are constants, and $P$ is electrical power.

Statistical models, including usage and distribution models, are predictive expressions that relate dependent variables, such as time to fail ($t_f$), and independent variables, such as power ($P$) in Eq. (2.2).

# 2.2.2  Distribution Modeling

When many physical causes and/or complex reactions result in failure, a distribution model is often used instead of a physical model. Engineering experience can relate special distribution types to given failure types. Examples of distribution models and associated applications include those shown in Table 2.2 (Medjaher and Zerhouni 2013; Viswanadham and Singh 1998; Hofmeister et al. 2006, 2013; Hofmeister and Vohnout 2011; Silverman and Hofmeister 2012).