# Conversions and Cancellations

Some commitment-based discount programs offer the ability to convert commitments or exchange them for another type. AWS Convertible reservations offer lower discount rates than reservations that cannot be exchanged, but allow users the option of exchanging the reservation for a different family or a different size (if ISF does not apply). The ability to convert reservations lowers the risk of ending up with reservations that don’t match your resource usage. If resources get updated so they no longer match, you can exchange the reservation for one that does. The ability to exchange or convert reservations is typically restricted to the region in which they were originally purchased.

Keep in mind also that converting RIs from one type to another requires analysis and work on the part of the FinOps team, so consider this cost in the overall equation when considering your options.

It should be noted here that AWS SPs, as of this writing, offer the same discounts that AWS RIs offer, but with convertibility built in. A compute SP, for example, covers compute instances in multiple regions and in any family without requiring any analysis or conversion to modify them, at the same discount as a convertible RI in one region for one type of family. SPs are discussed further in the section on AWS.

Azure does not offer convertible RIs, but it does allow users to cancel and exchange a certain dollar value of reservations each year. Sometimes this entails a fee, and usually it also requires the new reservation to start the clock over again from the time of the exchange. Azure SPs are now available as an alternative to reservations and function similarly to AWS SPs, offering much more flexibility than Azure RIs.

Google Cloud offers the ability to split or merge commitments. Splitting allows you to redistribute resources across your projects or organization. Merging commitments makes it easier to manage the discounts as a single entity with one expiration date. Google Cloud also now offers Flexible CUDs, bringing similar flexibility as SPs at AWS or Azure.

In general, the conversions you make must be equal or greater in value and duration. So if you lower your usage of the cloud service provider, you can’t exchange reservations if doing so results in a lower overall commitment.

One additional option AWS users have is the ability to sell certain types of reservations with a significant amount of time remaining on a secondary marketplace. Typically this can only be done with Standard, or nonconvertible, RIs with more than half their life remaining.