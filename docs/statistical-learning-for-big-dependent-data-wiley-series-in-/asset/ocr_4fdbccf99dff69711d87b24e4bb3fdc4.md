![](bbox=[0.15, 0.09, 0.85, 0.41])

**Figure 5.3** Average distances for the Taiwan AirBox Data.

```
> if(d==0) R[i,j,s]=1 else R[i,j,s] <-ACM[d,i]       }       }
>DetR=matrix(0,k,1)
>for (i in 1:k){
>     DetR[i]=det( R[i,] ))
>Mdis=matrix(0,k,k)
>for (i in 1:k){
>   for (j in 1:k){
>    Mdis[i,j]=abs (DetR[i] ^ (1/nl) -DetR[j] ^ (1/nl)) }}
>Md=Mdis [Mdis>0]
>hist (MdM)
>max(MdM)
>arrayInd (which.max(Mdis) ,dim(Mdis))
>mD=apply (Mdis,1,mean)
>plot (mD, main="Average distances determinant")
```

As a second example, we use the electricity price data in the file Pelectricity1344.csv. The data are the weekly series of electricity price each hour of each day during 678 weeks in eight regions of New England (NE), the United States, and we have 1344 series. Note that here k > T. The series correspond to each of the seven days of the week, at each of the 24 hours of the day, and for one of the eight regions in NE. That is, 7 x 24 x 8 = 1344. Using the same R commands, Figures 5.6 and 5.7 are obtained. The plots are similar between the two data sets, but their scales indicate that the electricity prices appear to be more homogeneous than the PM2.5 measurements.