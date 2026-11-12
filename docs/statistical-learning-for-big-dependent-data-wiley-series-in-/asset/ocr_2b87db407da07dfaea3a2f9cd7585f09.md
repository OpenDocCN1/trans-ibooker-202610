time series. The flexibility of \( w(B)/\delta(B) \) can be found in Box and Tiao (1975) and Chapter 3. Clearly, we require that \( w(B) \) and \( \delta(B) \) have no common factors in Eq. (4.6).

In real applications, a time series may be subject to the impacts of multiple unusual events at time \( t = h_i \), for \( i = 1, \dots, g \), with \( g \) being a nonnegative integer. In this case, we postulate the following model for \( z_t \):

```
\( z_t = \sum_{i=1}^g \frac{w_i(B)}{\delta_i(B)} I_t^{(h_i)} + \psi(B)a_t, \tag{4.7} \)
```

where \( w_i(B) = w_{i,0} + w_{i,1}B + \cdots + w_{i,m_i}B^{m_i} \) and \( \delta_i(B) = 1 - \delta_{i,1}B - \cdots - \delta_{i,r_i}B^{r_i} \) with \( m_i \) and \( r_i \) are nonnegative integers.

The prior discussion of modeling impacts of unusual events on a scalar time series can easily be generalized to the vector case. For instance, for an observed vector series the model in Eq. (4.3) would become,

```
\( z_t = \boldsymbol{w}_0 I_t^{(h)} + \boldsymbol{y}_t = \boldsymbol{w}_0 I_t^{(h)} + \boldsymbol{\psi}(\boldsymbol{B})\boldsymbol{a}_t, \)
```

where \( \boldsymbol{w}_0 = (w_{01}, \dots, w_{0k})' \) is the vector of effects of the event with \( w_{0i} \) being the impact on the \( i \)th component \( z_{it} \), \( \boldsymbol{y}_t \) is the corresponding uncontaminated vector time series, and \( \boldsymbol{\psi}(\boldsymbol{B}) = [\boldsymbol{\Phi}(\boldsymbol{B})]^{-1}\theta(\boldsymbol{B}) \) denotes the \( \psi \)-weight matrices of \( \boldsymbol{y}_t \), which follows the VARMA model \( \boldsymbol{\Phi}(\boldsymbol{B})\boldsymbol{y}_t = \theta(\boldsymbol{B})\boldsymbol{a}_t \). See Chapter 3 for further discussion on the \( \psi \)-weight matrices. Similarly, the model in Eq. (4.6) can be generalized as

```
\( \boldsymbol{z}_t = [\boldsymbol{\delta}(\boldsymbol{B})]^{-1} \boldsymbol{w}(\boldsymbol{B}) I_t^{(h)} + \boldsymbol{y}_t, \tag{4.8} \)
```

where \( \boldsymbol{w}(\boldsymbol{B}) = \boldsymbol{w}_0 + \boldsymbol{w}_1 \boldsymbol{B} + \cdots + \boldsymbol{w}_m \boldsymbol{B}^m \) and \( \boldsymbol{\delta}(\boldsymbol{B}) = \boldsymbol{I} - \boldsymbol{\delta}_1 \boldsymbol{B} - \cdots - \boldsymbol{\delta}_r \boldsymbol{B}^r \) are two matrix polynomials of finite orders \( m \) and \( r \), respectively, and \( \boldsymbol{w}_i \) and \( \boldsymbol{\delta}_i \) are \( k \times k \) real matrices with \( k \) being the dimension of \( \boldsymbol{z}_t \). Here we require that \( \boldsymbol{w}(\boldsymbol{B}) \) and \( \boldsymbol{\delta}(\boldsymbol{B}) \) are left co-prime. A special case of Eq. (4.8) that is useful in application is

```
\( \boldsymbol{z}_t = (1 - \delta \boldsymbol{B})^{-1} \boldsymbol{w}_0 I_t^{(h)} + \boldsymbol{y}_t, \)
```

in which the impacts of the unusual event on each component of \( \boldsymbol{z}_t \) decay at the same rate.

# 4.1.2 Intervention with Step Functions

In some applications, an unusual event can permanently affect the time series under study. For instance, consider the market share of Brand A of a product, and a big sale promotion by its manufacturer at time index \( t = h \). If the promotion was announced prior to the effective time of sale promotion, consumers might delay any purchase in an anticipation of a price drop. Then, they purchased a large quantity of Brand A during the promotion period, resulting in a drop in sales immediately after the promotion was over. In theory, such a big promotion may permanently change the market share of Brand A. To describe such a complex market responses, it would become harder to use the indicator variable \( I_t^{(h)} \). Instead, the step function of Eq. (4.2) could be used to simplify the model. A candidate model for such an intervention is

```
\( z_t = (w_{-1}F + w_0 + w_1 B) S_t^{(h)} + y_t, \)
```