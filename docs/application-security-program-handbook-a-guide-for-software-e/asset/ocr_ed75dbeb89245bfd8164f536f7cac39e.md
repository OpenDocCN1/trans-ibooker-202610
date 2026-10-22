they don’t need the data. Therefore, limiting the collection of this sensitive data is the best way to secure it. However, organizations and application development teams will err on the side of collecting more data because you may need it at some time, and storage is cheap.

Some Questions that you need to ask regarding data:

+   - Do you need to store that data?
+   - Do you have regulations or contracts that require to maintain the data?
+   - Can you properly classify this data?
+   - Can you provide the appropriate level of protection for the collected data?
+   - Do you know when you no longer need it and can destroy it?

If you are comfortable with answering these questions in the affirmative, then you are on the right path. If not, then follow the previous recommendations of classifying, inventorying, and encrypting the data.

EXERCISE If you currently work in a development team or a team that collects data. Take a look at the database schema identify at least one set of data like a column in the database that contains sensitive information. Ask yourself or your team whether that data that is being collected is imperative to the operation of the application and whether not having the data would impact the application. The answer might be yes, in fact odds are it will be yes, but asking these questions regularly will get you into the habit of questioning whether you need to retain sensitive information.

# 2.3 Availability

Confidentiality takes the limelight when it comes to the CIA Triade primarily because you can get the biggest bang for the buck by properly encrypting data. Availability sounds like something that is more for IT Operations or site reliability engineering (SRE) who are primarily concerned with the uptime of the systems. The reality is that availability can be critical for applications where being down can be a matter of life and death. It’s not hard to imagine scenarios where this is evident. Hospitals, emergency services, critical infrastructure, and so forth have high demands for uptime.

Not all uptime demands are for public safety. Organizations that are solely an online retailer stand to lose money should their systems be unavailable. In early 2021 Amazon, the online retail giant, was taking in roughly $830,000 per minute. It’s pretty clear that an outage at Amazon would cost the company millions of dollars depending on how long it persisted.

You may not be running Amazon or a hospital but make no mistake that application uptime is still important for the organization and does have a financial impact. Organizations regularly report on their uptime, and it is a critical metric that needs to be met. Most organizations have contractual obligations that require them to meet Service Level Agreements (SLA) otherwise penalties will be incurred. Usually in the form of monetary compensation to the clients.