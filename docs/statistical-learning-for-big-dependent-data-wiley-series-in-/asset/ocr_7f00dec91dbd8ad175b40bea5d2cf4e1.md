where $F = B^{-1}$ is the forward-shift operator, $y_t$ is the underlying market share without the promotion and $w_i$ are parameters used to quantify the effect of sale promotion. In this particular case, one would expect $w_{-1} < 0$, $w_0 > 0$ and $w_1 < 0$, and the permanent effect of the promotion is given by $w_{-1} + w_0 + w_1$.

Turn to the case of vector time series. The effect of an unusual event that begins at $t = h$ can be modeled as

$$z_t = w_0 S_t^{(h)} + \Psi(B) a_t,$$

where, as before, $w_0 = (w_{0,1}, \dots, w_{0,k})'$ with $k$ being the dimension of $z_t$. Here, it is easy to see that we have $z_t = y_t$ for $t < h$ and $z_t = w_0 + y_t$ for $t \ge h$, where $y_t$ is the uncontaminated vector time series. Obviously, we can generalize the model to include cases with multiple interventions, each with complicated impacts on the underlying time series.

Since

$$I_t^{(h)} = S_t^{(h)} - S_{t-1}^h = \nabla S_t^{(h)},$$ (4.9)

the choice of using an indicator variable or a step function for a given intervention may not be critical in real applications. One can let the data choose a proper variable to use. For instance, if we estimate the model

$$z_t = (w_0 - w_1 B) S_t^{(h)} + \Psi(B) a_t,$$ and find that $\hat{w}_0 \simeq \hat{w}_1$, then we can simplify the model as

$$z_t = \hat{w}_0 \nabla S_t^{(h)} + \Psi(B) a = \hat{w}_0 I_t^{(h)} + \Psi(B) a_t,$$

indicating that the intervention effect only occurs at $t = h$. If the choice between using indicator variables and step functions is in doubt, one can always use model selection methods such as information criteria to select a proper model.

# 4.1.3 Intervention with General Exogenous Variables

Intervention analysis is not limited to using either indicator variables or step functions. In fact, one can use any exogenous variable for intervention analysis so long as the variable is known at time $t$. For instance, let $x_t$ be the monthly expenditure of advertisement of a company on its products and $z_t$ be the total monthly sales of the company. Then, the effect of advertisement on sales can be modeled as

$$z_t = v(B)x_t + y_t,$$

where $y_t$ is the monthly sales without any advertisement and $v(B) = v_0 + v_1 B + v_2 B^2 + \cdots$ is the transfer function. In practice, $v(B)$ can often be well approximated by ratio of two polynomials such as $v(B) = w(B)B^b/\delta(B)$, where $w(B) = w_0 + w_1 B + \cdots + w_m B^m$ and $\delta(B) = 1 - \delta_1 B - \cdots - \delta_r B^r$ with $m$ and $r$ are nonnegative integers, and $b$ is an integer commonly referred to as the *delay* of the intervention. This belongs to the transfer function models discussed in Chapter 3.

# 4.1.4 Building an Intervention Model

Again, we start with the scalar case with a single intervention at time $t = h$ and consider the model

$$z_t = v(B) I_t^{(h)} + \psi(B) a_t.$$ (4.10)