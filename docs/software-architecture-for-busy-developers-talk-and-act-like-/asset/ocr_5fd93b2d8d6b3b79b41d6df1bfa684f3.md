Many other scenarios could be crafted, but the preceding six scenarios are among the most relevant. We skipped the performance aspect since there is no real hard constraint on that one, and we left out security.

# Identified sensitivity points

Out of the scenarios listed in the previous section, we can infer a few sensitivity points, as follows:

+   - **SP1**: The availability of the upload channel *is sensitive* to the amount and duration of operational maintenance.
+   - **SP2**: The availability of the upload channel *is extremely sensitive* to the level of redundancy.
+   - **SP3**: The scalability and, especially, the maximum ingress throughput *is sensitive* to the number of concurrent upload channels.
+   - **SP4**: The usability *is sensitive* to the upload method.
+   - **SP5**: The interoperability *is sensitive* to the hosting platform location.

Now we have our list of scenarios and a few sensitivity points, let's try to respond with some architectural approaches and evaluate their impact. We should see trade-off points emerging after a first analysis of the proposed architectures.

# Architectural approaches

Before diving into different approaches, let's look at the following simplified diagram showing the different components and the flow:

![](bbox=[0.165, 0.75, 0.835, 0.76])

From left to right, the client system sends data files to the upload channel. Files are picked up by our data services for data cleansing and data wrangling. The resulting data is routed to different systems according to the nature of the input data. Note that we completely left out the integration with the other systems, which, in reality, would certainly have a big impact on the architecture. Let's focus on the data ingestion and data handling aspects.