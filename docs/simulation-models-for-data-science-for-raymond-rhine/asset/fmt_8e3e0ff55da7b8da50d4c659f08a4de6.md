
Simulation is a better approach to understanding the nature of the dealth spiral. It allows us to model how customer make decisions about cost vs risk tolerance as well as modeling the changing compositon of the pool of insured and how that affects costs. Simulations can help us identify thresholds, such as when the death spiral becomes irreversible becaue the rate of customer loss is so high. We can also use simulations to test different assumptions about price sensitivity and introduce new product features, like wellness incentives that can influence the cost dynamics for the insurer.

Another example of a feedback loop occurs with marketing campaigns. Changes in the way a retailer uses different marketing channels may show initial success. We may measure a 5% increase in sales revenues and rightly assume the new marketing strategy is the cause. If we stop at that level of analysis we’ll miss the response of our competitors. If, instead of measuring the change in revenue at one point in time, we built a model based on system dynamics, we could get a better understanding of how the makerting changes create unanticipated outcomes through reinforcing loops, such as competitors dropping thier prices.

## Delays

Simulation modeling helps us understand complete business systems rather than just a component within in a larger system. The immediate impact of a change in aa point of sale systems might be obvious: we reduce the number of abandonded carts for an online retailer. To understand the full impact of the change we should understand how it affects other components and aspects of the wider business system. For this, we need a way to model outcomes that unfold over time and across different componets of a system.

Let’s look at a basic A/B test for an online retailer. The retailer has streamlined the checkout process to reduce the number of pages a customer has to click through to complete a purchase. After a couple of weeks, the retailer finds that there is a 15% increase in the number of compeleted checkouts relative to the number of checkouts started. Great, the retailer takes the win and continues to use the streamlined checkout process.

A couple of months after the change, the retailer notices an increase in the return rate. Prior to the check out change, the commpany had a 5% retrun rate. Since the change, that rate has been increasing and is now approaching 10%. The retailer starts analyzing data about sales before and after the change and finds differences in the two populations (before and after change). After the change, there is an increase in the number of incorrect shipping addresses and wrong order quantities. This leads to higher labor costs to process returns, increased shipping costs, as well as lower customer satisfaction. Eventually, the retailer will see customer ratings decline and eventually customer churn and a loss in net promoter score.

## Non-linear Effects and Thresholds

A tell tale sign of a complex system is a sudden change in the state of the system that is not predicted by past behavior. As water cools, it maintains its liquid state until it reaches the freezing point and suddenly becomes solid. There is no linear progression from a more liquid state to a more solid state, there is a sudden transition. These transitions occur in a variety of systems.

### Sudden Phase Transitions at Critical Points

Queueing systems are widely used to model processes in which a request for a service arrives, possibly waits, receives a service, and then departs. If requests arrived at regular 10 minute intervals and always 9 minutes to process then no waiting would occur. When there is variability in the time requests arrive or in the time required to process, then queues will occur. Of course, we could scale our processing so that no matter how frequent the requests are, we can keep up. This kind of peak-demand sizing is wasteful and inefficient. The math behind queueing indicates that as queue lengths approach 80-85% utilization, we can expect a sudden increase in processing time, regardless of the the historical average of processing time was.

![](assets/figure_2-2_queue_length_vs_utilization.png)

### Diminishing and Accelerating Returns

Linear thinking comes naturally. If we increase our workforce by 10%, we’ll increase our output by 10%. With the help of statistics, we can often find factors, like revenue per salesperson or cost per unit produced. Combine these metrics with some basic spreadsheet skills and we can make all kinds of linear projections. This kind of reassoning sometimes works out when we restrict ourselves to narrow ranges and ideal circumstances. In real world scenarios with all their comlexity we often find relationships between inputs and outputs have non-linear relationships.

Consider the cost of acquireing new customers. A new direct-to-consumer retailer has been advertising for six months. Based on the initial data, the company determines that the average cost to acquire a customer is $50. Executives approve a budget of $250,000 for customer acquistion with the expectation that this will increase the number of customers by 5,000. After 2 months and an exhausted budget, the company has acquired and additional 2,800 customers. What they failed to appreciate is that the early customers where the easiest to acquire. They were the ones most interested and engaged with the company while the customers acquired later were less responsive to advertisments.

### Complex Interactions between Resources

When we need multiple resources to produce some good, it can be challenging to optimize the allocation of those resources. We might think, adding 10% more of one resource will increase our output by 10% (there is that linear thinking again). Adding 10% might increase the output of the one component but how does that increase affect other components? If a manufacturer has 10 produciton lines and adds one more, they will likely get 10% increase in the number of units produced. That does not seem unreasonable. The problem is that we stop thinking about the process too soon. What happens to those additional 10% of units produced? If they are sent to a quality control inspection station, then we risks creating a backlog of work for inspectors and this in turn creates a queue of work to be done.

### Emergent Properties from Component Interactions

Emergence is one of the most challenging aspects of working with complex systems. Emergence occurs when a behavior or characterisitc appears in a system that could not be predicted by analyzing the patterns of interaction of system components. When emergence occurs, a system is not simply the sum of its parts, it is something more. This phenomenon is ever present in biology. Cardiologist can explain how a heart muscle works in terms of responding when a neighboring cell exceeds a certain voltage, causing the cell to contract, enter a period where it cannot fire (refactory period) and then recover. Millions of cells in our hearts establish a dominant rhythm that synchronizes the firing process at the organ level in spite of the fact that individual cells have different firing rates. Researchers have found that two hearts with identical cellular properties can have different deviations from normal firing based on characteristics of tissue and prior states of the organ. To understand firing at the organ level, we have to reason about collections of cells or tissues because that is where the behaviors we are interested in emerge.

## Capacity Constraints and Resource Contention

The past is not always a good indicator of the future, and this is especially true when comparing a system during normal operations and the same system under stress. If we try to generalize principles from normal times that apply during high-stress periods, we are likely to miss the need to make trade-offs as resources become constrained.

Hard limits that invalidate extrapolation
:
