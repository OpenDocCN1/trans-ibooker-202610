shown. Since these measurements are noisy and the component values are imprecise (±Δ), we require an improved estimate of the output voltage. We develop an LKF to solve this problem from first principles—a typical approach. Writing the Kirchhoff current equations at the node, we have

$$I_{in}(t)-\frac{e(t)}{R}-C\frac{de(t)}{dt}=0$$

where $e_o$ is the initial voltage and $R$ is the resistance with $C$ the capacitance. The measurement equation for a voltmeter of gain $K_e$ is simply

$$e_{out}(t)=K_e e(t)$$

We choose to use the discrete formulation; therefore, approximating the derivatives with first differences and substituting into, we have

$$C\frac{e(t)-e(t-1)}{\Delta T}=-\frac{e(t-1)}{R}+I_{in}(t-1)$$

or

$$e(t)=\left(1-\frac{\Delta T}{RC}\right)e(t-1)+\frac{\Delta T}{C}I_{in}(t-1)$$

where the measurement is given above. Suppose that for this circuit the parameters are as follows: $R=3.3\ \mathrm{k}\Omega$ and $C=1000\ \mu\mathrm{F}$, $\Delta T=100\ \mathrm{ms}$, $e_o=2.5\ \mathrm{V}$, $K_e=2.0$, and the voltmeter is precise to within ±4 V. Then transforming the physical circuit model into state-space form by defining $x=e$, $y=e_{out}$, and $u=I_{in}$, we obtain

$$x(t)=0.97x(t-1)+100u(t-1)+w(t-1)$$

$$y(t)=2x(t)+v(t)$$

The process noise covariance is used to model the circuit parameter uncertainty with $R_{ww}=0.0001$, since we assume standard deviations, $\Delta R,\ \Delta C$ of 1%. Also, $R_{vv}=4$, since two standard deviations are $\Delta V=2\left(\frac{1}{2}4\ \mathrm{V}\right)$. We also assume initially that the state is $x(0)\sim\mathcal{N}(2.5,10^{-12})$ and that the input current is a step function of $u(t)=300\ \mu\mathrm{A}$.

With the data using MATLAB (see Figure 1.6), we now consider the design of the Kalman filter. In the ideal problem, we are given the model set

$$\Sigma:=\{A,B,C,D,R_{ww},R_{vv},x(0),P(0)\}$$

along with the known input $\{u(t)\}$ and the set of noisy measurements, $\{y(t)\}$, to construct the processor. The LKF for the RC-circuit problem is

$$\hat{x}(t\mid t-1)=0.97\hat{x}(t-1\mid t-1)+100u(t-1)\quad\text{(Predicted State)}$$

$$\tilde{P}(t\mid t-1)=0.94\tilde{P}(t-1\mid t-1)+0.0001\quad\text{(Predicted Covariance)}$$

$$e(t)=y(t)-2\hat{x}(t\mid t-1)\quad\text{(Innovation)}$$

$$R_{ee}(t)=4\tilde{P}(t\mid t-1)+4\quad\text{(Innovations Covariance)}$$