$t_i \in R (i = 1, 2)$, we have

$$\text{Cov}[Y(\boldsymbol{s}_1, t_1), Y(\boldsymbol{s}_2, t_2)] = C^{(s)}(\boldsymbol{s}_1, \boldsymbol{s}_2) \times C^{(t)}(t_1, t_2),$$ (9.66)

where $C^{(s)}$ and $C^{(t)}$ are spatial and temporal covariance functions, respectively. For stationary functions, Eq. (9.66) becomes

$$C_y(\boldsymbol{h}, \tau) = C^{(s)}(\boldsymbol{h}) \times C^{(t)}(\tau), \quad \boldsymbol{h} \in R^d, \tau \in R.$$ (9.67)

The separability also implies that the S-T correlation function can be written as

$$\rho(\boldsymbol{h}, \tau) = \rho^{(s)}(\boldsymbol{h}, \mathbf{0}) \times \rho^{(t)}(\mathbf{0}, \tau), \quad \boldsymbol{h} \in R^d, \tau \in R.$$ (9.68)

In practice, separability says that the spatial and temporal effects of the process $Y(\boldsymbol{s}, t)$ are orthogonal to each other, and they can be parameterized by separate functions. Conversely, if the S-T correlation is the product of a spatial correlation function and a temporal correlation function, then the process is separable. Thus, Eq. (9.68) is a characterization of separability in second-order stationary processes. Therefore, the equation provides a way to check the separability assumption by comparing $C_y(\boldsymbol{h}, \tau)$ with $C_y(\boldsymbol{h}, \mathbf{0}) \times C_y(\mathbf{0}, \tau)$. 

Figure 9.14 shows a contour plot of a nonseparable S-T correlation given by Eq. (6.3) of Cressie and Wikle (2011). From the plot, it is seen that the correlations are nonlinear functions of spatial distance $h$ and temporal lag $\tau$. In the literature, many models have been proposed for the S-T processes. These models have various features in their S-T covariance functions. See, for instance, the regional-effect model of Reinsel et al. (1981) and the mixture model of Oehlert (1993), among others.

# 9.6.2 Some Nonseparable Covariance Functions

In what follows, we mention two specific families of S-T covariance functions that are nonseparable and flexible. The first family is proposed by Cressie and Huang (1999)

![](bbox=[0.28, 0.61, 0.75, 0.82])

Figure 9.14 Contour plot of a nonseparable S-T correlation function given in eq. (6.3) of Cressie and Wikle (2011). The parameters are $\alpha = 1$ and $\beta = 20$. Source: Cressie and Wikle (2011). ©2011 John Wiley & Sons.