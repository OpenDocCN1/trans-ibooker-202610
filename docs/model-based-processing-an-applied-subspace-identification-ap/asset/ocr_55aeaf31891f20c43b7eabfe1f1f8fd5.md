voltage, and the Kalman filter predicted measurement voltage. As illustrated, the filter performs reasonably compared to the noisy data and tracks the true signal quite well. We derive the Kalman filter algorithm from the innovations viewpoint to follow, since it will provide the foundation for the nonlinear algorithms to follow.

# 4.2.1 Innovations Approach

In this section, we briefly develop the (linear) Kalman filter algorithm from the innovations perspective following the approach by Kailath [4–6]. First, recall from Chapter 2 that a model of a stochastic process can be characterized by the Gauss–Markov model using the state-space representation

x(t) = A(t − 1)x(t − 1) + B(t − 1)u(t − 1) + w(t − 1) (4.1)

where w is assumed zero-mean and white with covariance Rww and x and w are uncorrelated. The measurement model is given by

y(t) = C(t)x(t) + D(t)u(t) + v(t) (4.2)

where v is a zero-mean, white sequence with covariance Rvv and v is (assumed) uncorrelated with x and w.

The linear state estimation problem can be stated in terms of the preceding state-space model as GIVEN a set of noisy measurements {y(i)}, for i = 1, …, t characterized by the measurement model of Eq. (4.2), FIND the linear minimum (error) variance estimate of the state characterized by the state-space model of Eq. (4.1). That is, find the best estimate of x(t) given the measurement data up to time t, Y(t) := {y(1), …, y(t)}.

First, we investigate the batch minimum variance estimator for this problem, and then we develop an alternative solution using the innovations sequence. The recursive solution follows almost immediately from the innovations. Next, we outline the resulting equations for the predicted state, gain, and innovations. The corrected, or filtered, state equation then follows. Details of the derivation can be found in [2].

Constraining the estimator to be linear, we see that for a batch of N-data, the minimum variance estimator is given by 1

X̂_MV = K_MV Y = R_xy R_yy⁻¹ Y (4.3)

1 Note that the complete form of the linear minimum variance estimator (see Appendix A) for nonzero mean is given by [2]

X̂_MV = m_x + R_xy R_yy⁻¹ (y − m_y)