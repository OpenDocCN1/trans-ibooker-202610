```
x = np.linspace(0,50,num=51)
ax2.plot( x, st.binom.pmf(x,n=250,p=0.1), 'bo')
ax2.vlines(x, 0, st.binom.pmf(x, n=250, p=0.1), color='b', lw=2,
ax2.set_xlabel("Number of successes")
ax2.set_title("Binomial distribution: n=250, p=0.1")
```

Figure 5.1. For large n or p,p50%,B(n,p) is close to normal

![](bbox=[0.13, 0.27, 0.75, 0.72])

When the number of trials is small or the probability of success is close to 0 or 1, on the other hand, the binomial distribution isn’t so closely approximated by a normal distribution. For example, here are the