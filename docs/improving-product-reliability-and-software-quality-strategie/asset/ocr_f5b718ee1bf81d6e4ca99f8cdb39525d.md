# 6.4 Availability

When a system fails and becomes dysfunctional to the customer, the time it takes to rectify the problem is critical (MTTR). Long repair times take a lot of productivity out of the usefulness of a continuously operating product, so having short MTTRs is a critical availability issue. Simply stated, static availability may be expressed as

Availability = MTBF/(MTBF + MTTR)

Example: With an MTBF of 10 000 hours and a 10-hour MTTR:

Availability = 10,000/(10,000 + 10) = 10,000/10,010 = 0.999 or 99.9%

The reader can see that bigger MTBFs can drive availability to nearly 100%. Small MTBFs drive availability down, even with the same MTTR figure. A customer who experiences frequent outages needs to correct the problem quickly. If the MTTR is very short, less than one hour, the impact is relatively small. But when the MTTR is long, such as several days, the availability can become very small. This customer will be unhappy. It is in the manufacturer’s best interest to have high MTBFs, but they must also drive toward short MTTRs. Because parts availability is a major contributor to bringing a product back into operation, addressing this issue early in the reliability improvement planning is recommended.

Short availability is not a savior when the MTBF is low. Some manufacturers produce complex equipment that requires skilled, on-site, service personnel. If they can fix everything in a few hours on average, but there are many units operating, they will be fighting an uphill battle. For illustration, we will consider a customer who has 100 units in operation. With an MTBF of 1000 hours and a 4-hour MTTR, there will be a new failure every 10 hours on average. Service will have to fix these failures that are expected to accumulate at about 2.4 failures per day if the equipment runs 24/7 hour. The repair person can be expected to fix an average of two units per 8-hour shift and have another 0.4 units to repair before going home. The repair service may require more service personnel. Logistical constraints often drive MTTR requirements even higher. Long procurement from a few hours to a few days or even weeks may be observed. With an average of 2.4 accumulating failures per day, it is easy to see that the repair team will be quickly overwhelmed.

As failures appear, customers may have reduced output from their production. The customers will consider other manufacturers the next time they select equipment for the production line.

Shorter MTTRs translate into service capabilities that can correct field failures quickly. This means trained service personnel and readily available spare parts. Service/repair personnel have to be available to the customer almost immediately. This can be accomplished in eight ways:

+   1. On-site manufacturer service personnel
+   2. Customer trained service personnel
+   3. Manufacturer training for customer service personnel
+   4. Easy-to-use service manuals