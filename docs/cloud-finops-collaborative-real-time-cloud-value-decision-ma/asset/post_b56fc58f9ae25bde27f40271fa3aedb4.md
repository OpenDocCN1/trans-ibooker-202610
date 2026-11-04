
Lambda resources, that commitment will cover *all* eligible usage in that account first. So you may find that your SPs are covering substantial amounts of Lambda serverless usage before they are flowing to other accounts where the savings would be at a much higher percentage (and subsequently more monetary savings).

So your strategy for where to purchase your commitments should consider this.

One approach we have seen among some more advanced customers and in use by some vendors who offer commitment management *as a service* is the use of so-called *sidecar* accounts: dedicated member accounts which *only* contain commitments, no usage at all. Buying commitments in a specially designated account for them eliminates the need for the FinOps team to access the management account (again, always a good thing to limit access here), and gives customers the ability to potentially move the commitments around between organizational units to have them apply differently over time.

This can be particularly valuable as an approach for managed service providers or companies that routinely buy or sell parts of the business through merger or acquisition. If commitment-based discounts are attached to an account that is operating a system that is being sold, the sidecar account with the commitment can simply be repositioned in the organization as those accounts are moved to a new management account in its new home. Had the commitments been purchased in those accounts, they would have moved with the resources.

Commitments might all be purchased in one sidecar account or in several to help influence where coverage might apply. Not every organization needs to obtain this level of control, but if you do, this can be an important aspect of your overall commitment strategy.

## Step 4: Purchase Regularly and Often

Cloud is designed for just-in-time purchasing. You shouldn’t be running infrastructure or purchasing capacity (i.e., buying commitments) in a data center fashion.

Years ago, AWS had graphs like those in Figure 18-3 on their home page. The graph on the left showed the way you had to buy capacity in the data center and hardware world, fraught with constraints and long lead times for hardware. The graph on the right shows the ideal way to run infrastructure in the cloud: you spin it up just when you need it. The same is true for purchasing commitments.

![](img/4d4736bc293fd892d1da81802d0f9520_320_0.png)

Figure 18-3. Diagram AWS used to host on their website

## Step 5: Measure and Iterate

We have heard many horror stories of companies that have purchased commitments incorrectly. To reduce the risk of this common mistake, make sure your initial purchases are small. You should have carefully set metrics with thresholds. These can start with simple measures, such as what percentage of cloud spend that should be covered with a discount are covered with a discount program. Conversely, you should track the amount of waste (or vacancy) of existing commitments to ensure they are used to their maximum extent. For more on the metrics to use in managing commitments, we cover metric-driven cost optimization in Chapter 22.

One frequent cause of overpurchasing of commitments or purchasing incorrectly is the tools that recommend what types of commitments to buy! Commitment recommendations, like any of the automated processes performed by FinOps tools and platforms (from the cloud providers or from third parties), are primarily based on expert analysis of your past usage. To the extent your future usage looks exactly the same as your past usage (unlikely) these forecasts can be very accurate. However, you have the power to look back at a variable time period to generate recommendations.

One contributor told us the story of purchasing a large quantity of AWS RIs to cover a large amount of on-demand cost. The purchase began to make a measurable difference immediately and the business was supportive of increasing the coverage by making another incremental purchase of RIs only two weeks later. However, the tool being used to generate recommendations had a look-back period of 30 days, so it was generating recommendations based on two weeks with that new RI coverage and two weeks without it, which caused it to recommend far too many RI purchases for the second go-round.

Purchasing commitments is about managing risk; the more commitments you make, the more risk you carry of having them become underutilized. The better your forecasting capability—see Chapter 13—becomes, the more comfortable you will be at making larger commitments safely.

![](img/4d4736bc293fd892d1da81802d0f9520_321_0.png)

There is a lot to take into account as you purchase commitments, so take your time to analyze the impact of your purchases, and be sure of each incremental step toward higher coverage. As your experience and confidence in your process grow, you can increase both the frequency and the size of your purchases.

## Step 6: Allocate Up-Front Commitment Costs Appropriately

Chapter 4 introduced the matching principle, which holds that expenses should be recorded in the period in which the value was received. In AWS, when a commitment is paid up front, even partially, it applies a discount to your usage over a long period (one or three years), and you need to distribute the up-front payment over the term. Distributing the up-front payment is called amortization. With AWS, you are given the option of paying all, some, or nothing up front. You receive a higher discount for the commitment the more you pay in advance. With Azure you have the option of paying up front or monthly, with no difference in price. And with Google Cloud, you pay monthly, so there is no amortization to track.

Deciding whether or not to pay up front requires two major considerations:

### Accounting

The first is the impact to your accounting processes. Chapter 17 highlighted that commitment discounts will apply randomly across your cloud usage. If you need to allocate part of the up-front payment to one team and another part to another team, you will be required to track exactly where the commitment applied its discounts and then amortize accordingly.

### Granularity

You can’t just divide a commitment into equal monthly parts. Not all commitments start on the first hour of the first day of the month, and February is 10% shorter than March, so the amortization requires a more granular approach.

We will cover who pays for commitments later in the chapter.

While commitments save money, they can require available cash to apply to up-front fees. For some organizations, this isn’t an issue, while for others it is prohibitive to business activities. Some organizations need to spend money inside particular time periods, such as when a budget must be consumed before the end of the fiscal year.

Where the money is available, we see experienced FinOps practitioners considering the weighted average cost of capital (WACC) and the net present value (NPV) of cash. These calculations determine whether applying the business capital to up-front commitments is a good investment versus other possible investments. While there's a lower discount for not paying up front, investing some money somewhere else in the business might have better financial outcomes.

Think of the premium charged by AWS for each of the No upfront and Partial upfront purchase options as being equivalent to an interest charge for the vendor having lent the non-up-front portion of the purchase, and think of that interest charge as carrying an annual interest rate. We refer to this as the implied interest rate of the No upfront or Partial upfront purchasing options.

Whether a consumer of cloud resources should choose the All upfront or alternative payment options depends on whether the consumer's WACC is lower or higher than the implied interest rate charged by the service provider to defer payment of the commitments.

A consumer's WACC (sometimes called the internal cost of capital) is the cost of financial capital to the cloud consumer in the form of interest costs for the company's debt, and the cost of the company's equity capital. The process of determining which cost is greater involves calculating the NPV of the purchase cash flows for each of the All upfront and No upfront scenarios, respectively.

The process begins by determining the consumer's WACC. Most enterprises will have a standard WACC in use by the finance department for decisions of this type, and in general you can take this figure as a given.

If the discount rate calculated—using techniques like Excel's Goal Seek—exceeds the client's WACC, the client should purchase All upfront commitments. If the figure is lower than the consumer's WACC, then the implied interest rate of the No upfront commitment purchase option will be less expensive to the consumer than the AWS's All upfront option.

The mix of commitments purchased changes this imputed interest rate for each purchase, so this can be done for each purchase decision, or it can be done for representative purchases to help a customer set a policy of buying no, partial, or all up-front RIs or SPs, and only revisited occasionally. Note this applies only to AWS commitments as they are the only ones that have a variable discount when paying up front.
