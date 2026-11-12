![](bbox=[0.27, 0.1, 0.73, 0.38])

Figure 9.13 Hovmöller plot for the longitude of the sea surface temperature anomalies (°C) from January 1996 to December 2002.

plot provides certain information of the data, such as the anomalies seem to move from east to west for a given year, it fails to reveal many other features of the data. In fact, there are many ways to explore the data. First, they can be regarded as a 2520-dimensional time series with 84 observations (7 years). Second, they can also be regarded as a 84-by-30 matrix-variate time series with 84 observations. Our goal of this section is to study such S-T data.

# 9.6.1 Basic Properties

Consider the S-T process {Z(s, t)|s ∈ D; t ∈ Dt}, where D ⊂ Rd with d > 0 and Dt ⊂ R. In practice, we only observe data at a finite number of locations, say {si|i = 1, ..., m}, and a finite number of time points, say {tj|j = 1, ..., T}, where m and T are positive integers. For convenience, we write Z' tj = [Z(s1, tj), ..., Z(sm, tj)]′ and Z(si) = [Z(si, t1), ..., Z(si, tT)]′. That is, Z tj is the m-dimensional vector of observations at time tj and Z(si) is the T-dimensional vector of observations at location si. We start with some basic concepts. Associated with the observed process Z(s, t), there is an underlying S-T process Y(s, t), i.e. Z(s, t) = Y(s, t) + ϵ(s, t) with ϵ(s, t) being iid measurement errors.

Let D be a domain of interest. For spatial processes, D ⊂ Rd. For time series, D is the set of integers if time is discrete and D ⊂ R if time is continuous. For S-T processes, D ⊂ Rd × R or D ⊂ Rd × {..., -1, 0, 1, ...}. A function {g(u, v)|u, v ∈ D} defined on D × D is said to be nonnegative-definite if for any complex numbers {bi|i = 1, ..., m}, and any locations {ui|i = 1, ..., m} in D, and any positive integer m, we have

```
∑ m ∑ m bi b̄j g(ui, uj) ≥ 0, (9.62) i=1 j=1
```