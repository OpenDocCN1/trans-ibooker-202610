# System MTBF

![](bbox=[0.19, 0.18, 0.84, 0.48])

Figure 24.3 Reliability growth chart.

reliability increases because another failure mechanism has been designed out. This increasing product reliability (for new products that are produced) is tracked using reliability growth. But how do you know if the progress to improve product reliability is acceptable? Are the efforts being made to improve product reliability making a difference to product reliability? Are the reliability improvements being made quick enough to meet the business needs?

The reliability growth curve displays the effect that FRACAS is having on improving product reliability. This gives a snapshot of the current product reliability along with the rate at which it has improved. By tracking reliability growth, a decision can be made regarding the effectiveness of past reliability efforts. However, it is important to determine if the improvements in reliability are occurring too slowly to meet the business needs. Reliability growth can also be used to estimate what the future reliability of the product will be. Knowing this, a decision can be made regarding whether the product design is maturing at a rate fast enough to meet the business needs. An example of this is shown in Figure 24.4.

The future reliability growth can be estimated using a Duane curve. The Duane curve provides an estimate of the time required to achieve the reliability goal based on the current rate of reliability growth. The reliability growth rate is shown in Figure 24.5. Duane observed that the reliability improved on the basis of cumulative MTBF (θc) plotted against the total time, on a log–log scale. This plot could be represented as a straight line using

```
Log θc = log θo + α(log T − log To)
```