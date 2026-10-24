+   - Special offers
+   - Coupons
+   - Sales records
+   - Invoice
+   - Payment

Figure 4.2 shows a map from functionalities to business capabilities. The map is a logical one, in the sense that it shows which business capabilities are needed to implement each function, but it doesn’t indicate any direct technical dependencies. For instance, the arrow from prepare invoice to coupons doesn’t indicate a direct call from some prepare invoice code in a client to a coupons microservice. Rather, the arrow indicates that in order to prepare an invoice, coupons need to be taken into account, so the prepare invoice function depends on the coupons business capability.

![](bbox=[0.175, 0.367, 0.832, 0.727])

Finding the business capabilities in real domains can be hard work and often requires a good deal of iterating. The list of business capabilities isn’t a static list made at the start of development; rather, it’s an emergent list that grows and changes over time as your understanding of the domain and the business grows and deepens.