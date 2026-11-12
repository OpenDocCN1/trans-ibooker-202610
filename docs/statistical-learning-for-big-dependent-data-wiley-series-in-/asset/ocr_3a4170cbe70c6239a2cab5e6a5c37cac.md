![](bbox=[0.15, 0.095, 0.85, 0.38])

Figure 5.27 Three of the projection directions computed to find clusters for the Taiwan Air-Box data with the average daily measurements as variables.

# R commands for clustering via projection.

```
> par(mfrow=c(1,1))
> sall=ClusKur(Data) # Data is the data matrix from Example 5.4.
> plot(sall$lbl,main="AirBoxes groups,-1 are outliers of these 4 groups")
> bb=ClusMain(Data)
##The columns of matrix pp are the projections of the data in the 31
##directions of largest and the 31 of smallest kurtosis
> pp=Data\%*\%t(bb$V
> par(mfrow=c(1,3))
##Some projections are shown
> plot(pp[,1], main="projection on maxima kurtosis")
> plot(pp[,9], main="projection on large kurtosis")
> plot(pp[,32], main="projection on minima kurtosis")
> memb=sall$lbl
> g1u=which(memb==1)
> g2u=which(memb==2)
> g3u=which(memb==3)
> g4u=which(memb==4)
> g0u=which(memb==-1)
## The number of series in each group are shown
> ng1=length(g1u)
> ng1
[1] 154
> ng2=length(g2u)
> ng2
[1] 130
> ng3=length(g3u)
> ng3
[1] 76
> ng4=length(g4u)
```