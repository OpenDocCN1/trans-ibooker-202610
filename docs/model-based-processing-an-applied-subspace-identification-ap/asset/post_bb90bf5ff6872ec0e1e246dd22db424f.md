
## 5 Parametrically Adaptive Processors

### 5.1 Introduction

The model-based approach to the parameter estimation/system identification problem [1–4] is based on the decomposition of the joint posterior distributions that incorporate *both* dynamic state and parameter variables. From this formulation, the following problems evolve: joint state/parameter estimation; state estimation; and parameter (fixed and/or dynamic) estimation. The state estimation problem was discussed in the previous chapters. However, the most common problem found in the current literature is the parameter estimation problem that can be solved “off-line” using batch approaches (maximum entropy, maximum likelihood, minimum variance, least squares, etc.) or “on-line” using the recursive identification approach, the stochastic Monte Carlo approach and for that matter almost any (deterministic) optimization technique [5, 6]. These on-line approaches follow the classical (EKF), modern (UKF), and the sequential Monte Carlo particle filter (PF). However, it still appears that there is *no* universally accepted technique to solve this problem especially for fixed parameters [7–9].

From the pragmatic perspective, the most useful problem is the *joint* state/parameter estimation problem, since it evolves quite naturally from the fact that a model is developed to solve the basic state estimation problem and it is found that its inherent parameters are poorly specified, just bounded, or even unknown, inhibiting the performance of the processor. We call this problem the “joint” state/parameter estimation, since *both* states and parameters are estimated simultaneously (on-line) and the resulting processor is termed *parametrically adaptive* [10]. This terminology evolves because the inherent model parameters are adjusted sequentially as the measurement data becomes available.

In this chapter, we concentrate primarily on the joint state/parameter estimation problem and refer the interested reader to the wealth of literature available on this subject [7–19]. First, we precisely define the basic problem from the Bayesian perspective and then investigate the classical, modern, and particle approaches to its solution. We incorporate the nonlinear trajectory estimation problem of Jazwinski [20] used throughout as an example of a parametrically adaptive design. Next we briefly develop the prediction error approach constrained to a linear model (RC-circuit) and then discuss a case study to demonstrate the approach.

### 5.2 Parametrically Adaptive Processors: Bayesian Approach

To be more precise, we start by defining the joint state/parameter estimation problem. We begin by formulating the Bayesian recursions in terms of the posterior distribution using Bayes’ rule for decomposition, that is,

Pr(x(t), θ(t)|Y_t) = Pr(x(t)|θ(t), Y_t) × Pr(θ(t)|Y_t)  
= Pr(θ(t)|x(t), Y_t) × Pr(x(t)|Y_t) (5.1)

From this relation, we begin to “see” just how the variety of state and parameter estimation related problems evolve, that is,

- Optimize the state posterior: Pr(x(t)|Y_t) (State Estimation)
- Optimize the parametric posterior: Pr(θ(t)|Y_t) (Parameter Estimation)
- Optimize the joint state/parameter posterior: Pr(x(t), θ(t)|Y_t) (State/Parameter Estimation)

Now if we proceed with the usual factorizations, we obtain the Bayesian decomposition for the state estimation problem as

Pr(x(t)|Y_t) = [Pr(y(t)|x(t)) / Pr(y(t)|Y_{t-1})] × Pr(x(t)|Y_{t-1}) (5.2)

Equivalently for the parameter estimation problem, we have

Pr(θ(t)|Y_t) = [Pr(y(t)|θ(t)) / Pr(y(t)|Y_{t-1})] × Pr(θ(t)|Y_{t-1}) (5.3)

Now for the joint state/parameter estimation problem of Eq. (5.1), we can substitute the above equations to obtain the posterior decomposition of interest, that is,

Pr(x(t), θ(t)|Y_t) = [Pr(y(t)|θ(t)) / Pr(y(t)|Y_{t-1})] × [Pr(x(t)|θ(t), Y_t) × Pr(θ(t)|Y_{t-1})] (5.4)

Noisy measurement data. Usually, the model structure is predefined and then a parameter estimator is developed to “fit” parameters according to some error criterion. After completion or during this estimation, the quality of the estimates must be evaluated to decide if the processor performance is satisfactory or equivalently the model adequately represents the data. There are various types (criteria) of identifiers employing many different model (usually linear) structures [2–4]. Here we are primarily concerned with joint estimation in which the models and parameters are nonlinear and discuss the linear case in Section 5.5. Thus, we will concentrate on developing parameter estimators capable of on-line operations with nonlinear dynamics.

The solution to this joint estimation problem involves the augmentation of a parametric model (Θ) along with the dynamic state (x) that embeds the parameters directly. Here the simple solution is to apply the same algorithm (LZKF, EKF, IEKF, UKF) with the original state vector replaced by the new “augmented” state X → [x | Θ]′. The identical algorithm is then applied directly to the data. This is true for the classical and modern techniques, but not true for the MC-based particle filters, since they must now draw particles from their priors and conditional parametric distributions. However, what happens if we do not have a direct physics-based model for the parameters? The nonlinear processors require some characterization or parametric model to be embedded in the joint processor; therefore, we must have some way of providing a parametric representation, if a “physics-based” model is not available.

#### 5.3.1 Parametric Models

Perhaps the most popular approach to this quandary is to use the so-called “random walk” model that has evolved from the control/estimation area. This model essentially assumes a constant parameter (vector) θ with slight variations. In continuous time, a constant can be modeled by the differential equation

dθ_t / dt = 0   for   θ_0 = constant (5.5)

Here the solution is just the θ_0 ∀t. Note that this differential equation is a state-space formulation in θ enabling us to incorporate it directly into the overall dynamic model. Now if we artificially assign a Gaussian process noise term w_t to excite the model, then a Wiener process or random walk (pseudo-dynamic) model evolves. Providing a small variance to the noise source approximates small variations in the parameter, enabling it to “walk” in the underlying parameter space and converge to the true value. Since we typically concentrate on a sampled-data system, we can discretize this representation using first differences (with Δt = 1) and incorporate its corresponding mean/covariance propagation into our Gauss–Markov model, that is,

\theta(t) = \theta(t-1) + \mathbf{w}_{\theta}(t-1) \quad \text{for} \quad \mathbf{w}_{\theta} \sim \mathcal{N}(0, \mathbf{R}_{w_{\theta}w_{\theta}})  
\mathbf{m}_{\theta}(t) = \mathbf{m}_{\theta}(t-1)  
\mathbf{P}_{\theta\theta}(t) = \mathbf{P}_{\theta\theta}(t-1) + \mathbf{R}_{w_{\theta}w_{\theta}}(t-1) \tag{5.6}

The process noise covariance \mathbf{R}_{w_{\theta}w_{\theta}} controls the excursions of the random walk or equivalently the parameter space.

We are primarily interested in physical systems that have parametric uncertainties that are well modeled by the random walk or other statistical variations. Of course, if the parametric relations are truly dynamic, then the joint approach incorporates these dynamics directly into the augmented state-space model and yields an optimal filtering solution to this joint problem. Using these “artificial dynamics” is the approach employed in the classical LZKF, EKF, IEKF, modern UKF and other nonlinear state-space techniques as well as the purely Bayesian processors (PF).

A popular variation of the random walk is based on incorporating the “forgetting factor” that has evolved from the system identification literature [1] and is called so because it controls the time constant of an inherent exponential window over the data. To see this, consider a squared-error performance function defined by

\mathcal{J} = \sum_{t=1}^{k} \alpha(t,k) e^{2}(t) \quad \text{for} \quad \alpha(t,k) = \lambda^{k-t}, \quad 0 < \lambda < 1 \tag{5.7}

where the window or weight \alpha is the forgetting factor that creates an exponentially decaying window of length, say \(T_{\lambda}\), in which the most recent data are weighted more than the past data. This follows from the following approximation [1, 2]:

\alpha(t,k) = e^{(t-k)\ln \lambda} \approx e^{-(t-k)(1-\lambda)} \quad \text{for} \quad T_{\lambda} = \frac{1}{1-\lambda} \tag{5.8}

where \(T_{\lambda}\) is called the memory time constant that remains constant (approximately) over \(T_{\lambda}\) samples with typical choices between \(0.98 < \lambda < 0.995\) in practice.

The forgetting factor has been introduced into a variety of sequential algorithms in different roles. For instance, a direct implementation into the random walk model of Eq. (5.6) is [23]

\theta(t) = (1 - \lambda(t))\,\theta(t-1) + \lambda(t)\mathbf{w}_{\theta}(t-1) \tag{5.9}

Here \(\lambda\) affects the tracking speed of the updates providing a trade-off between speed and noise attenuation. Setting \(\lambda = 1\) implies \(T_{\lambda} \to \infty\) an “infinite memory” while \(\lambda = 0\) provides instantaneous response forgetting the entire past.

Thus, the forgetting factor introduces a certain amount of flexibility into the joint state/parameter estimation processors by enabling them to be more
