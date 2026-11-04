Lambda resources, that commitment will cover *all* eligible usage in that account first. So you may find that your SPs are covering substantial amounts of Lambda serverless usage before they are flowing to other accounts where the savings would be at a much higher percentage (and subsequently more monetary savings).

So your strategy for where to purchase your commitments should consider this.

One approach we have seen among some more advanced customers and in use by some vendors who offer commitment management *as a service* is the use of so-called *sidecar* accounts: dedicated member accounts which *only* contain commitments, no usage at all. Buying commitments in a specially designated account for them eliminates the need for the FinOps team to access the management account (again, always a good thing to limit access here), and gives customers the ability to potentially move the commitments around between organizational units to have them apply differently over time.

This can be particularly valuable as an approach for managed service providers or companies that routinely buy or sell parts of the business through merger or acquisition. If commitment-based discounts are attached to an account that is operating a system that is being sold, the sidecar account with the commitment can simply be repositioned in the organization as those accounts are moved to a new management account in its new home. Had the commitments been purchased in those accounts, they would have moved with the resources.

Commitments might all be purchased in one sidecar account or in several to help influence where coverage might apply. Not every organization needs to obtain this level of control, but if you do, this can be an important aspect of your overall commitment strategy.

# Step 4: Purchase Regularly and Often

Cloud is designed for just-in-time purchasing. You shouldn’t be running infrastructure or purchasing capacity (i.e., buying commitments) in a data center fashion.

Years ago, AWS had graphs like those in Figure 18-3 on their home page. The graph on the left showed the way you had to buy capacity in the data center and hardware world, fraught with constraints and long lead times for hardware. The graph on the right shows the ideal way to run infrastructure in the cloud: you spin it up just when you need it. The same is true for purchasing commitments.